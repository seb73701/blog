# ARGO CD
----

## <i class="fa-solid fa-hashtag"></i> Information

![Logo](../../_media/apps/argocd/argocd_logo.png ':size=250 :no-zoom')


> <i class="fa-solid fa-quote-left"></i> **Argo CD** is a declarative, GitOps continuous delivery tool for Kubernetes.
> 
> **Argo CD** follows the GitOps pattern of using Git repositories as the source of truth for defining the desired application state. Kubernetes manifests can be specified in several ways:
>
> - kustomize applications
> - helm charts
> - jsonnet files
> - Plain directory of YAML/json manifests
> - Any custom config management tool configured as a config management plugin
>
> **Argo CD** automates the deployment of the desired application states in the specified target environments. Application deployments can track updates to branches, tags, or pinned to a specific version of manifests at a Git commit.
> 
> **Argo CD** is implemented as a kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the Git repo). A deployed application whose live state deviates from the target state is considered OutOfSync. **Argo CD** reports & visualizes the differences, while providing facilities to automatically or manually sync the live state back to the desired target state. Any modifications made to the desired target state in the Git repo can be automatically applied and reflected in the specified target environments.
> 
> Features
>
> - Automated deployment of applications to specified target environments
> - Support for multiple config management/templating tools (Kustomize, Helm, Jsonnet, plain-YAML)
> - Ability to manage and deploy to multiple clusters
> - SSO Integration (OIDC, OAuth2, LDAP, SAML 2.0, GitHub, GitLab, Microsoft, LinkedIn)
> - Multi-tenancy and RBAC policies for authorization
> - Rollback/Roll-anywhere to any application configuration committed in Git repository
> - Health status analysis of application resources
> - Automated configuration drift detection and visualization
> - Automated or manual syncing of applications to its desired state
> - Web UI which provides real-time view of application activity
> - CLI for automation and CI integration
> - Webhook integration (GitHub, BitBucket, GitLab)
> - Access tokens for automation
> - PreSync, Sync, PostSync hooks to support complex application rollouts (e.g.blue/green & canary upgrades)
> - Audit trails for application events and API calls
> - Prometheus metrics
> - Parameter overrides for overriding helm parameters in Git. <i class="fa-solid fa-quote-left fa-rotate-180"></i>


- <i class="fa-solid fa-globe"></i> **Site Officiel** : https://argoproj.github.io/cd/
- <i class="fa-solid fa-book"></i> **Documentation** : https://argo-cd.readthedocs.io/
- <i class="fa-solid fa-file-circle-question"></i> **Changelog** : https://github.com/argoproj/argo-cd/releases
- <i class="fa-brands fa-github"></i> **Github** : https://github.com/argoproj/argo-cd
- <i class="fab fa-blogger-b"></i> **Blog** : https://blog.argoproj.io/
- <i class="far fa-question-circle"></i> **FAQ** : https://argo-cd.readthedocs.io/en/stable/faq/
- <i class="far fa-calendar-alt"></i> **Live Demo** : https://cd.apps.argoproj.io/

---

## <i class="fa-regular fa-newspaper"></i> Articles

<details open>

</details>

---

## <i class="fa-solid fa-glasses"></i> Articles externes

<details>

