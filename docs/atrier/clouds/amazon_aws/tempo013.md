---
title: AWS Batch : 5 minutes pour monter votre cluster de calcul
description: 
published: true
date: 2023-06-11T14:11:28.532Z
tags: 
editor: markdown
dateCreated: 2023-06-11T14:11:28.532Z
---

# AWS Batch : 5 minutes pour monter votre cluster de calcul

Ayant récemment travaillé sur le service AWS Batch, et ayant été bluffé par la simplicité de mise en œuvre, je relève le défi de vous présenter ce service en 5 minutes...

Dans la documentation officielle AWS, le service est décrit comme :

“AWS Batch permet aux développeurs, aux scientifiques et aux ingénieurs d'exécuter aisément et efficacement plusieurs centaines de milliers de tâches de calcul par batch sur AWS. AWS Batch alloue dynamiquement une quantité optimale et un type de ressources de calcul (par exemple, des instances optimisées pour la mémoire ou CPU) en fonction du volume et des besoins en ressources spécifiques des tâches par lots soumises. Avec AWS Batch, il est inutile d'installer et de gérer les clusters de serveur ou les logiciels de calcul par batch que vous utilisez pour exécuter vos tâches, ce qui vous permet de vous concentrer sur l'analyse des résultats et la résolution des problèmes.”

Dans le contexte de mon client, nous devions lancé des calculs complexes de rapprochement de données sur de gros volumes. AWS Batch nous semblait être le service idéal pour effectuer cette tâche. Les traitements de rapprochement ont été codés sous forme de scripts python, embarqués dans des conteneurs. D’autre part, afin de limiter les coûts, nous avions décidé d’utiliser des “spots instances”.

Comment ça marche ? C’est très simple : AWS Batch s’articule autour de 3 ressources :

- le “**compute environment**” : C’est votre unité de calcul. C’est à l’intérieur de cette unité de calcul que les traitements seront exécutés.
- Les “**job queues**” : Vous pourrez en définir plusieurs et affecter des priorités. Ces priorités serviront à AWS pour affecter tout ou partie des ressources disponibles dans le “compute environnement”.
- Les “**job definitions**” : votre job proprement dit. Pour ceux qui connaissent “ECS Fargate”, vous retrouverez les mêmes notions. Les 2 services sont d’ailleurs souvent comparés dans l’écosystème “data”. En définitive, AWS Batch encapsule la création d’un cluster ECS pour vous.

Rentrons dans le détail de l’implémentation (en terraform) :

Avant de pouvoir implémenter AWS Batch, nous avons besoin de 2 choses :

- 1 security group
- des permissions IAM

Comme toute ressource dans AWS qui va communiquer avec d’autres ressources du VPC, vous devrez définir un **security group** :

```yaml
#######################################
# Create the Security Group associated to the BATCH
#######################################
resource "aws_security_group" "sg-batch" {
  name        = "sgr-${var.environment}-my-batch"
  description = "Allow Private subnets access batch"
  vpc_id      = var.vpc_id
  tags = {
    "Name" = "sgr-${var.environment}-my-batch"
  }
}
```

Et bien sûr ouvrir tout type de flux légitime (autoriser votre batch à écrire sur votre EFS, à joindre votre base etc …)

En terme de permissions et de ressources IAM, 4 rôles sont nécessaires (!!) :

“**ECS Task Execution Role**” : Les permissions nécessaires pour l’exécution du conteneur.

```yaml
######################################################################
# ECS Task Execution Role
######################################################################
resource "aws_iam_role" "batch_ecs_task_execution_role" {
 name = "role-${var.environment}-${var.application}-batch-ecs-task-execution-role"
 
 assume_role_policy = <<POLICY
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Action": "sts:AssumeRole",
         "Effect": "Allow",
         "Principal": {
            "Service": ["ecs-tasks.amazonaws.com","ec2.amazonaws.com"]
         }
       }
    ]
}
POLICY
}
 
# Attach the mandatory policy “AmazonECSTaskExecutionRolePolicy
resource "aws_iam_role_policy_attachment" "batch_ecs_task_execution_role" {
 role       = aws_iam_role.batch_ecs_task_execution_role.name
 policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
}
 
# Attach any permissions needed for your batch (readSecrets, getParameterStore, etc, ..)
resource "aws_iam_role_policy_attachment" "additional-policy-attachment" {
 role       = aws_iam_role.batch_ecs_task_execution_role.name
 policy_arn = aws_iam_policy.batch-additional-policy.arn
}
```

