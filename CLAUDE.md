# CLAUDE.md

Este archivo brinda orientación a Claude Code (claude.ai/code) al trabajar con el código de este repositorio.

## Descripción general del repositorio

Este repo es una landing page estática de una sola página para **ModLab Mérida**, un negocio local de modificación/reparación de PS4 por software en Mérida, Yucatán, México. Se despliega mediante GitHub Pages (URL canónica `https://demoniodeb.github.io/PS-JailBreak/`). No hay backend, ni sistema de build, ni gestor de paquetes — todo el sitio es un único archivo HTML autocontenido más un puñado de imágenes estáticas.

## Estructura del repositorio

- `index.html` — el sitio completo: markup, CSS en `<style>`, datos estructurados (JSON-LD) y JS vanilla, todo inline en un solo archivo.
- `assets/` — favicons e iconos táctiles (`favicon-16.png`, `favicon-32.png`, `favicon-48.png`, `apple-touch-icon.png`). Nota: `index.html` referencia `assets/og-image.jpg` para las previsualizaciones de Open Graph/Twitter card, pero ese archivo actualmente no existe en `assets/`.

## Flujo de trabajo de desarrollo

No hay paso de build, lint, tests ni instalación de paquetes — es HTML/CSS/JS plano sin dependencias.

- **Previsualizar en local**: abre `index.html` directamente en el navegador, o sirve el directorio (p. ej. `python3 -m http.server`) desde la raíz del repo para que las rutas relativas de los assets resuelvan correctamente.
- **Despliegue**: al hacer push a la rama que GitHub Pages tenga configurada para servir (revisa Settings → Pages del repo) el sitio se publica automáticamente; no hay un paso de build/deploy separado que ejecutar.
- Al editar, verifica los cambios abriendo el archivo en el navegador en lugar de depender de una suite de tests — no existe ninguna.

## Convenciones de contenido y estructura

- **Arquitectura de archivo único**: todo el CSS vive en el bloque `<style>` del `<head>`; todo el JS vive en un bloque `<script>` al final del `<body>`. Mantén los nuevos estilos/scripts inline en `index.html` en lugar de separarlos en archivos distintos, siguiendo la estructura actual.
- **El idioma del sitio es español** (`<html lang="es">`) y está dirigido a una audiencia mexicana (locale `es_MX`, moneda MXN). Mantén todo el copy de cara al usuario en español salvo indicación contraria.
- **Layout basado en secciones**: la página es una secuencia de bloques `<section>` con `id`s descriptivos (`beneficios`, `paquetes`, `ubicacion`, `garantia`, `faq`, `contacto`) usados tanto para los enlaces de navegación/anclas como límites naturales de edición — la mayoría de los cambios de contenido apuntan a una sola sección.
- **Variables CSS**: colores, espaciados y radios están centralizados en propiedades personalizadas `:root` (`--bg`, `--cyan`, `--green-wa`, `--text`, etc. al inicio del bloque `<style>`). Reutiliza estas variables en vez de hardcodear nuevos colores.
- **Selectores de precio/paquete**: las tarjetas de paquete "Con Disco Interno" e "Interno + Externo" tienen píldoras de capacidad interactivas (botones `.pill` con atributos `data-price`/`data-label`) conectadas mediante dos IIFEs casi idénticas al final del script. Cada IIFE actualiza el precio mostrado (vía `fmt()`), la nota descriptiva y el texto/URL del deep-link de WhatsApp para ese paquete. Al agregar una nueva opción de capacidad, añade un botón `.pill` con el `data-price`/`data-label` correcto en el `.pill-row` correspondiente; al agregar un nuevo nivel de paquete, replica una de estas IIFEs existentes en lugar de inventar un patrón nuevo.
- **WhatsApp es el único canal de contacto/CTA**: cada enlace de llamada a la acción apunta a `https://wa.me/529991558206` con un mensaje pre-rellenado y codificado en URL (`?text=...`). Los nuevos CTAs deben seguir este mismo patrón de deep-link `wa.me` con un mensaje debidamente codificado.
- **Metadatos SEO/redes sociales**: el `<head>` contiene Open Graph, Twitter Card y datos estructurados JSON-LD (`LocalBusiness`). Si los precios, el teléfono, la dirección o la descripción del servicio cambian en el contenido visible, actualiza también los valores correspondientes en el bloque JSON-LD y en las meta tags para que se mantengan sincronizados.
