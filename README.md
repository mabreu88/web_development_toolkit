# 🛠️ Web Dev Utils

Repositorio con **scripts y configuraciones útiles** para proyectos de desarrollo web con **HTML, CSS, JavaScript y Sass**.
La idea es centralizar snippets, configuraciones base y buenas prácticas que pueden servir como punto de partida en nuevos proyectos.

---

## 📑 Índice

1. [Reset & Base CSS](#1-reset--base-css)
2. [NPM Scripts](#2-npm-scripts)
3. [Configuración con Gulp](#3-configuración-con-gulp)
4. [Snippets Sass](#4-snippets-sass)
5. [Snippets JavaScript](#5-snippets-javascript)

---

## 1. Reset & Base CSS

```css
html {
    box-sizing: border-box;
    font-size: 62.5%;
    /* 1rem = 10px */
}

*,
*::before,
*::after {
    box-sizing: inherit;
    margin: 0;
    padding: 0;
}
```

**Explicación**

* `box-sizing: border-box;` → facilita el maquetado.
* `font-size: 62.5%;` → `1rem = 10px`.
* Reset global para `margin` y `padding`.

---

## 2. NPM Scripts

En `package.json`:

```json
"scripts": {
  "sass": "sass --watch src/scss:build/css",
  "dev": "gulp dev",
  "build": "gulp css && npm run minify",
  "minify": "uglifyjs src/js/app.js -o build/js/app.min.js"
}
```

**Explicación**

* `sass`: compila y observa `.scss`.
* `dev`: inicia flujo con Gulp.
* `build`: compila CSS + minifica JS.
* `minify`: genera versión optimizada de JS con **UglifyJS**.

---

## 3. Configuración con Gulp

Archivo `gulpfile.js`:

```js
import {src, dest, watch} from 'gulp';
import gulpSass from 'gulp-sass';
import * as dartSass from 'sass';
import cleanCSS from 'gulp-clean-css';
import terser from 'gulp-terser';

const sass = gulpSass(dartSass);

// Compilar Sass → CSS
export function css(done) {
    src('src/scss/app.scss')
        .pipe(sass().on('error', sass.logError))
        .pipe(cleanCSS()) // minifica el CSS
        .pipe(dest('build/css'));
    done();
}

// Minificar JS
export function js(done) {
    src('src/js/**/*.js')
        .pipe(terser())
        .pipe(dest('build/js'));
    done();
}

// Vigilar cambios
export function dev() {
    watch('src/scss/**/*.scss', css);
    watch('src/js/**/*.js', js);
}
```

**Explicación**

* `css`: compila y minifica el CSS.
* `js`: minifica los archivos JavaScript.
* `dev`: observa cambios en `scss` y `js`.

---

## 4. Snippets Sass

### Mixin para `flexbox`

```scss
@mixin flex-center($direction: row, $gap: 1rem) {
  display: flex;
  flex-direction: $direction;
  justify-content: center;
  align-items: center;
  gap: $gap;
}
```

### Breakpoints responsivos

```scss
$breakpoints: (
  "sm": 480px,
  "md": 768px,
  "lg": 1024px,
  "xl": 1200px
);

@mixin respond($size) {
  @media (min-width: map-get($breakpoints, $size)) {
    @content;
  }
}
```

Ejemplo de uso:

```scss
.container {
  @include respond("md") {
    max-width: 70%;
  }
}
```

---

## 5. Snippets JavaScript

### Selección rápida de elementos

```js
const $ = (selector) => document.querySelector(selector);
const $$ = (selector) => document.querySelectorAll(selector);
```

### Scroll suave hacia un elemento

```js
function scrollToId(id) {
  document.getElementById(id).scrollIntoView({ behavior: "smooth" });
}
```

### Debounce (optimizar eventos)

```js
function debounce(func, delay = 300) {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => func(...args), delay);
  };
}
```

Ejemplo de uso:

```js
window.addEventListener("resize", debounce(() => {
  console.log("Resize optimizado");
}, 500));
```

---

## 🚀 Uso

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/tu-usuario/web-dev-utils.git
   ```
2. Instalar dependencias:

   ```bash
   npm install
   ```
3. Ejecutar compilación Sass directa:

   ```bash
   npm run sass
   ```
4. O usar el flujo con Gulp:

   ```bash
   npm run dev
   ```

---

## 📌 Próximos aportes

* Más mixins y funciones útiles en Sass.
* Configuración de Prettier/ESLint.
* Snippets de animaciones CSS y JS.
* Helpers para fetch API y manejo de datos.

---

## 🤝 Contribuciones

Si querés sumar snippets o configuraciones útiles, hacé un **fork** del repo y mandá un **pull request**.

---

## 📜 Licencia

Este proyecto está bajo la licencia **MIT**, por lo que podés usar, modificar y compartir libremente.
