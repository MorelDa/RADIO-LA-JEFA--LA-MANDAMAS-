# La Jefa — Web / PWA

Reproductor de radio en vivo, instalable como app (PWA), listo para subir a **GitHub** y desplegar en **Vercel**.

## 📁 Contenido del proyecto

```
la-jefa/
├── index.html          → la app completa (HTML + CSS + JS)
├── manifest.json        → configuración de la PWA (nombre, íconos, colores)
├── sw.js                → service worker (funcionamiento offline + control de caché)
├── vercel.json          → reglas de caché para que Vercel actualice bien
└── icons/               → íconos, favicon e imagen para compartir en redes
```

## ✏️ Cosas que puedes editar tú mismo

Abre `index.html`, busca el bloque `CONFIG` cerca del inicio del `<script>` (línea ~470) y edita ahí:

```js
const CONFIG = {
  STREAM_URL: "https://tu-servidor-de-stream...",
  STATION_NAME: "La Jefa",
  SOCIAL: {
    facebook:  "https://www.facebook.com/lajefallanosorientales",
    whatsapp:  "https://wa.me/573123181061"
  }
};
```

Ese es el único lugar donde necesitas pegar tus URLs de redes sociales — los íconos del botón "EN DIRECTO" tomarán esas URLs automáticamente.

## 🚀 Subir a GitHub

```bash
cd la-jefa
git init
git add .
git commit -m "La Jefa - versión inicial"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/la-jefa.git
git push -u origin main
```

## ▲ Desplegar en Vercel

1. Entra a [vercel.com](https://vercel.com) e inicia sesión con tu cuenta de GitHub.
2. Clic en **Add New → Project**.
3. Selecciona el repositorio `la-jefa`.
4. Como es un sitio estático (HTML puro), deja el **Framework Preset en "Other"** y no cambies nada más.
5. Clic en **Deploy**.

En unos segundos tendrás tu URL, por ejemplo `https://la-jefa.vercel.app`.

### Importante: actualiza la URL en el `<head>`
Una vez que tengas tu dominio de Vercel, abre `index.html` y reemplaza esta línea:
```html
<meta property="og:url" content="https://TU-DOMINIO.vercel.app/">
```
con tu dominio real. Esto es lo que hace que, al compartir el link en WhatsApp/Facebook/etc., aparezca la imagen y el título correctos.

## 🔄 Cómo funciona la caché (para que tus cambios se vean de inmediato)

Este proyecto ya viene configurado para evitar el problema típico de "subí un cambio y no se ve":

- `vercel.json` le dice a Vercel que **nunca** guarde en caché `index.html`, `manifest.json` ni `sw.js` — siempre se piden frescos al servidor.
- El *service worker* (`sw.js`) usa la estrategia "red primero" para la página principal: siempre intenta traer la versión más nueva, y solo usa la copia guardada si no hay conexión a internet.
- Los íconos sí se guardan en caché por 24 horas (porque casi nunca cambian), pero se revisan en segundo plano.

**Cada vez que subas una actualización de tu sitio**, sube también el número de versión en `sw.js` (línea 10):

```js
const CACHE_VERSION = 'v1';   // cámbialo a 'v2', 'v3', etc. en cada actualización
```

Esto obliga a que los teléfonos de tus oyentes que ya instalaron la app descarguen la versión nueva la próxima vez que la abran, en vez de quedarse con una copia vieja.

## 📲 Botón "Instalar App"

Aparece automáticamente arriba a la derecha cuando el navegador del visitante permite instalar la PWA (Chrome/Edge en Android y escritorio). En iPhone (Safari), como Apple no permite ese botón automático, el usuario debe instalarla manualmente: **Compartir → Agregar a pantalla de inicio**. Los íconos y el nombre para esa instalación ya están configurados en `manifest.json` y las etiquetas `apple-*` del `<head>`.

## 🖼️ Imagen al compartir en redes sociales

Ya está lista en `icons/og-image.jpg` (1200×630 con el logo). Si más adelante quieres cambiarla por una foto distinta, solo reemplaza ese archivo manteniendo el mismo nombre y tamaño.

## ✅ Qué se hizo en esta versión

- Pantalla de bienvenida (splash) con el logo animado antes de entrar al sitio.
- Nombre de la estación actualizado a **La Jefa** en todo el sitio (título, encabezado, reproductor, notificaciones, PWA).
- Overlay rojo difuminado sobre el video del reproductor principal.
- Favicon e íconos de la app generados a partir de tu logo.
- Configurada como **PWA instalable**, con botón "Instalar App" bien ubicado en el encabezado.
- Meta-etiquetas Open Graph / Twitter Card para que al compartir en redes aparezca la imagen y el nombre de la radio.
- Se eliminó la barra inferior de pestañas "Inicio / Programación"; el reproductor mini quedó como única barra inferior, funcional (play/pausa, silenciar, abrir reproductor completo).
- `vercel.json` + `sw.js` configurados para que las actualizaciones que subas se reflejen de inmediato para los usuarios.
- Un solo lugar (`CONFIG.SOCIAL` en `index.html`) para pegar todas tus URLs de redes sociales.
