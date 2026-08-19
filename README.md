# Don Pufo — Gestión estratégica de marrones

Web de **Don Pufo**, una consultora ficticia y humorística que resuelve problemas cotidianos, incómodos o absurdos con una seriedad empresarial completamente desproporcionada.

Sitio estático de un solo archivo (`index.html`), sin build, sin dependencias que instalar. Animaciones 3D con Three.js (CDN).

---

## Cambiar el nombre de la marca

Todo el naming está parametrizado. Abre `index.html`, busca el bloque **① CONFIGURACIÓN DE MARCA** (arriba del primer `<script>`) y edita:

```js
const BRAND = {
  name : 'Don Pufo',   // Nombre completo de la marca
  root : 'Pufo',       // Raíz para productos derivados
  ...
};
```

Con eso se recalcula automáticamente:

| Elemento | Se deriva de | Ejemplo `Don Pufo` | Ejemplo `Mr. Fixit` |
|---|---|---|---|
| Logo / monograma | iniciales de `name` | `DP` | `MF` |
| Códigos de expediente | `filePrefix` | `DP-2026-0047` | `MF-2026-0047` |
| Laboratorio | `root` + `Lab` | PufoLab | FixitLab |
| Puntuación | `root` + `Score` | PufoScore | FixitScore |
| Clasificación | `root` en mayúsculas | PUFO I–V | FIXIT I–V |
| Divisiones, casos, FAQ, tarifas… | plantillas `{{...}}` | — | — |

Ningún texto del sitio escribe el nombre a pelo: todo el contenido usa marcadores `{{brand}}`, `{{founder}}`, `{{lab}}`, `{{score}}`, `{{tier}}`, `{{prefix}}`, `{{problem}}`, `{{problems}}`… que resuelve la función `T()`.

También puedes fijar cualquier campo a mano (`monogram`, `lab`, `score`, `tier`, `filePrefix`) si no te gusta el valor automático.

---

## Estructura del archivo

`index.html` está dividido en seis bloques comentados:

1. **Configuración de marca** — el objeto `BRAND` y el motor de plantillas `T()`.
2. **Contenido** — divisiones, casos, testimonios, tarifas, FAQ, escalas de riesgo. Añadir una división nueva es añadir un objeto a `SERVICES`.
3. **Motor** — preguntas adaptativas, generación del expediente, PufoScore, límites éticos.
4. **Hook para LLM** — `AI` + `buildPayload()` + `RESPONSE_SCHEMA`.
5. **Vistas y enrutado** — SPA con hash routing.
6. **Escena 3D** — Three.js.

---

## Estado del generador

⚠️ **En esta versión publicada la generación con IA no está operativa.**

Los expedientes se generan con un **motor local de plantillas** que se ejecuta íntegramente en el navegador: funciona sin servidor, sin API key y sin conexión. Es determinista sobre las respuestas del usuario (número de implicados, relación, plazo, información previa, impacto del fracaso…) y produce estrategia, argumentario, objeciones, contramedidas, plan B, PufoScore y probabilidad.

Para conectar una LLM real más adelante:

```js
AI.enabled  = true;
AI.endpoint = 'https://tu-backend/api/expediente';
```

Tu backend recibe el payload de `buildPayload()` y debe devolver el JSON de `RESPONSE_SCHEMA`. Si falla, el sitio cae automáticamente al motor local.

> **Nunca pongas una API key en este archivo.** Es un sitio estático: cualquiera puede leerlo. Usa siempre un proxy propio.

---

## Límites

El motor bloquea peticiones relacionadas con documentación falsificada, suplantación de identidad, justificantes médicos, comunicaciones en nombre de instituciones, fraude o declaraciones ante autoridades. En esos casos devuelve un expediente **no admitido**, en personaje, con alternativas legales.

---

## Publicación

Sitio estático. Se sirve tal cual desde GitHub Pages (rama `main`, carpeta raíz). El archivo `.nojekyll` evita el procesado de Jekyll.

---

## Aviso

Obra de ficción y parodia. La empresa, los servicios, los clientes, los testimonios, los expedientes y las tarifas son inventados. No se presta ningún servicio real y no se recoge ningún dato: los expedientes guardados se quedan en el `localStorage` del navegador.