“**EC2 Instance Profile & Role**” : Les permissions des instances EC2 sur lesquelles les conteneurs sont lancés.

```
######################################################################
#BATCH - INSTANCE ROLE
######################################################################
data "aws_iam_policy_document" "assume-role-ec2-policy" {
 statement {
   actions = ["sts:AssumeRole"]
 
   principals {
     type        = "Service"
     identifiers = ["ec2.amazonaws.com"]
   }
 }
}
 
#ROLE
resource "aws_iam_role" "batch_instance_role" {
   name = "role-${var.environment}-${var.application}-batch-instance"
   assume_role_policy = data.aws_iam_policy_document.assume-role-ec2-policy.json
  
   tags = local.common_tags
}
 
#INSTANCE PROFILE
resource "aws_iam_instance_profile" "batch_instance_profile" {
 name       = "role-${var.environment}-${var.application}-batch-instance-profile"
 role       = aws_iam_role.batch_instance_role.name
}
 
##Attach AWS Policy AmazonEC2ContainerServiceforEC2Role
resource "aws_iam_role_policy_attachment" "ecs_instance_role" {
 role       = aws_iam_role.batch_instance_role.name
 policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceforEC2Role"
}
```

Le “**Batch Service Role**” : Ce sera le rôle utilisé par le service AWS Batch pour interagir dans votre compte.

```yaml
######################################################################
# Batch Service Role
######################################################################
data "aws_iam_policy_document" "assume-role-batch-policy" {
 statement {
   actions = ["sts:AssumeRole"]
 
   principals {
     type        = "Service"
     identifiers = ["batch.amazonaws.com"]
   }
 }
}
 
resource "aws_iam_role" "batch_service_role" {
   name = "role-${var.environment}-${var.application}-batch-service"
   assume_role_policy = data.aws_iam_policy_document.assume-role-batch-policy.json
  
   tags = local.common_tags
}
 
# Attach AWS Policy AWSBatchServiceRole
resource "aws_iam_role_policy_attachment" "aws_batch_service_role" {
 role       = aws_iam_role.batch_service_role.name
 policy_arn = "arn:aws:iam::aws:policy/service-role/AWSBatchServiceRole"
}
```


Enfin, la gestion de votre **flotte d’instances Spot** :

```
# https://aws.amazon.com/getting-started/hands-on/run-batch-jobs-at-scale-with-ec2-spot/
 
# added linked service roles
resource "aws_iam_service_linked_role" "AWSServiceRoleForEC2Spot" {
 aws_service_name = "spot.amazonaws.com"
}
 
resource "aws_iam_service_linked_role" "AWSServiceRoleForEC2SpotFleet" {
 aws_service_name = "spotfleet.amazonaws.com"
}
 
## AmazonEC2SpotFleetRole ###
resource "aws_iam_role" "AmazonEC2SpotFleetRole" {
 name = "AmazonEC2SpotFleetRole"
 assume_role_policy = <<POLICY
{
   "Version": "2012-10-17",
   "Statement": [
       {
           "Action": "sts:AssumeRole",
           "Effect": "Allow",
           "Principal": {
               "Service": "spotfleet.amazonaws.com"
           }
       }
   ]
}
POLICY
}
 
resource "aws_iam_role_policy_attachment" "AmazonEC2SpotFleetRole" {
 role       = aws_iam_role.AmazonEC2SpotFleetRole.name
 policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonEC2SpotFleetTaggingRole"
}
```

OK. Le plus dur est fait. Ecrivons maintenant la task definition de notre batch (identique à une task definition d’un cluster ECS) :

```yaml
{
   "command": [],
   "environment": [
      {
        "name": "environment",
        "value": "${ENV}"
      },
      {
        "name": "BUCKET_NAME",
        "value": "${BUCKET_NAME}"
      }
   ],
   "secrets": [
      {
        "name": "MY_PASSWORD",
        "valueFrom": "${MY_PASSWORD_SECRET_ARN}"
      }
    ],
   "image": "${BATCH_IMAGE}",
   "jobRoleArn": "${JOB_ROLE_ARN}",
   "executionRoleArn": "${EXECUTION_ROLE_ARN}",
   "memory": ${BATCH_MEMORY},
   "vcpus": ${BATCH_VCPUS},
   "privileged": false,
   "mountPoints": [],
   "ulimits": [],
   "volumes": [],
   "retryStrategy": {
      "attempts": 2
   },
   "timeout": {
      "attemptDurationSeconds": 180
   }
}
```

