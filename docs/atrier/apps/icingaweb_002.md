---
title: Création d'un Thème Personnalisé
description: 
published: true
date: 2023-04-21T10:47:22.059Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:16.738Z
---

===== Création du thème =====

Fichier à placer dans : ''/usr/share/icingaweb2/public/css/themes/''
Fichier avec extension ''.less''

Exemple "Winter.less" : 

<code css>
/*! Icinga Web 2 | (c) 2015 Icinga Development Team | GPLv2+ */

@icinga-blue: #001E5b;

#icinga-logo {
    background-image: url('../img/winter/logo_icinga_big_winter.png');
}

@tr-active-color: #dadeea;
@tr-hover-color: #c6cfe4;

/* Snow, from http://codepen.io/NickyCDK/pen/AIonk */
#header, #login {
    background-image: url('../img/winter/snow1.png'), url('../img/winter/snow2.png'), url('../img/winter/snow3.png');
    -webkit-animation: snow 10s linear infinite;
    -moz-animation: snow 10s linear infinite;
    -ms-animation: snow 10s linear infinite;
    animation: snow 10s linear infinite;
}

@keyframes snow {
  0% {background-position: 0px 0px, 0px 0px, 0px 0px;}
  50% {background-position: 500px 500px, 100px 200px, -100px 150px;}
  100% {background-position: 500px 1000px, 200px 400px, -100px 300px;}
}
@-moz-keyframes snow {
  0% {background-position: 0px 0px, 0px 0px, 0px 0px;}
  50% {background-position: 500px 500px, 100px 200px, -100px 150px;}
  100% {background-position: 400px 1000px, 200px 400px, 100px 300px;}
}
@-webkit-keyframes snow {
  0% {background-position: 0px 0px, 0px 0px, 0px 0px;}
  50% {background-position: 500px 500px, 100px 200px, -100px 150px;}
  100% {background-position: 500px 1000px, 200px 400px, -100px 300px;}
}
@-ms-keyframes snow {
  0% {background-position: 0px 0px, 0px 0px, 0px 0px;}
  50% {background-position: 500px 500px, 100px 200px, -100px 150px;}
  100% {background-position: 500px 1000px, 200px 400px, -100px 300px;}
}

#menu .nav-level-2 > .nav-item.active {
  > a:focus, > a:hover {
    opacity: 1;
  }
}

#menu ul.nav-level-1 > .nav-item {
  &:focus, &:hover {
    background-image: url('../img/winter/snow1.png'), url('../img/winter/snow2.png'), url('../img/winter/snow3.png');
    -webkit-animation: snow 10s linear infinite;
    -moz-animation: snow 10s linear infinite;
    -ms-animation: snow 10s linear infinite;
    animation: snow 10s linear infinite;
  }

  > a:hover, > a:focus {
    color: white;
  }

</code>

Exemple "solarized-dark.less" : 

<code css>
/*! Icinga Web 2 | (c) 2017 Icinga Development Team | GPLv2+ */

/* solarized colors: http://simianuprising.com/wp-content/uploads/2012/08/solarized-reference-horizontal.png */

@base01: #586e75;
@base00: #657b83;
@base02: #073645;
@base03: #002b36;
@base0: #839496;
@base1: #93a1a1;
@base2: #eee8d5;

@icinga-blue: @base1;
@body-bg-color: @base03;

@color-ok: #859900;
@color-critical: #dc322f;
@color-critical-handled: #a82523;
@color-warning: #cf9b00;
@color-warning-handled: #b58900;
@color-unknown: #6c71c4;
@color-unknown-handled: #444f83;
@color-unreachable: #2aa198;
@color-unreachable-handled: #1d736c;
@color-pending: #268bd2;

@text-color: @base2;
@text-color-light: @base1;
@text-color-on-icinga-blue: @base2;
@text-color-inverted: @base02;

@link-color: @base0;

@gray: @base1;
@gray-light: @base01;
@gray-lighter: #00222b;
@gray-lightest: @base02;

@tr-active-color: @base01;
@tr-hover-color: @base02;

#header {
  background-color: @base02;
}

.tabs > li > a {
  color: @base00;
}

.action-link {
  color: @link-color;
}

input, select {
  background-color: @body-bg-color;
}

