---
title: Créer un Stepper en VueJS
description: 
published: true
date: 2023-06-21T22:23:56.396Z
tags: 
editor: markdown
dateCreated: 2023-06-21T22:23:56.396Z
---

# Créer un Stepper en VueJS

![stepper-hugo.jpg](/assets/img/developpement/vue_js/stepper-hugo.jpg)

Cet article a pour but de montrer comment réaliser son propre composant de stepper en VueJS, bien que le code reste très facilement adaptable pour n’importe quel autre framework front.

Ce type de stepper est généralement utilisé pour illustrer simplement des processus, par étapes, à l’utilisateur, et lui faire comprendre rapidement les étapes qu’il a réalisées et celles qu’il lui reste avant de terminer le processus. Les steppers créent un chemin clair vers la finalisation d’un processus. Il est vrai qu'offrir aux utilisateurs une idée précise du nombre d'étapes nécessaires pour atteindre la cible finale pourrait réduire considérablement le taux d'abandon. On retrouve ce type de stepper dans un tunnel d’achat par exemple, mais il peut servir à tout autre cas d’utilisation qui nécessite un processus défini par avance (enregistrement d’utilisateur, recherche avancée, constitution d’un dossier, prise de rendez-vous, …).

À la fin de cet article, vous saurez comment réaliser le composant suivant, en VueJS 2, TypeScript, HTML, CSS. Il a été réalisé avec quelques classes utilitaires de Bootstrap relatives aux flexboxes. Il est responsive puisqu’il permet de mettre environ une vingtaine d’étapes sans qu’elles soient trop serrées, ce qui est déjà bien trop pour un stepper de ce type !

![breadcrumb_final.png](/assets/img/developpement/vue_js/breadcrumb_final.png)

## 1 - Créer une étape

![breadcrumb_step1.png](/assets/img/developpement/vue_js/breadcrumb_step1.png)

Commençons par créer une seule étape de notre stepper. Nous allons d’abord créer un nouveau composant qui s’appellera “`Stepper.vue`”. Dans le template, le code suivant permet d’avoir une simple puce, ou bullet en anglais :

`Stepper.vue - Template`

```html
<div class="step row no-gutters col justify-content-center align-items-center">
 <div class="bullet row justify-content-center align-items-center">
   <div>1</div>
 </div>
 <div class="title col-12">
   Étape 1
 </div>
</div>
```

Nous aurons aussi besoin du code SCSS suivant :

`Stepper.vue - Styles`

```scss
.step {
 .bullet {
   border-radius: 50%;
   background-color: #26318d;
   color: white;
   font-weight: bold;
   width: 2rem;
   height: 2rem;
 }

 .title {
   font-size: small;
   color: #26318d;
   text-align: center;
   margin-top: 0.3rem;
 }
}
```

Ici nous définissons donc une div `step` qui est un composant flexbox dont les enfants sont centrés (`row no-gutters justify-content-center align-items-center`). La classe `no-gutters` permet d’enlever les gouttières (padding) sur le côté de notre composant, ainsi que ses enfants.

Le premier enfant de `step` est une div `bullet` qui contient le numéro de l’étape que nous créons. Le texte est parfaitement centré encore une fois grâce aux flexboxes. En matière de CSS, on lui donne un `border-radius` de 50% pour rendre ronde notre bullet, et on fixe sa taille à 2rem.

Enfin, nous avons le dernier enfant `title`, qui contient le nom de l’étape actuelle. On lui donne la classe `col-12` pour qu’il puisse prendre toute la largeur disponible.

## 2 - Agencer plusieurs étapes

![breadcrumb_step2.png](/assets/img/developpement/vue_js/breadcrumb_step2.png)

Dans cette deuxième étape, nous allons essayer de rendre notre composant un peu plus paramétrable, en donnant en entrée une liste d’étapes à afficher. Pour cela nous allons créer notre fichier “`Stepper.ts`”, et y ajouter le code qui suit. Il suffit ensuite de faire un lien vers ce fichier dans notre composant Vue. Les deux méthodes fonctionnent et sont équivalentes.

`Stepper.ts`

```typescript
@Component
export default class Stepper extends Vue {
 @Prop({ default: [] })
 public steps: string[];
}
```

Ici nous introduisons donc un nouvel attribut à notre composant : `steps`. C’est une prop (une donnée d’entrée de notre composant) qui est par défaut un tableau vide.

`Stepper.vue - Template`

```typescript
<div class="steps-container w-100 row no-gutters justify-content-between align-items-center">
 <div class="step row no-gutters col justify-content-center align-items-center"
      v-for="(step, index) in steps"
      :key="step">
   <div class="bullet row justify-content-center align-items-center">
     <div>{{ index + 1 }}</div>
   </div>
   <div class="title col-12">
     {{ step }}
   </div>
 </div>
</div>
```

Dans notre template HTML, nous itérons sur ce tableau pour afficher une étape pour chaque élément de notre tableau `steps`. Pour cela, nous entourons notre code existant par une div `steps-container` qui, grâce aux flexboxes, centre ses enfants verticalement (`align-items-center`) et les écarte au maximum à l’horizontal (`justify-content-between`). Il est important de rajouter l’attribut `:key="step"` ici puisqu’il permet à la directive `v-for` de garder la trace de l’élément en cours, si jamais ce dernier change dynamiquement, en lui donnant un identifiant.

## 3 - Relier les bullets par une ligne

![breadcrumb_step3.png](/assets/img/developpement/vue_js/breadcrumb_step3.png)

Cette étape est souvent la plus difficile, puisqu’elle nécessite un peu de logique et de dextérité, comparé aux étapes précédentes. C’est parti !

