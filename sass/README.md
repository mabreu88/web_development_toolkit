# 🧰 SASS Utilities — Web Development Toolkit

Esta carpeta contiene utilidades y configuraciones base para proyectos que utilicen **SASS/SCSS**, pensadas para mejorar la organización, escalabilidad y mantenimiento del código.
Forma parte del **Web Development Toolkit**, una colección de scripts y configuraciones para desarrollo web moderno.

---

## 📂 Estructura de archivos

```
sass-utils/
│
├── _normalize.scss
├── _variables.scss
├── _mixins.scss
└── index.scss
```

---

## 🧩 Descripción de los archivos

### `_normalize.scss`

Normaliza los estilos por defecto del navegador, garantizando una base consistente para todo el proyecto.

```scss
html {
  box-sizing: border-box;
  font-size: 62.5%; // 1rem = 10px
}

*,
*::before,
*::after {
  box-sizing: inherit;
  margin: 0;
  padding: 0;
}
```

---

### `_variables.scss`

Contiene únicamente los **breakpoints responsivos** que definen las resoluciones clave del diseño adaptable.

```scss
// Responsive breakpoints
$smartphone: 480px;
$tablet: 768px;
$desktop: 1200px;
$desktopXL: 1400px;
```

Estas variables son utilizadas por otros archivos (como los mixins) para mantener un control centralizado de los puntos de quiebre del proyecto.

---

### `_mixins.scss`

Define **mixins reutilizables** para simplificar tareas comunes en el desarrollo con SASS.
En particular, incluye un **mixin para media queries** que utiliza los breakpoints definidos en `_variables.scss`.

```scss
@use 'sass:map';
@use 'variables' as v;

$breakpoints: (
  smartphone: v.$smartphone,
  tablet: v.$tablet,
  desktop: v.$desktop,
  desktopXL: v.$desktopXL
);

// Mixin responsive
@mixin respond-to($device) {
  $size: map.get($breakpoints, $device);

  @if $size {
    @media screen and (min-width: #{$size}) {
      @content;
    }
  } @else {
    @warn "⚠️ Breakpoint '#{$device}' no existe en el mapa de breakpoints.";
  }
}
```

**Ejemplo de uso:**

```scss
.container {
  width: 100%;

  @include respond-to(tablet) {
    width: 80%;
  }

  @include respond-to(desktop) {
    width: 70%;
  }
}
```

---

### `index.scss`

Centraliza todas las importaciones de los archivos parciales SASS.

```scss
@forward 'normalize';
@forward 'variables';
@forward 'mixins';
```

Así, podés importar todas las utilidades desde un único punto:

```scss
@use 'sass-utils/index' as *;
```

---

## 💡 Snippets recomendados (VSCode)

Para acelerar tu flujo de trabajo, podés agregar snippets personalizados en VSCode.
Abrí la paleta de comandos (`Ctrl+Shift+P` → *Preferences: Configure User Snippets* → *scss.json*)
y agregá lo siguiente:

```json
{
  "Media Query Responsive": {
    "prefix": "mq",
    "body": [
      "@include respond-to(${1:tablet}) {",
      "  ${2:// tu código aquí}",
      "}"
    ],
    "description": "Crea un bloque responsive con el mixin respond-to"
  },
  "Mixin Base": {
    "prefix": "mixin",
    "body": [
      "@mixin ${1:name}($${2:params}) {",
      "  ${3:// contenido del mixin}",
      "}"
    ],
    "description": "Crea un mixin base en SASS"
  },
  "Variable": {
    "prefix": "var",
    "body": [
      "$${1:name}: ${2:value};"
    ],
    "description": "Crea una variable SASS rápidamente"
  }
}
```

Estos snippets te permiten crear mixins, variables o bloques responsivos con atajos como `mq`, `mixin` o `var`.

---

## 🚀 Objetivo de la carpeta

* Proveer una **base sólida y modular** para iniciar cualquier proyecto con SASS.
* Centralizar configuraciones de **breakpoints, mixins y normalización**.
* Fomentar un estilo de código **ordenado, escalable y reutilizable**.
* Servir como base para futuros módulos y componentes dentro del *Web Development Toolkit*.

---

📦 *Parte del repositorio:*
**`web-dev-toolkit`**
🧠 *Módulo:* SASS Utilities
👨‍💻 *Autor:* Martin Abreu
🗓️ *Versión inicial:* Octubre 2025
