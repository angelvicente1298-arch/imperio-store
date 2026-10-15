# Imperio Shop — Tienda

Sitio oficial de **Imperio Shop**: bots para Discord, hosting y servidores.

## Direcciones

| Sitio | Dirección |
| --- | --- |
| Tienda | https://imperioshopp.com |
| Verificación de cuentas | https://verificacion.discord.imperioshopp.com |
| Términos y privacidad | https://terminos.imperioshopp.com |

## Cómo está montado

La tienda es **estática** (HTML, CSS, JavaScript e imágenes) y se sirve desde el
borde de Cloudflare. No depende de ningún servidor encendido: si este repositorio
responde, la tienda funciona.

```
Visitante  →  Cloudflare (borde)  →  repositorio de este proyecto
                    ↓
             copia guardada 24 h
```

El Worker que la sirve:

- Responde a las rutas de la tienda (`/`, `/servicios`, `/planes`, …) con `index.html`.
- Sirve los archivos reales (`/style.css`, `/script.js`, `/assets/…`) con su tipo correcto.
- Guarda cada archivo en el borde durante 24 horas, así las visitas siguientes son instantáneas.
- Si una fuente falla, prueba automáticamente la siguiente (tres direcciones de respaldo).
- `/estado` devuelve el estado de salud, útil para el vigilante de caídas.

## Archivos

```
index.html            Página completa de la tienda
style.css             Estilos (minificado)
script.js             Interacción: menú, planes, preguntas frecuentes
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
   git push
   ```

3. En un minuto los visitantes ya ven el cambio. Para forzarlo al instante, pedile
   al asistente que limpie la copia guardada.

## Imágenes

Las imágenes se convirtieron a **WebP** (con respaldo PNG para los iconos), lo que
bajó su peso de 541 KB a 132 KB sin perder calidad visible. Si agregás imágenes
nuevas, conviene respetar esos formatos para mantener la tienda liviana.

## Marca

| Color | Uso |
| --- | --- |
| `#e11d48` | Rojo principal |
| `#ff7b00` | Naranja de acento |
| `#08080a` | Fondo oscuro |

Tipografías: **Outfit** para títulos, **Inter** para el texto y **JetBrains Mono**
para los detalles técnicos.