`Stepper.vue - Template`

```typescript
<div class="steps-container w-100 row no-gutters justify-content-between align-items-center">
 <div class="step row no-gutters col justify-content-center align-items-center"
      v-for="(step, index) in steps"
      :key="step">
   <div class="bullet row justify-content-center align-items-center">
     <div>{{ index + 1 }}</div>
   </div>
   <div class="title col-12">
     {{ step }}
   </div>
   <div class="line"
        v-if="index < steps.length - 1">
   </div>
 </div>
</div>
```

Nous allons donc créer une ligne pour chaque étape que nous avons, sauf la dernière étape bien sûr, sinon on dépasserait (d’où la condition du `v-if`).

`Stepper.vue - Styles`

```scss
.step {
 position: relative;

 .line {
   width: 100%;
   height: 2px;
   background-color: #26318d;
   position: absolute;
   top: 25%;
   left: 50%;
   z-index: -1;
 }

 [...]
}
```

Cette ligne va prendre toute la largeur de notre bloc (donc la largeur d’une col auto-calculée par flexbox). Nous lui donnons une position absolue, qui va donc se référer à son parent relatif le plus proche, donc nous rendons son parent `step` positionné relativement. Ainsi, nous pouvons utiliser les propriétés `left` et `top` pour positionner librement notre ligne dans son conteneur parent, comme le montre le GIF ci-dessous.

![breadcrumb_step3_line_placement.gif](/assets/img/developpement/vue_js/breadcrumb_step3_line_placement.gif)

## 4 - Gérer l’état de chaque étape

![breadcrumb_final-1.png](/assets/img/developpement/vue_js/breadcrumb_final-1.png)

Enfin, il ne nous reste plus qu’à gérer l’état de chaque étape. Nous devons gérer l’état disabled des `bullet`, `title` et `line` lorsque l’étape est postérieure à l’étape actuelle. Les étapes précédentes, elles, doivent afficher une coche verte en lieu et place du numéro de l’étape. Pour cela, nous avons besoin d’une nouvelle prop qui identifiera l’étape actuelle.

`Stepper.ts`

```typescript
@Component({
 components: {
   CheckIcon,
 },
})
export default class Stepper extends Vue {
 @Prop({ default: [] })
 public steps: string[];

 @Prop({ default: 0 })
 public currentStep: number;
}
```

Nous définissons cette prop dans notre composant Stepper, sous le nom `currentStep` qui sera un nombre, par défaut 0, et qui correspond à l’index de l’étape actuelle dans le tableau `steps`. On en profite aussi pour injecter le composant `CheckIcon` de [vue-feather-icons](https://vue-feather-icons.egoist.sh/) qui nous servira pour afficher la coche verte.

`Stepper.vue - Template`

```typescript
<div class="steps-container w-100 row no-gutters justify-content-between align-items-center">
  <div class="step row no-gutters col justify-content-center align-items-center"
         v-for="(step, index) in steps"
         v-bind:class="{ disabled: index > currentStep }"
         :key="step">
    <div class="bullet row justify-content-center align-items-center">
      <check-icon v-if="index < currentStep || currentStep === steps.length - 1"
                  size="1x"
                  class="check-icon"></check-icon>
      <div v-else>{{ index + 1 }}</div>
    </div>
    <div class="title col-12">
      {{ step }}
    </div>
    <div class="line"
         v-if="index < steps.length - 1"
         v-bind:class="{ disabled: index === currentStep }">
    </div>
  </div>
</div>
```

Nous ajoutons ici dynamiquement la classe `disabled` sur notre `step` lorsque l’index est supérieur à l’index de notre `currentStep`. Cette règle va permettre de donner un style disabled à tous les éléments enfants de notre `step`, que nous définissons ci-dessous. Cependant, puisque notre `line` correspond au lien entre notre étape et la suivante, nous devons la rendre disabled pour notre étape actuelle également.

Enfin, l’icône de coche verte a été ajoutée dans la `bullet`, avec la condition d’apparaître uniquement si nous avons déjà passé l’étape OU si le processus est terminé (auquel cas toutes les `bullet` auront donc une coche verte).

`Stepper.vue - Styles`

```scss
.step {
 position: relative;

 .line {
   [...]

   // For disabling line for currentStep
   &.disabled {
     background-color: lighten(#26318d, 40%);
   }
 }

 [...]

 .check-icon {
   color: #00d992;
 }

 // For disabling everything for next steps
 &.disabled {
   .line, .bullet {
     background-color: lighten(#26318d, 40%);
   }

   .title {
     color: lighten(#26318d, 40%);
   }
 }
}
```

Pour gérer l’état `disabled` de notre `line` actuelle, nous changeons juste son `background-color` en utilisant une fonction bien pratique de SCSS qui s’appelle `lighten` et permet d’éclaircir une couleur d’un certain pourcentage. Nous utilisons la même technique pour le deuxième bloc de code qui correspond à l’état disabled des étapes suivantes.

Et voilà, c’est déjà fini ! Voici le résultat sous forme d’animation pour se rendre compte de l’état de notre stepper dans chaque étape :

![breadcrumb_final_animation-1.gif](/assets/img/developpement/vue_js/breadcrumb_final_animation-1.gif)

## Aller plus loin
Ce stepper est plutôt simple et ne permet pas d’interaction. On pourrait rajouter des actions lors d’un clic sur une étape précédente par exemple, ce qui permettrait un retour en arrière. On pourrait aussi ajouter un peu d’animation lors du passage d’une étape à l’autre, pour rendre le composant plus agréable.

source : https://blog.ippon.fr/