#login {
  background: @base02;

  .control-label {
    color: @base0;
  }

  input[type=submit] {
    color: @base0;
    background-color: @base02;
  }
}

#login-footer {
  color: @base01;
}

textarea {
  background-color: @base01;
}

.control-button, input[type="submit"] {
  color: @base0;
}

.badge.handled {
  background-color: @body-bg-color !important;
  color: @text-color;

  &.state-warning {
    border: 1px solid @color-warning-handled;
    margin-top: -1px;
  }

  &.state-critical {
    border: 1px solid @color-critical-handled;
    margin-top: -1px;
  }

  &.state-down {
    border: 1px solid @color-critical-handled;
    margin-top: -1px;
  }

  &.state-unreachable {
    border: 1px solid @color-unreachable-handled;
    margin-top: -1px;
  }

  &.state-unknown {
    border: 1px solid @color-unknown-handled;
    margin-top: -1px;
  }
}

#menu {
  background-color: @base02;

  input.search {
    color: @text-color;
    background-image: url(../img/icons/search_white.png);

    &:focus {
      background-image: url(../img/icons/search_white.png) !important;
    }
  }

  .nav-level-1 > .nav-item {
    color: @text-color;

    &.active, &:hover {
      color: @text-color;
      background-color: @base00;
    }
  }

  .nav-level-2 > .nav-item {
    color: @text-color;
    background-color: @base03;

    &.active, &:hover {
      a {
        color: @text-color;
      }
      background-color: @base00;
    }
  }


  .nav-itemxx{
    &:hover,
    &:hover > a,
    &.active,
    &.active > a {
      color: @text-color !important;
      //background-color: @base01 !important;
    }
  }
}

.boxview a:focus {
  color: @text-color;
  text-decoration: underline;
}

.icinga-module.module-monitoring {
  @timeline-notification-color: #1650CF;
  @timeline-hard-state-color: #A24600;
  @timeline-comment-color: #346964;
  @timeline-ack-color: #855D18;
  @timeline-downtime-start-color: #515151;
  @timeline-downtime-end-color: #5e5e2f;

  // Unfortunately it does not suffice to only override the timeline colors here, because our less compiler seems to
  // have the related style block in module.less already evaluated

  .timeline-notification {
    background-color: @timeline-notification-color;

    &.extrapolated {
      background-color: lighten(@timeline-notification-color, 20%);
    }
  }

  .timeline-hard-state {
    background-color: @timeline-hard-state-color;

    &.extrapolated {
      background-color: lighten(@timeline-hard-state-color, 20%);
    }
  }

  .timeline-comment {
    background-color: @timeline-comment-color;

    &.extrapolated {
      background-color: lighten(@timeline-comment-color, 20%);
    }
  }

  .timeline-ack {
    background-color: @timeline-ack-color;

    &.extrapolated {
      background-color: lighten(@timeline-ack-color, 20%);
    }
  }

  .timeline-downtime-start {
    background-color: @timeline-downtime-start-color;

    &.extrapolated {
      background-color: lighten(@timeline-downtime-start-color, 20%);
    }
  }

  .timeline-downtime-end {
    background-color: @timeline-downtime-end-color;

    &.extrapolated {
      background-color: lighten(@timeline-downtime-end-color, 20%);
    }
  }
}

.icinga-module.module-monitoring {
  .tactical {
    .box {
      border-color: @body-bg-color !important;
    }

    div.box.contents {
      background: @body-bg-color;
    }
  }
}

.icinga-module.module-director {
  form {
    input[type=text], input[type=password], textarea, select, .director-suggestions li {
      background-color: @body-bg-color;
      border-bottom: 1px solid @gray-light;
    }

    .director-suggestions li:hover {
      background-color: @icinga-blue;
    }

    ul.extensible-set input[type=text] {
      background-color: @icinga-blue;
      color: @base2;
    }

    p.description {
      background-color: @base02;
    }

    option {
      background-color: @body-bg-color;
    }
  }

  dd.active ul.extensible-set input[type=text], ul.extensible-set.sortable input[type=text] {
    background: @body-bg-color;
    border: 1px solid @gray-light;
  }

  ul.main-actions li a {
    color: @icinga-blue;
  }
}