Notez qu’on peut monter des volumes comme pour les tasks ECS.

Notez également que l'attribut "executionRoleArn" est à renseigner si vous avez des “secrets” à récupérer : il faudra alors associer les permissions nécessaires pour lire les secrets.

Assemblons le tout :

Le “**compute environment**” :

```yaml
resource "aws_batch_compute_environment" "compute-environment" {
 compute_environment_name = local.ce_name
 service_role             = aws_iam_role.batch_service_role.arn
 type                     = "MANAGED"
 
 compute_resources {
   instance_role       = aws_iam_instance_profile.batch_instance_profile.arn
   spot_iam_fleet_role = aws_iam_role.AmazonEC2SpotFleetRole.arn
 
   instance_type       = var.instance_type
   max_vcpus           = var.max_vcpus
   min_vcpus           = var.min_vcpus
   desired_vcpus       = var.desired_vcpus
   bid_percentage      = var.bid_percentage
   allocation_strategy = var.allocation_strategy
   security_group_ids  = [aws_security_group.sg-batch.id]
   subnets             = var.subnet_ids
   type                = "SPOT"
 }
 
 tags = merge(map("Name", local.ce_name), local.common_tags)
 
 lifecycle {
   ignore_changes = [
      compute_resources.0.desired_vcpus
   ]
  }
}
```

Le job queue et le job definition :

```yaml
resource "aws_batch_job_queue" "job_queue" {
 name                 = var.batch_job_queue_name
 state                = var.batch_job_queue_state
 priority             = var.batch_job_queue_priority
 compute_environments = [aws_batch_compute_environment.compute-environment.arn]
 tags                 = merge(map("Name", var.batch_name), var.tags)
}
 
resource "aws_batch_job_definition" "job-definition" {
 name                 = var.batch_job_definition_name
 type                 = "container"
 container_properties = templatefile("job-defintions/functionaltest.json",
 {
  JOB_ROLE_ARN           = aws_iam_role.batch_ecs_task_execution_role.arn
  EXECUTION_ROLE_ARN     = aws_iam_role.batch_ecs_task_execution_role.arn 
  REGION                 = var.region
  ENV                    = var.environment
  BATCH_IMAGE            = "busybox"
  BATCH_MEMORY           = 1024
  BATCH_VCPUS            = 1
  BUCKET_NAME            = "my-bucket"                                            
  MY_PASSWORD_SECRET_ARN = data.aws_secretsmanager_secret.rds_password.arn
 }
                                           
 tags                 = merge(map("Name", var.batch_name), var.tags)
}
```

L’ensemble du code est récupérable depuis mon github :

https://github.com/jparnaudeau/demo-aws-batch

Pour conclure :

AWS Batch n’est pas le service le plus connu dans les traitements de la “data”. On a d’ailleurs du mal à le situer dans cet écosystème.

L’un des principaux avantages d’AWS Batch par rapport à un cluster EMR transient (https://blog.ippon.fr/2019/04/16/cluster-spark-ephemere-avec-terraform-et-aws-emr/) est la facilité de prise en main.

Si votre équipe IT n’est pas à l’aise avec les technologies plus poussées comme EMR ou Lambda & Step Functions, la mise en œuvre d’AWS Batch sera un bon compromis. L’utilisation des spots instances vous permettra de réduire drastiquement vos coûts.

Notez néanmoins que l’utilisation des “spots instances” reste toujours assez déroutante les premières fois : en effet, quand votre traitement est interrompu car l’instance vous est retirée, cela surprend toujours un peu. Préférez donc des traitements “courts”, et des reprises sur erreur.

Vos traitements AWS Batch peuvent également être déclenchés depuis l’arrivée de fichiers sur un bucket s3. Je vous invite à lire le lien suivant :

https://medium.com/swlh/aws-batch-to-process-s3-events-388a77d0d9c2

Source : https://blog.ippon.fr/
