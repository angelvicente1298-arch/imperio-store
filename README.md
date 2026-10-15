# Imperio Shop — Tienda

Sitio oficial de **Imperio Shop**: bots para Discord, hosting y páginas web.

## Direcciones

| Sitio | Dirección |
| --- | --- |
| Tienda | https://imperioshopp.com |
| Verificación de cuentas | https://verificacion.discord.imperioshopp.com |
| Términos y privacidad | https://terminos.imperioshopp.com |
| Tienda de pagos (Tebex) | https://imperioshop.tebex.io |

## Cómo está montado

La tienda es **estática** (HTML, CSS, JavaScript e imágenes) y se sirve desde el
borde de Cloudflare. No depende de ningún servidor encendido: si este repositorio
responde, la tienda funciona.

```
Visitante  →  Cloudflare (borde)  →  repositorio de este proyecto
                    ↓
             copia guardada en el borde
```

El Worker que la sirve (`imperio-web`):

- Responde a las rutas de la tienda (`/`, `/servicios`, `/planes`, …) con `index.html`.
- Sirve los archivos reales (`/style.css`, `/script.js`, `/assets/…`) con su tipo correcto.
- Guarda cada archivo en el borde: **5 minutos** el HTML y **1 hora** las imágenes.
- Si una fuente falla, prueba automáticamente la siguiente (dos direcciones de respaldo).
- `/estado` devuelve el estado de salud y la versión, útil para el vigilante de caídas.

## Catálogo y precios

Los precios están en la sección `id="planes"` de `index.html`, agrupados en tres
pestañas: **Bots para Discord**, **Hosting** y **Páginas web**.

| Servicio | Plan | Precio |
| --- | --- | --- |
| Bots para Discord | Bot Básico | $8 |
| Bots para Discord | Bot Avanzado | $16 |
| Bots para Discord | Bot Pro | $28 |
| Hosting | Hosting para bots | $5 |
| Páginas web | Web Inicial | $16 |
| Páginas web | Web Profesional | $32 |
| Páginas web | Web Avanzada | $48 |
| Páginas web | Web Premium | $64 |

Son **pagos únicos** en dólares (USD), sin impuestos locales incluidos.

### Cómo cambiar un precio

Busca el nombre del plan en `index.html` y cambia el número:

```html
<span class="plan__amount">16</span>
```

Para añadir un plan nuevo, copia un bloque `<article class="plan ...">` completo
y edita el nombre, la descripción, el precio y las características. Si el panel
llega a tener cuatro tarjetas, usa la rejilla `grid--4`.

## Pagos (Tebex)

- La tienda de Tebex es **https://imperioshop.tebex.io** (nombre interno:
  *Mega_imperio*, cuenta **1906919**).
- El botón **Comprar en Tebex** de cada plan lleva a esa dirección.
- El sitio carga el checkout oficial (`https://js.tebex.io/v/1.js`), que permite
  pagar con tarjeta, PayPal o Google Pay sin salir de la web.

> **Importante:** la tienda de Tebex está hoy **inactiva** («The store is
> currently inactive»). Hasta que se active desde el panel de Tebex, los botones
> llevarán a esa página de aviso. Hay que entrar en el panel de Tebex y poner la
> tienda en modo activo (requiere completar la verificación de la cuenta).

## Discord

La invitación del servidor está en **`script.js`**, en la primera línea:

```js
const CONFIG = { discordInvite: "discord.gg/cffbceYF2", ... }
```

Ese valor se aplica a todos los botones «Unirse al Discord» y al botón de copiar.

> La invitación anterior (`discord.gg/imperioshop`) **ya no existe**: daba error
> 404. Si algún día caduca, crea una nueva en Discord (sin fecha de caducidad ni
> límite de usos) y cambia ese valor.

## Archivos

```
index.html            Página completa de la tienda
style.css             Estilos
script.js             Interacción: menú, contadores, pestañas del catálogo y copiar la invitación
assets/
  logo-mark.webp      Emblema de la corona
  logo.webp           Logotipo vertical
  logo-patern.webp    Patrón de fondo
  banner.webp         Banner principal
  banner@2x.webp      Banner para pantallas de alta densidad
  og-image.jpg        Imagen para compartir en Discord y redes
  favicon-*.png       Iconos del navegador
```

## Para actualizar la tienda

1. Editá el archivo que corresponda (`index.html`, `style.css`, `script.js` o algo dentro de `assets/`).
2. Subilo al repositorio:

   ```bash
   git add -A
   git commit -m "Actualizo la tienda"
   git push origin main
   ```

3. Para que el cambio se vea **al instante** en `imperioshopp.com`, hay que subir
   el número `VERSION` del Worker (`imperio-web`) y limpiar la copia guardada.
   Sin ese paso, el HTML tarda hasta 5 minutos y las imágenes hasta 1 hora.

## Estructura de la página

1. **Cabecera** fija con menú y botón de Discord
2. **Inicio** con el titular, el banner y las estadísticas
3. **Servicios** (seis tarjetas)
4. **Panel del bot** con la consola interactiva
5. **Catálogo** con las tres pestañas de precios
6. **Preguntas frecuentes**
7. **Contacto** y **pie** de página

## Imágenes

Las imágenes están en **WebP** (con respaldo PNG para los iconos), lo que bajó su
peso de 541 KB a 132 KB sin perder calidad visible. Si agregás imágenes nuevas,
conviene respetar esos formatos para mantener la tienda liviana.

## Marca

| Color | Uso |
| --- | --- |
| `#e11d48` | Rojo principal |
| `#ff7b00` | Naranja de acento |
| `#08080a` | Fondo oscuro |

Tipografías: **Outfit** para títulos, **Inter** para el texto y **JetBrains Mono**
para los detalles técnicos.

## Detalles técnicos

- Sin dependencias ni proceso de compilación: son tres archivos sueltos.
- Respeta `prefers-reduced-motion`: si el visitante pidió menos movimiento en su
  sistema, las animaciones se desactivan.
- Adaptado a celular, tableta y escritorio.
