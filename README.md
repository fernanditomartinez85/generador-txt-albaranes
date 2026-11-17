# Generador de TXT de albaranes

Herramienta web, en un único archivo HTML, que convierte los informes de albaranes de varios proveedores en el fichero TXT de ancho fijo (109 caracteres por línea) que pide el sistema de facturación. Además, asigna a cada línea su centro de coste.

La hice para automatizar una tarea que repetía en mi trabajo cada quincena.

## El problema

Cada quincena llegaban los informes Excel de los proveedores. Con los de octubre de 2026 como referencia, eran unos **190 bloques de centro y 3.500 líneas** entre las dos empresas. El proceso a mano era este:

1. Copiar el código de cada centro (por ejemplo EMB001) en todas las líneas de su bloque, hasta llegar al siguiente centro.
2. Borrar las filas con el nombre del centro y las de «TOTAL CLIENTE» (unas 340 por quincena).
3. Quitar las columnas que sobran y pegar el resultado en una plantilla Excel.
4. Sacar de la plantilla el TXT de 109 caracteres por línea.

## Qué hace

- **Lee los informes de tres proveedores con formatos distintos:** informe por bloques de centro, listado de albaranes y sumario de facturación. Las columnas se localizan por el texto de su cabecera, no por su posición.
- **Asigna el centro de coste** a cada línea a partir de la hoja ALMACENES del libro maestro.
- **Gestiona los centros:** detecta los nuevos y permite darlos de alta, y además buscar, modificar o quitar centros y guardar el Excel actualizado.
- **Avisa antes de generar:** centros sin albaranes, productos sin precio y filas con datos incompletos. Si se decide seguir, crea un TXT parcial sin esas filas, siempre después de pedir confirmación.
- **Valida el resultado:** cada campo debe tener su ancho exacto y cada línea, 109 caracteres. Nunca se inventan precios ni cantidades.
- **Todo ocurre en el navegador:** ningún archivo sale del equipo.

### Formato de cada línea (109 caracteres)

| Campo | Ancho | Campo | Ancho |
|---|---|---|---|
| Código de proveedor | 6 | Cantidad × 100 | 10 |
| Código de centro o cliente | 10 | Signo de la cantidad | 1 |
| Albarán o pedido | 10 | Descuento | 4 |
| Fecha (AAAAMMDD) | 8 | Empresa | 3 |
| Artículo | 15 | Centro de coste | 6 |
| Unidad | 3 | Relleno con ceros | 15 |
| Precio × 1000 | 11 | Código de proveedor | 6 |
| Signo del precio | 1 | | |

## Resultado

| | Por quincena |
|---|---|
| A mano | 45-70 minutos |
| Con la herramienta | unos 5 minutos |

El ahorro es de **1,5 a 2 horas al mes, unas 18-26 horas al año (unas 3 jornadas de trabajo)**. A eso se suma que desaparecen los errores de copiar y borrar filas a mano en ficheros de miles de líneas.

## Cómo se usa

1. Abrir `index.html` en el navegador (Edge o Chrome). No hace falta instalar nada.
2. Elegir el proveedor y la empresa.
3. Arrastrar los informes del proveedor y el libro Excel con la hoja ALMACENES.
4. Escribir la fecha de exportación y pulsar **Generar TXT de 109 caracteres**.
5. Si hay avisos, revisarlos; después, descargar el TXT.

## Tecnología

HTML, CSS y JavaScript, sin servidor ni instalación. Para leer y escribir Excel lleva integrada la librería [SheetJS](https://sheetjs.com/) (licencia Apache 2.0; el texto de la licencia está dentro de la página).

## Nota

Esta es una versión pública anonimizada: los nombres de las empresas y de los proveedores, y los códigos internos, se han sustituido por otros genéricos (Proveedor A/B/C, Empresa A/B, 000001…). La lógica es la misma que la de la versión que uso en el trabajo.