</code>

Autre exemple "high-contrast.less" : 

<code css>
/*! Icinga Web 2 | (c) 2016 Icinga Development Team | GPLv2+ */

@icinga-blue: #006D8C;

@color-ok: #006400;
@color-critical: #EE0000;
@color-critical-handled: #EE0000;
@color-warning: #8B5A00;
@color-warning-handled: #8B5A00;
@color-unknown: #800080;
@color-unknown-handled: #800080;
@color-unreachable: #800080;
@color-unreachable-handled: #800080;
@color-pending: #0000EE;

@text-color: #191919;
@text-color-light: #555555;

@menu-highlight-color: white;
@menu-2ndlvl-color: white;
@menu-2ndlvl-highlight-color: white;

#menu ul.nav-level-1 > .nav-item > a {
  &:focus, &:hover {
    text-decoration: underline;
  }
}

#menu .nav-level-2 > .nav-item.active {
  background-color: black;

  > a:focus, > a:hover {
    opacity: 1;
  }
}

#menu .nav-level-2 > .nav-item > a {
  &:hover, &:focus {
    text-decoration: underline;
  }
}

#menu .active > a {
  text-decoration: underline;
}

.badge {
  background-color: @text-color-light;
}

.badge.handled {
  background-color: @body-bg-color !important;
  color: @text-color;

  &.state-warning {
    border: 1px solid @color-warning-handled;
    margin-top: -1px;
  }

  &.state-critical {
    border: 1px solid @color-critical-handled;
    margin-top: -1px;
  }

  &.state-down {
    border: 1px solid @color-critical-handled;
    margin-top: -1px;
  }

  &.state-unreachable {
    border: 1px solid @color-unreachable-handled;
    margin-top: -1px;
  }

  &.state-unknown {
    border: 1px solid @color-unknown-handled;
    margin-top: -1px;
  }
}

.boxview a:focus {
    color: @text-color;
    text-decoration: underline;
}

.icinga-module.module-monitoring {
  @timeline-notification-color: #1650CF;
  @timeline-hard-state-color: #A24600;
  @timeline-comment-color: #346964;
  @timeline-ack-color: #855D18;
  @timeline-downtime-start-color: #515151;
  @timeline-downtime-end-color: #5e5e2f;

  // Unfortunately it does not suffice to only override the timeline colors here, because our less compiler seems to
  // have the related style block in module.less already evaluated

  .timeline-notification {
    background-color: @timeline-notification-color;

    &.extrapolated {
      background-color: lighten(@timeline-notification-color, 20%);
    }
  }

  .timeline-hard-state {
    background-color: @timeline-hard-state-color;

    &.extrapolated {
      background-color: lighten(@timeline-hard-state-color, 20%);
    }
  }

  .timeline-comment {
    background-color: @timeline-comment-color;

    &.extrapolated {
      background-color: lighten(@timeline-comment-color, 20%);
    }
  }

  .timeline-ack {
    background-color: @timeline-ack-color;

    &.extrapolated {
      background-color: lighten(@timeline-ack-color, 20%);
    }
  }

  .timeline-downtime-start {
    background-color: @timeline-downtime-start-color;

    &.extrapolated {
      background-color: lighten(@timeline-downtime-start-color, 20%);
    }
  }

  .timeline-downtime-end {
    background-color: @timeline-downtime-end-color;

    &.extrapolated {
      background-color: lighten(@timeline-downtime-end-color, 20%);
    }
  }
}

</code>

Exemple d'un thème qui passe tout au vert (''always-green.less'') :

<code css>
@icinga-blue: #44BB77;

@color-ok: #44BB77;
@color-critical: #44BB77;
@color-critical-handled: #44BB77;
@color-warning: #44BB77;
@color-warning-handled: #44BB77;
@color-unknown: #44BB77;
@color-unknown-handled: #44BB77;
@color-unreachable: #44BB77;
@color-unreachable-handled: #44BB77;
@color-pending: #44BB77;

#header, #login {
	background-color: #44BB77;
}
</code>

Autre thème : [[https://github.com/vita2/icingaweb2-module-theme-dark]]

Autre thème : [[https://github.com/Icinga/icingaweb2-theme-company]]