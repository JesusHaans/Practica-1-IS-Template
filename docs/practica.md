# Práctica 1: (HTML5 + CSS + JS + API)

## Contexto

En esta práctica construirás un **mini Weather Dashboard**: una página web donde escribes una ciudad y se muestra el clima actual.

**Duración sugerida:** 2 horas (laboratorio).

## Producto final esperado

Una página que permite escribir una ciudad o la latitud y longitud de una ciudad, y mostrar:

- Nombre de la ciudad y país
- Temperatura actual, sensación térmica, humedad, viento
- Descripción e ícono del clima
- Estado de carga y manejo de error (ciudad inválida / sin red / API key inválida)

## Habilidades que repasa

### HTML (semantico)

- Etiquetas comunes y HTML5 semántico: `header`, `main`, `section`, `article`, `footer`
- Formularios: `form`, `label`, `input`, `button`

### CSS

- Selectores (tag, clase, id), combinadores
- Box model: padding, margin, border, `box-sizing`
- Tipografía, colores, `border-radius`
- Layout: **CSS Grid**

### JavaScript

- DOM: `querySelector`, eventos
- HTTP: `fetch`
- Async: `async/await`
- Manejo de errores: `try/catch`
- Renderizado en DOM (actualizar texto/atributos)

## Estructura del repositorio

> Importante: el **código está dentro de `src/`**.

``` bash
Practica-1/
├─ README.md
├─ docs/
│  └─ practica.md
└─ src/
   ├─ index.html
   └─ assets/
      ├─ css/
      │  └─ styles.css
      └─ js/
         ├─ main.js
         ├─ api/
         │  └─ openweather.js
         └─ ui/
            └─ render.js
```

**Idea de arquitectura (separación de responsabilidades):**

- `api/`: solo red/datos (no toca DOM)
- `ui/`: solo DOM/render (no llama a la red)
- `main.js`: orquesta (evento → API → UI)

## Nota importante: API Key (OpenWeather)

Para consultar OpenWeather necesitas una **API key**.

- En esta práctica, necesitaras usar tu propia API key de OpenWeather. Puedes obtenerla gratis registrándote en su sitio web haciendo clic aqui: [OpenWeather API Key](https://home.openweathermap.org/users/sign_up).
- **Importante **NO** debes subir tu API key a GitHub.**

## Guía por tiempo

### Setup + objetivo

* Revisa la estructura del repo (`docs/` y `src/`).
* Abre `src/index.html` con un servidor local (recomendado: Live Server).
* Edita `src/index.html` para añadir una estructura de basica de HTML5 y en los metadatos  linkea el `styles.css` y el `main.js`.
* Abre el navegador donde esta el `index.html`
* Abre DevTools → **Console** y confirma que no hay errores.
* DevTools → **Network**: confirma que cargan `styles.css` y `main.js`.

### HTML5 semántico + formulario

**Meta:** construir el esqueleto con semántica.

Tareas (en `src/index.html`):

* Crea la estructura con: `header`, `main`, `section`, `article`, `footer`.
* En el `header`: título y subtítulo.
* En `main`: arma dos bloques `section`:

  + Panel de búsqueda (formulario)
  + Resultados (tarjeta/card)

Formulario (requisitos):

* `form`.
* `label` con `for` apuntando al input.
* `input` con `id="cityInput"`, `name="city"`y un placeholder.
* `input` con `id="latInput"`, `name="latitude"`y un placeholder.
* `input` con `id="lonInput"`, `name="longitude"`y un placeholder.
* `button` con `type="submit"` y `id="btnSearchByName"`.
* `button` con `type="submit"` y `id="btnSearchByLatLon"`.

Mensajes:

* Un elemento `p` para estado/errores con `id="status"`.

Tarjeta de resultados (requisitos):

* Un `article` con `id="card"` y clase `hidden` (oculta al inicio).
* Dentro, agrega elementos con estos IDs (se usan después en JS):

  + `place`, `icon`, `desc`, `temp`, `feels`, `humidity`, `wind`.

### CSS: selectores + box model + grid

**Meta:** que se vea la app "bonita".

Tareas (en `src/assets/css/styles.css`):

* Aplica `box-sizing: border-box` globalmente.
* Define tipografía base y quita márgenes por defecto del `body`.
* Estiliza contenedores:

  + `.container` con **Grid** + `gap`
  + `.panel` y `.card` como “cajas” (borde, padding, border-radius)
* Estiliza el formulario:

  * `.search` con layout vertical y separación
  * `input` con padding/borde/redondeo y estado visible en `:focus`
  * `button` con estilo claro y `:hover`
* En la tarjeta:

  * `.metrics` como grid responsive (tarjetitas internas)
  * `.hidden` que realmente oculte el card

#### 80–110 min | JS: DOM + fetch + async/await

**Meta:** buscar ciudad y renderizar clima.

Tareas:

1. **API module**

* En `src/assets/js/api/openweather.js` implementa una función asíncrona para:

  * Construir URL con `URL` + `searchParams`
  * Llamar `fetch`
  * Revisar `res.ok` y lanzar error con el status si falla
  * Regresar JSON con el clima actual

2. **UI module**

* En `src/assets/js/ui/render.js` implementa:

  * setear mensajes de estado
  * mostrar/ocultar card alternando `.hidden`
  * pintar datos en el DOM (place, desc, métricas e ícono)

3. **Orquestación**

* En `src/assets/js/main.js` implementa el flujo:

  * `submit` del form → `preventDefault`
  * leer ciudad (`trim`)
  * mostrar “Cargando…”, ocultar card
  * pedir datos (API) → renderizar (UI)
  * manejar errores con `try/catch`

Pruebas manuales:

* Ciudad válida: “CDMX”, “Guadalajara”
* Ciudad inválida: “asdfghj”
* API key incorrecta: debe reflejar error HTTP (p. ej. 401)


### Cierre

* Repaso: **HTML estructura → CSS layout → JS datos/render**

## Entregables

1. **Funcionalidad:** buscar ciudad y mostrar el card con datos:
   - lugar (ciudad, país)
   - temp, sensación, humedad, viento
   - descripción + ícono
2. **Manejo de estados:**
   - “Cargando…” mientras se consulta
   - mensaje de error en caso de fallo (ciudad inválida, sin red, key inválida)
3. **README.md (obligatorio)** con:
   - **Datos del alumno** (nombre, grupo, correo)
   - **Cómo ejecutar** (pasos y comandos)
   - **Problemas/incidencias** (qué se te complicó y cómo lo resolviste o qué quedó pendiente)
   - (opcional) captura de pantalla

## Referencias sugeridas

### Referencias generales

- [cheatsheets](https://htmlcheatsheet.com/http://example.net/)
- [documentacion](https://developer.mozilla.org/en-US/)
- [API](https://openweathermap.org/)
- [API DOCS](https://openweathermap.org/current?collection=current_forecast)

### Donde practicar CSS

- [Froggy](https://flexboxfroggy.com/#es)
- [Garden](https://cssgridgarden.com/#es)
- [CSS Diner](https://flukeout.github.io/)
- [CSS Battle](https://cssbattle.dev/)

### Donde practicar JS

- [Aprende JavaScript](https://aprendejavascript.org/)

### Donde Practicar HTML

- [W3 school](https://www.w3schools.com/html/)
