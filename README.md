# Bisturí 3D — Archivos para GitHub

Entrega del 4 de noviembre de 2026. Basada en el código fuente versión 5 de esta conversación, con comprobación de tapa visible y trabajo temporal sin guardar/recuperar proyectos. No integra automáticamente los paquetes separados de otras conversaciones.

## Actualizar desde iPhone

1. Descarga el ZIP y descomprímelo en Archivos.
2. Abre tu repositorio en Safari. En Add file → Upload files, selecciona los archivos descomprimidos de la raíz y confirma la actualización. Si no aparece el menú, solicita la versión de escritorio.
3. Conserva la carpeta vendor y sus archivos. Si el selector no permite subir carpetas, entra en vendor dentro del repositorio y sube allí su contenido. No pongas earcut.js en la raíz.
4. index.html debe quedar en la raíz publicada, junto a app.js, styles.css, manifest.webmanifest y todos los demás módulos. No subas solamente el ZIP: GitHub Pages no lo descomprime.
5. Si Pages ya publica desde main y / (root), conserva esa configuración. Si aún no está configurado, utiliza Settings → Pages → Deploy from a branch, selecciona tu rama y / (root).
6. Abre la dirección de Pages cuando termine la publicación y recarga Safari. El ZIP descargado no ejecuta la aplicación directamente.

No requiere npm ni compilación. Requiere conexión para las dependencias Three.js existentes en esm.sh. Las rutas locales son relativas para funcionar bajo el nombre de tu repositorio. No se han publicado cambios automáticamente.

Los encastres usan three-bvh-csg 0.0.17 desde esm.sh solo al pulsar «Agregar pasador y alojamiento». Si falla esa descarga, la aplicación continúa cargada, muestra un aviso y deja sin aplicar los encastres. La vista previa de cola de milano y las herramientas cilíndricas comparten geometrías corregidas; aún se deben comprobar encastres reales en Safari y en el laminador.

## Qué contiene

Todos los módulos de ejecución, Worker de selección, Worker de corte, CSS, HTML, manifest y triangulador local con su licencia. INVENTARIO.txt enumera los archivos. No contiene tus modelos, texturas privadas, STL de prueba ni archivos temporales. Conserva cualquier CNAME propio de tu repositorio si utilizas dominio personalizado.

## Continuación: ajuste automático de la selección

El motor compara seis desplazamientos cercanos de la superficie y solo intenta una alternativa si mejora la coincidencia por área. La alternativa debe superar las comprobaciones geométricas; si falla, vuelve al ajuste original. Se conserva la revisión visual antes de confirmar.

En el FBX real, la coincidencia pasa de 89,0791 % a 89,3023 % (+0,2232 puntos). Se recuperan 7.640 caras marcadas que antes quedaban fuera, pero entran 5.836 caras no marcadas adicionales: es una mejora del indicador global, no coincidencia exacta. A tiene 713.160 triángulos y dos componentes; B tiene 810.788 triángulos y uno. El informe numérico está en INFORME_AJUSTE.json. La alternativa se elige con 1.199.836 triángulos y se evalúa también en 299.960 triángulos reservados. En esta muestra la coincidencia por área pasa de 89,0663 % a 89,2658 %. Es validación de este ajuste en este modelo, no garantía de otros contornos ni porcentaje de desarrollo.

Se verificó el retorno al ajuste original mediante un fallo simulado del candidato. Ambos nuevos STL se exportaron y reabrieron con VTK sin aristas abiertas o no manifold. La comprobación visual en iPhone continúa pendiente.

## Estado comprobado de la entrega base

La corrección interior de tapa pasó 47 comprobaciones automatizadas en la entrega base. El caso real generó mallas de 698.976 y 823.736 triángulos, reabiertas como STL sin aristas abiertas o no manifold. A tiene dos componentes y B uno. Se verificaron 25.916 muestras. En esta preparación se comprobó sintaxis de todos los módulos y presencia de las dependencias locales.

La fase 2 sigue abierta: el ajuste a la selección coincide por área en 89,1 % en ese ensayo; faltan contornos más fieles, preservación UV/materiales de piezas cortadas y validación visual completa en Safari/iPhone. No se garantiza imprimibilidad de cualquier modelo. No se ha probado esta publicación en tu dispositivo.

