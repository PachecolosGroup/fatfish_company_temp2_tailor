# LO-V Fashion Design — Página de citas

Página de una sola pantalla para que las clientas vean el trabajo y agenden
una cita de cotización. HTML, CSS y JavaScript en **un solo archivo**, más una
carpeta de imágenes. Sin build, sin npm, sin dependencias.

---

## Cómo verla

Doble clic en `index.html`. Se abre en cualquier navegador.

## Cómo publicarla

Es un sitio estático. Sirve cualquier hosting:

- **Netlify Drop** — arrastra la carpeta completa a `netlify.com/drop`. Gratis, 30 segundos.
- **Cloudflare Pages / Vercel** — igual de simple, gratis.
- **Hosting tradicional (cPanel/FTP)** — sube `index.html` y la carpeta `img/` a `public_html/`.

**Sube la carpeta completa**, no solo el `index.html`. Sin `img/` no se ve ninguna foto.

---

## LO PRIMERO: conectar el calendario

Ahora mismo, donde va el calendario aparece un panel que invita a escribir por
WhatsApp. Eso es a propósito: **el calendario todavía no está conectado**, y es
preferible eso a mostrar un calendario decorativo que no reserva nada.

### Paso 1 — Crear la cuenta

Entra a [calendly.com](https://calendly.com) y crea una cuenta. El plan gratis alcanza.
Conecta el Google Calendar de Lucero: así los horarios ocupados se bloquean solos
y nunca se agendan dos clientas a la misma hora.

### Paso 2 — Crear DOS tipos de cita, ambos de 1 hora

| Tipo de cita          | Duración | Ubicación en Calendly |
| --------------------- | -------- | --------------------- |
| Cita por videollamada | 60 min   | Google Meet o Zoom    |
| Cita en el atelier    | 60 min   | Dirección física      |

Configura también los horarios de atención y cuánta anticipación mínima quieres
(por ejemplo, que nadie pueda agendar para dentro de 2 horas).

### Paso 3 — Pegar los dos enlaces

Abre `index.html`, busca `TU-USUARIO` (aparece dos veces, cerca del final del
archivo) y reemplaza las direcciones completas:

```js
var CALENDLY = {
  video: "https://calendly.com/TU-USUARIO/cita-videollamada",
  presencial: "https://calendly.com/TU-USUARIO/cita-atelier",
};
```

En cuanto los enlaces sean reales, el calendario aparece solo. **Mientras digan
`TU-USUARIO`, se sigue mostrando el panel de WhatsApp** — así la página nunca
queda rota, solo con un camino distinto.

> Acuity Scheduling y Square Appointments funcionan igual de bien. Si prefieres
> alguno de esos, el cambio es el mismo: reemplazar las dos direcciones.

---

## Cambiar los precios

Todos los precios viven en **un solo lugar**, en el `<script>` al final del archivo:

```js
var PAQUETES = [
  {
    id: "silver",
    nombre: "Silver",
    cuota: 1000,
    tramos: [
      [30, 2500],
      [50, 2800],
      [80, 3500],
    ],
  },
  {
    id: "gold",
    nombre: "Gold",
    cuota: 1500,
    tramos: [
      [100, 4500],
      [200, 5800],
      [300, 6800],
    ],
  },
  {
    id: "diamond",
    nombre: "Diamond",
    cuota: 2000,
    tramos: [
      [100, 7800],
      [200, 8800],
      [300, 9800],
    ],
    destacado: true,
  },
  {
    id: "royal",
    nombre: "Royal Signature",
    cuota: 2500,
    tramos: [
      [100, 9900],
      [200, 11300],
      [300, 12800],
    ],
  },
];
```

- Cada par `[80, 3500]` significa: **hasta 80 invitados, $3,500**.
- `cuota` es la cuota inicial que aparece como "Reserva tu fecha con $X".
- `destacado: true` es el que lleva la estrella dorada. Solo uno.

### Cómo cobra el deslizador

El precio **se redondea hacia arriba al siguiente tramo**: 150 invitados pagan
la tarifa de 200. Cuando eso pasa, la tarjeta lo dice en letra pequeña
("150 invitados · tarifa de hasta 200") para que nadie se lleve una sorpresa
en la cita.

### ⚠️ Un punto a confirmar con Lucero

La lista de precios **no tiene un tramo de Silver arriba de 80 invitados**.
La página no inventa esa cifra: si el deslizador pasa de 80, la tarjeta de
Silver se atenúa y dice _"Consúltalo en tu cita"_.

**Si Silver sí se ofrece para eventos más grandes**, agrega el tramo:

```js
{ id:'silver', nombre:'Silver', cuota:1000,
  tramos:[[30,2500],[50,2800],[80,3500],[200,XXXX],[300,XXXX]] },
```

**Si Silver efectivamente topa en 80 invitados**, déjalo como está: se lee como
un límite del paquete, no como un error.

---

## Cambiar los textos

La página está en español e inglés. Todos los textos están juntos en el objeto
`T` del `<script>`, con `es:` arriba y `en:` abajo. Si cambias un texto en
español, cambia también su versión en inglés — están en el mismo orden.

El contenido de cada paquete (los puntos con palomita y el texto del desplegable
"Ver todo lo que incluye") está en el objeto `CONTENIDO`, con la misma estructura.

---

## Cambiar las fotos

Las 11 fotos están en `img/`, en formato WebP. Para reemplazar una, guarda la
nueva con **exactamente el mismo nombre** y listo.

Para agregar o quitar fotos, edita la lista `FOTOS` del `<script>`. Cada línea es:

```js
['nombre-del-archivo', 'Descripción en español', 'Description in English'],
```

La descripción no es decorativa: la leen los lectores de pantalla y Google.

La foto grande de arriba se cambia en la etiqueta `<img class="hero-img">`,
cerca del inicio del `<body>`.

**Al agregar fotos nuevas:** guárdalas en WebP y a menos de 300 KB cada una.
Una foto de 4 MB salida del teléfono hace que la página tarde una eternidad
en cargar en datos móviles, que es como la va a ver la mayoría de tus clientas.

---

## Datos que faltan

Estos tres aparecen en el pie como "pendiente", en gris:

1. **Dirección del atelier** — busca `pieDir` en el `<script>`
2. **Horarios de atención** — busca `pieHorario`
3. **Correo de contacto** — busca `pieCorreo`

Reemplaza el texto en las dos versiones (español e inglés) y quita la clase
`pendiente` de la etiqueta `<span>` correspondiente en el HTML para que dejen
de verse en gris.

---

## Notas técnicas

- **Idiomas:** español por defecto, botón ES/EN arriba a la derecha. La elección
  se guarda en el navegador de la clienta.
- **Móvil:** barra fija abajo con "Llamar" y "Agendar cita". La mayoría del
  tráfico va a ser desde el teléfono.
- **Galería:** al tocar una foto se abre a pantalla completa. Se navega con las
  flechas del teclado y se cierra con Escape.
- **Peso:** 1.9 MB de fotos en total, pero solo la primera carga de inmediato.
  Las demás se cargan conforme la clienta baja.
- **Accesibilidad:** contraste verificado sobre el fondo negro, navegable por
  teclado, textos alternativos en las 11 fotos.
- **Verificado:** sin errores de JavaScript, sin desbordes horizontales a 390 px,
  768 px y 1440 px, y los 4 paquetes probados en los 29 valores del deslizador.

### Sobre las tipografías

La página usa Cormorant Garamond, Jost y Pinyon Script, que se piden a Google
Fonts. Si el hosting o la conexión los bloquea, se ven tipografías de respaldo
y todo sigue funcionando: solo cambia un poco el aspecto.