- [Argo CD : Économiser ses deniers sans impacter la performance](https://blog.ippon.fr/2025/04/04/argo-cd-economiser-ses-deniers-sans-impacter-la-performance/)
- [ArgoCD, le meilleur ami du GitOps et de Kubernetes](https://fgtech.medium.com/argocd-le-meilleur-ami-du-gitops-et-de-kubernetes-25f2694c746)
- [3 patterns for deploying Helm charts with Argo CD](https://developers.redhat.com/articles/2023/05/25/3-patterns-deploying-helm-charts-argocd)
- [A Comprehensive Overview of Argo CD Architectures – 2024](https://codefresh.io/blog/a-comprehensive-overview-of-argo-cd-architectures-2024/)
- [A Fully Argo-Based Ecosystem For Continuous Delivery and Deployment](https://codefresh.io/blog/a-fully-argo-based-ecosystem-for-continuous-delivery-and-deployment/)
- [Applied GitOps with Argo CD](https://thenewstack.io/applied-gitops-with-argocd/)
- [Argo CD - GitOps Continuous Delivery for Kubernetes](https://www.youtube.com/watch?v=aWDIQMbp1cc&feature=youtu.be&t=1m4s)
- [Argo CD and Codefresh GitOps Security Updates 3/18/2024 – Preventing Brute-Force and Denial of Service](https://codefresh.io/blog/argo-cd-and-codefresh-gitops-security-updates-3-18-2024-preventing-brute-force-and-denial-of-service/)
- [Argo CD and Tekton: Match made in Kubernetes heaven](https://developers.redhat.com/blog/2020/11/03/argo-cd-and-tekton-match-made-in-kubernetes-heaven)
- [Argo CD Application Dependencies](https://codefresh.io/blog/argo-cd-application-dependencies/)
- [Argo CD Best Practices](https://codefresh.io/blog/argo-cd-best-practices/)
- [Argo CD: Applying GitOps Principles To Manage Production Environment In Kubernetes](https://youtu.be/vpWQeoaiRM4)
- [Automate CI/CD on pull requests with Argo CD ApplicationSets](https://developers.redhat.com/articles/2022/04/05/automate-cicd-pull-requests-argo-cd-applicationsets)
- [Automation of Everything - How To Combine Argo Events, Workflows & Pipelines, CD, and Rollouts](https://youtu.be/XNXJtxkUKeY)
- [Awesome-Argo: A Curated List of Awesome Projects and Resources Related to Argo](https://github.com/terrytangyuan/awesome-argo)
- [CI/CD in Light Speed with K8s and Argo CD](https://www.youtube.com/watch?v=OdzH82VpMwI&feature=youtu.be)
- [Combining Argo CD (GitOps), Crossplane (Control Plane), And KubeVela (OAM)](https://youtu.be/eEcgn_gU3SM)
- [Comparison of Argo CD, Spinnaker, Jenkins X, and Tekton](https://www.inovex.de/blog/spinnaker-vs-argo-cd-vs-tekton-vs-jenkins-x/)
- [Couchbase - How To Run a Database Cluster in Kubernetes Using Argo CD](https://youtu.be/nkPoPaVzExY)
- [Creating Temporary Preview Environments Based On Pull Requests With Argo CD And Codefresh](https://codefresh.io/continuous-deployment/creating-temporary-preview-environments-based-pull-requests-argo-cd-codefresh/)
- [Decentralized GitOps over environments](https://blogs.sap.com/2021/05/06/decentralized-gitops-over-environments/)
- [Deploy Argo CD with Ingress and TLS in Three Steps: No YAML Yak Shaving Required](https://itnext.io/deploy-argo-cd-with-ingress-and-tls-in-three-steps-no-yaml-yak-shaving-required-bc536d401491)
- [Deploying Microservices with GitOps](https://codefresh.io/blog/deploying-microservices-with-gitops/)
- [Dynamically scale the Argo CD application controller with OpenShift GitOps 1.10](https://developers.redhat.com/articles/2023/09/26/dynamically-scale-argo-cd-application-controller-openshift-gitops-110)
- [Enterprise CI/CD Best Practices – Part 1](https://codefresh.io/blog/enterprise-ci-cd-best-practices-part-1/)
- [Environments Based On Pull Requests (PRs): Using Argo CD To Apply GitOps Principles On Previews](https://youtu.be/cpAaI8p4R60)
- [From code to production with OpenShift Pipelines and Argo CD](https://developers.redhat.com/blog/2020/09/10/from-code-to-production-with-openshift-pipelines-and-argo-cd)
- [Getting Started with ArgoCD for GitOps Deployments](https://youtu.be/AvLuplh1skA)
- [Getting Started With GitOps and Argo CD](https://codefresh.io/blog/getting-started-with-gitops-and-argo-cd/)
- [GitOps Continuous Delivery with Argo and Codefresh](https://codefresh.io/events/cncf-member-webinar-gitops-continuous-delivery-argo-codefresh/)
- [GitOps Deployment and Kubernetes - using Argo CD](https://medium.com/riskified-technology/gitops-deployment-and-kubernetes-f1ab289efa4b)
- [GitOps for Kubeflow using Argo CD](https://v0-6.kubeflow.org/docs/use-cases/gitops-for-kubeflow/)
- [GitOps Toolsets on Kubernetes with CircleCI and Argo CD](https://www.digitalocean.com/community/tutorials/webinar-series-gitops-tool-sets-on-kubernetes-with-circleci-and-argo-cd)
- [GitOps Without Pipelines With ArgoCD Image Updater](https://youtu.be/avPUQin9kzU)
- [How to Apply GitOps to Everything - Combining Argo CD and Crossplane](https://youtu.be/yrj4lmScKHQ)
- [How to create Argo CD Applications Automatically using ApplicationSet? "Automation of GitOps"](https://amralaayassen.medium.com/how-to-create-argocd-applications-automatically-using-applicationset-automation-of-the-gitops-59455eaf4f72)
- [How to deploy single sign-on as code using GitOps](https://developers.redhat.com/articles/2023/04/10/how-deploy-single-sign-code-using-gitops)
- [How to Install and Upgrade Argo CD](https://codefresh.io/blog/how-to-install-and-upgrade-argo-cd/)
- [How to Kustomize your Codefresh/Argo Runtime](https://codefresh.io/blog/how-to-kustomize-your-codefresh-argo-runtime/)
- [How to Preview and Diff Your Argo CD Deployments](https://codefresh.io/blog/argo-cd-preview-diff/)
- [Introducing GitOps Versions: A Unified Way to Version Your Argo CD Applications](https://codefresh.io/blog/argocd-gitops-versions/)
- [Introducing Products: A Tool to Model Argo CD Application Relationships and Promotions](https://codefresh.io/blog/argocd-promotions-with-products/)
- [Introduction to Argo CD : Kubernetes DevOps CI/CD](https://www.youtube.com/watch?v=2WSJF7d8dUg&feature=youtu.be)
- [Machine Learning as Code](https://www.youtube.com/watch?v=VXrGp5er1ZE&t=0s&index=135&list=PLj6h78yzYM2PZf9eA7bhWnIh_mK1vyOfU). Among other things, describes how Kubeflow uses Argo CD to implement GitOPs for ML
- [Manage namespaces in multitenant clusters with Argo CD, Kustomize, and Helm](https://developers.redhat.com/articles/2022/04/13/manage-namespaces-multitenant-clusters-argo-cd-kustomize-and-helm)
- [Multi-Service Progressive Delivery with Argo Rollouts](https://codefresh.io/blog/multi-service-progressive-delivery-with-argo-rollouts/)
- [Multiple sources for Argo CD applications](https://developers.redhat.com/articles/2023/02/20/multiple-sources-argo-cd-applications)
- [Multiple sources for Argo CD applications](https://developers.redhat.com/articles/2023/02/20/multiple-sources-argo-cd-applications)
- [Prevent auto-reboot during Argo CD sync with machine configs](https://developers.redhat.com/articles/2021/12/20/prevent-auto-reboot-during-argo-cd-sync-machine-configs)
- [Progressive delivery for Kubernetes Config Maps using Argo Rollouts](https://codefresh.io/blog/progressive-delivery-for-kubernetes-config-maps-using-argo-rollouts/)
- [Progressive Delivery for Stateful Services Using Argo Rollouts](https://codefresh.io/blog/progressive-delivery-for-stateful-services-using-argo-rollouts/)
- [Scaling Argo CD Securely in 2024](https://codefresh.io/blog/scaling-argo-cd-securely-in-2024/)
- [Serverless, Tekton, and Argo CD: How to craft modern CI/CD workflows | DevNation Tech Talk](https://developers.redhat.com/devnation/tech-talks/serverless-tekton-argocd)
- [Setting up Argo CD with Helm](https://www.arthurkoziel.com/setting-up-argocd-with-helm/)
- [Simplify and Automate Deployments Using GitOps with IBM Multicloud Manager 3.1.2](https://www.ibm.com/cloud/blog/simplify-and-automate-deployments-using-gitops-with-ibm-multicloud-manager-3-1-2)
- [Six Critical Blindspots While Securing Argo CD](https://dnastacio.medium.com/gitops-argocd-security-cbb6fb6378bb)
- [Solving configuration drift using GitOps with Argo CD](https://www.cncf.io/blog/2020/12/17/solving-configuration-drift-using-gitops-with-argo-cd/)
- [Stay up to date with Argo CD and Renovate](https://mjpitz.com/blog/2020/12/03/renovate-your-gitops/)
- [Tutorial: Everything You Need To Become A GitOps Ninja](https://www.youtube.com/watch?v=r50tRQjisxw) 90m tutorial on GitOps and Argo CD.
- [Unveil the Secret Ingredients of Continuous Delivery at Enterprise Scale with Argo CD](https://akuity.io/blog/unveil-the-secret-ingredients-of-continuous-delivery-at-enterprise-scale-with-argocd-kubecon-china-2021/)
- [Using Argo CD & Datree for Stable Kubernetes CI/CD Deployments](https://youtu.be/17894DTru2Y)
- [Using Argo CD and Kustomize for ConfigMap Rollouts](https://codefresh.io/blog/using-argo-cd-and-kustomize-for-configmap-rollouts/)
- [Using Argo CDs new Config Management Plugins to Build Kustomize, Helm, and More](https://codefresh.io/blog/using-argo-cds-new-config-management-plugins-to-build-kustomize-helm-and-more/)
- [Welcome to the Argo CD Tutorial](https://redhat-scholars.github.io/argocd-tutorial/argocd-tutorial/index.html?sc_cid=7013a000002q0QMAAY)
- [Why You Should Choose Argo CD for GitOps](https://codefresh.io/blog/why-you-should-choose-argo-cd-for-gitops/)

</details>