## Sesión temporal

No hay guardado ni recuperación de proyectos, autoguardado o historial persistente. Deshacer/Rehacer funciona solo en memoria. Al cerrar o recargar, los cambios se pierden. Descarga las piezas o informes que quieras conservar antes de cerrar o recargar. El aviso al salir solo recuerda esta pérdida; no guarda datos.

El botón «Descargar informe general» reúne el resumen del trabajo y el diagnóstico topológico de cada malla en un solo TXT. «Comprobar piezas» sigue mostrando la revisión en pantalla. El informe de camas y la comprobación del corte curvo siguen siendo descargas independientes.

## Documentación oficial

- https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

La lista de código abierto utilizado y evaluado está en `DEPENDENCIAS_ABIERTAS.md`.

La selección táctil en mallas densas usa opcionalmente three-mesh-bvh 0.9.18; requiere conexión a esm.sh y conserva el método normal si la carga falla. Una prueba con Three.js 0.169 comparó cara y punto antes/después en malla indexada y no indexada; el rendimiento en iPhone no está medido.

## Modelos generados con Hunyuan3D

En la web de Tencent, descarga preferentemente un GLB y ábrelo con «Abrir modelo 3D». Si descargas OBJ, selecciona **juntos** el OBJ, un MTL y las imágenes de textura asociadas en el mismo selector. Solo se admite un modelo y un MTL en cada importación. Las imágenes se enlazan por nombre de archivo, permanecen locales y se liberan de la memoria temporal al terminar su carga. Si faltan imágenes, el modelo puede abrirse sin parte de sus texturas.

Antes de cortar, revisa «Diagnóstico al importar». La geometría generada por IA puede tener bordes abiertos o piezas separadas; el aspecto visual no acredita un sólido imprimible. Esta integración no ejecuta el modelo generativo de Tencent ni incluye sus pesos o código. La disponibilidad del servicio gratuito y los derechos sobre sus resultados se rigen por las condiciones de Tencent, que deben consultarse para cada uso. El STL exportado no conserva las texturas del OBJ/GLB.

## Fase 2: apariencia del corte por plano

El corte por plano conserva UV, UV adicionales, colores de vértices y grupos de materiales exteriores. Interpola los atributos en los nuevos vértices; añade un material independiente para la tapa. Las normales se recalculan, por lo que el sombreado puede variar respecto al original. Una comprobación con una caja de seis materiales verificó ambas mitades y las UV interpoladas en el borde. No se ha comprobado visualmente una textura real en Safari. El corte curvo sigue pendiente de conservación de atributos; esta mejora no cierra toda la fase 2. El STL exportado continúa sin texturas.

En la intersección del corte plano, los vértices a una distancia menor o igual que la tolerancia se llevan al plano antes de construir los segmentos y recortar las dos mitades. El contorno y la geometría lateral comparten así la misma clasificación. También se incluyen en el contorno las aristas completas que coinciden con el plano. Una prueba con una caja verificó en dos posiciones (junto a un vértice y sobre una arista) el contorno cerrado, dos caras por arista y la conservación de volumen. Si una cara entera coincide con el plano, el programa pide mover el corte; las intersecciones curvas degeneradas todavía requieren tratamiento adicional.

El corte curvo usa los vértices de valor cero como puntos del contorno y reconoce una arista coincidente cuando sus dos caras vecinas quedan en lados opuestos. Si la distancia del cruce a un vértice es menor que la resolución Float32 local y que una fracción limitada del tamaño del modelo, reutiliza ese vértice; si un cruce no representable escapa a ese margen, solicita mover ligeramente el corte. Una prueba con caja verificó cortes ordinarios, por vértice, por arista y casi coincidentes con vértice, con piezas cerradas y volumen conservado. Las caras enteras coincidentes y otros contactos tangentes o degenerados todavía requieren tratamiento adicional.

En superficies cuadráticas se revisan los extremos interiores de todas las aristas, también las de caras cuyos vértices quedan del mismo lado. Una tangencia o un doble cruce en una arista se detecta antes de construir las tapas y solicita desplazar o subdividir el corte. Una prueba con caja verificó ambos rechazos junto a los cortes válidos anteriores. Este control impide aceptar un contorno ambiguo; aún no subdivide automáticamente esa zona.
