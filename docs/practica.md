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

```
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

Listo: ya lo ajusté para que **los archivos no traigan la solución** y el `docs/practica.md` los guíe a **escribir el código en clase**, sin mostrarles snippets.

[Descargar repo semilla (sin solución)](sandbox:/mnt/data/weather-lab-seed-no-solution.zip)

### Guía por tiempo (2 horas) — versión “hazlo tú” (sin código)

#### 0–10 min | Setup + objetivo

* Revisa la estructura del repo (`docs/` y `src/`).
* Abre `src/index.html` con un servidor local (recomendado: Live Server).
* Abre DevTools → **Console** y confirma que no hay errores.
* (Opcional) DevTools → **Network**: confirma que cargan `styles.css` y `main.js`.

**Checkpoint 1:** la página abre desde servidor local y no hay errores en consola.

---

#### 10–40 min | HTML5 semántico + formulario

**Meta:** construir el esqueleto con semántica y accesibilidad mínima.

Tareas (en `src/index.html`):

* Crea la estructura con: `header`, `main`, `section`, `article`, `footer`.
* En el `header`: título y subtítulo.
* En `main`: arma dos bloques lógicos:

  * Panel de búsqueda (formulario)
  * Resultados (tarjeta/card)

Formulario (requisitos):

* `form` con `id="searchForm"`.
* `label` con `for` apuntando al input.
* `input` con `id="cityInput"`, `name="city"`, `required` y un placeholder.
* `button` con `type="submit"`.

Mensajes:

* Un elemento para estado/errores con `id="status"` y `aria-live="polite"`.

Tarjeta de resultados (requisitos):

* Un `article` con `id="card"` y clase `hidden` (oculta al inicio).
* Dentro, agrega elementos con estos IDs (se usan después en JS):

  * `place`, `icon`, `desc`, `temp`, `feels`, `humidity`, `wind`.

**Checkpoint 2:** ves el formulario; el card sigue oculto (por `.hidden`).

---

#### 40–80 min | CSS: selectores + box model + grid

**Meta:** que se vea “app” y sea responsive.

Tareas (en `src/assets/css/styles.css`):

* Aplica `box-sizing: border-box` globalmente.
* Define tipografía base y quita márgenes por defecto del `body`.
* Estiliza contenedores:

  * `.container` con **Grid** + `gap`
  * `.panel` y `.card` como “cajas” (borde, padding, border-radius)
* Estiliza el formulario:

  * `.search` con layout vertical y separación
  * `input` con padding/borde/redondeo y estado visible en `:focus`
  * `button` con estilo claro y `:hover`
* En la tarjeta:

  * `.metrics` como grid responsive (tarjetitas internas)
  * `.hidden` que realmente oculte el card
* Media query para pasar de 1 columna a 2 columnas en pantallas anchas.

**Checkpoint 3:** el layout se ve como “app” y responde al ancho (1 → 2 columnas).


#### 80–110 min | JS: DOM + fetch + async/await

**Meta:** buscar ciudad y renderizar clima.

Tareas:

1. **Config local (sin subir key)**

* Crea `src/assets/js/config.js` (solo local) para guardar tu API key.
* Verifica que **no se suba**: el repo ya ignora ese archivo en `.gitignore`.

2. **API module**

* En `src/assets/js/api/openweather.js` implementa una función asíncrona para:

  * Construir URL con `URL` + `searchParams`
  * Llamar `fetch`
  * Revisar `res.ok` y lanzar error con el status si falla
  * Regresar JSON con el clima actual

3. **UI module**

* En `src/assets/js/ui/render.js` implementa:

  * setear mensajes de estado
  * mostrar/ocultar card alternando `.hidden`
  * pintar datos en el DOM (place, desc, métricas e ícono)

4. **Orquestación**

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

**Checkpoint 4:** al buscar una ciudad válida, aparece el card con métricas e ícono.


#### 110–120 min | Cierre

* Repaso: **HTML estructura → CSS layout → JS datos/render**
* Recomendaciones de entrega (README completo y key fuera del repo).


## 7) Entregables (obligatorios)
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

## 8) Retos (opcionales)
Elige 1 o más:
1. **Loader real** (spinner) mientras carga.
2. Guardar la **última ciudad** en `localStorage` y cargarla al abrir.
3. Cambiar estilos según el clima (lluvia/soleado/noche) usando clases CSS.
4. Agregar **forecast 5 días** y renderizar 5 tarjetas.
5. Botón “Usar mi ubicación” con `navigator.geolocation` + endpoint por lat/lon.

## 9) Criterios de evaluación (orientativos)
- HTML semántico y formulario correcto
- CSS: selectores + box model + grid + responsive
- JS: fetch + async/await + manejo de errores + render correcto
- README claro y reproducible

## 10) Referencias sugeridas
- MDN Web Docs (HTML, CSS, JS, Fetch API, CSS Grid)
- OpenWeather: documentación de “Current Weather” y “Weather Conditions / icons”

## 11) Checkpoints sugeridos (para la clase)
Si vas programando en vivo, estos checkpoints te ayudan a “marcar” avances (por ejemplo como commits):
1. **CH1**: HTML semántico + formulario + card oculto.
2. **CH2**: CSS base (box model + grid + responsive).
3. **CH3**: JS listo (submit + loading + fetch + render).
4. **CH4**: Manejo de errores afinado + pruebas de casos (401/404/offline).