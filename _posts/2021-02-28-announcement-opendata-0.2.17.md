---
layout: post
title: "Novedades en la API Open Data: Más métricas, nueva UI y más"
author: Som Energia IT
tags: opendata
featured-image: assets/images/announcement-opendata-0.2.13.png
image: assets/images/announcement-opendata-0.2.13.png
---


![Pantallazo del anuncio]({{site.baseurl}}/assets/images/2021-02-diapo-presentacion-grupos-locales.png){:style="width:100%;margin:auto;padding:1em"}

URL: {{site.url}} Base: {{site.baseurl}}

Coincidiendo con el [encuentro de grupos locales de Som Energia de 2021](),
anunciamos la nueva **version 0.2.17** de la API Open Data de Som Energia.
Es la última versión de una serie de versiones cortas
que se han hecho desde la [versión 0.2.10 que se hizo en noviembre]({% link _posts/2020-11-25-announcement-opendata-0.2.10.md %}).
El objetivo era llegar al evento con funcionalidades útiles para los grupos locales,
enfatizando en **la usabilidad para personas no programadoras**,
y la incorporación de métricas útiles para **evaluar el impacto
de las actividades de difusión** que realizan los grupos locales. 

Podeis ver el [vídeo de la presentación a los grupos locales](https://drive.google.com/file/d/1u-XnYfDzmvkZifF51Zy8H4Wqxj9QFtDA/view?usp=sharing).

Desde un punto de vista más técnico,
éste sería el resúmen de las funcionalidades,
algunas de las cuales ampliamos después con mas detalle:

- Métricas:
	- **Nueva métrica:** Contratos de **autoconsumo**
	- 6 métricas derivadas para **altas y bajas** de las 3 métricas principales: socias, contratos y autoconsumo
	- **Correción:** la **fecha de alta de las socias** a veces eran muy posteriores a la real porque se obtenía del sitio incorrecto
	- `/discovery/metrics` incluye una **descripción** que explica el **significado**, **fuente** y **limitaciones** de cada métrica.
- [Nueva UI basada en React](https://opendata.somenergia.coop/ui) con nuevas funcionalidades.
	- Usa la **api de descubrimiento** para adaptarse a futuras métricas y niveles geográficos
	- Nuevo **selector de filtros geográficos** mucho más práctico
	- Mejora de la visualización de **tablas**
	- **Descarga** de datos como **hoja de cálculo** (formato CSV)
	- **Resaltado de syntaxis** en las vistas YAML y JSON
	- **Selector del idioma** en el menú para la interficie
	- Enlace al **GapMinder** en el menú
- Nuevos **mapas animádos**
	- Son SVG's animados en vez de GIF's
	- Se generan **más rápido**
	- Se pueden ampliar **sin pixelarse**
- Especificación en **OpenAPI 3.0**
	- Se ha añadido un punto de entrada `/spec` para obtenerla
	- **Nueva documentación** basada en la especificación, substituyendo a la antigua basada en APIDoc
- **GapMinder**
	- Obtiene los datos **de la API en vivo** (hasta ahora trabajaba con una copia descargada)
	- También usa la **API de descubrimiento** para adaptarse a las métricas nuevas

## Métrica de autoconsumo

Una petición de los grupos locales era conocer el progreso del autoconsumo en su zona.
Esta métrica recoge los **contratos que tienen activado el autoconsumo**.
Considera la fecha de activación que nos ofrece la distribuidora.
Cuando un contrato pasa a Som Energia ya con auto consumo,
se considera la fecha del cambio a SomEnergia.

Por últmo, aclarar que se cuentan todos los contratos de autoconsumo,
sean o no fruto de compras colectivas de los Grupos Locales.

Aquí vemos en el GapMinder enfrentadas las métricas de contratos y de contratos de autoconsumo.
El tamaño de los puntos está controlado por el crecimiento mensual en autoconsumo.

![Métricas de autoconsumo en el GapMinder]({{site.baseurl}}/assets/images/pantallada-opendata-gapminder-autoconsum.png){:style="width:100%;margin:auto;padding:1em"}


## Nuevas métricas derivadas: Altas y bajas

En el cómputo total de contratos y socias, las **bajas**
quedaban enmascaradas con las altas que solían ser muchas más.
Sin embargo, es interesante conocer este flujo para estar **alerta de tendencias negativas**.

Por otro lado, las **altas** durante un mes, sin sumar el acomulado
y sin contar las bajas, da una idea más limpia
del **efecto de las acciones de difusión** que hace el grupo local,
durante el mes o el año.

Por ello de las tres métricas básicas (contratos, socias y contratos de autoconsumo)
se han derivado 6 nuevas métricas derivadas que separan las altas y bajas
Podremos detectar mejor en qué momentos y lugares se han producido.

## Migración de React de la interfície web

Se estan migrando al framework [React]
todas las interfices web de Som Energia,
que estaban hechas en [Angular] o [Mithril].
Por ejemplo,
los formularios de contratación,
de modificación de contratos...
Esta vez le ha tocado el turno a la
[inteficie web de OpenData](https://opendata.somenergia.coop/ui).

Con React podremos aprovechar la mayor disponibilidad de
librerias, herramientas y comunidad en esa plataforma.

[React]: https://reactjs.org/
[Angular]: https://angularjs.org/
[Mithril]: https://mithril.js.org/


Quedaría pendiente, para próximas versiones, migrar el ejemplo [GapMinder].

[GapMinder]: https://opendata.somenergia.coop/ui/gapminder.html


## Interficie adaptable usando el API de descubrimiento

El nuevo front-end de usuario utiliza la introspección del API de descubrimiento (_discovery_)
para adaptarse a las nuevas métricas que vayamos añadiendo.
Por eso todas las nuevas métricas ya se pueden usar en la interfaz nueva.

![Métricas obtenidas de la API discover]({{site.baseurl}}/assets/images/pantallada-opendata-ui-discover-metrics.png)

También se usa para saber qué niveles geográficos se pueden usar para cada cosa:
Como nivel de detalle en los mapas o en los datos numéricos o como filtro.
En los mapas se usa esa información para deshabilitar
las opciones no disponibles:

![Deshabilitando los niveles geográficos que no se pueden usar en el mapa]({{site.baseurl}}/assets/images/pantallada-opendata-ui-discover-geolevels.png)

El código de la nueva interfície, que está [publicado en github](https://github.com/som-energia/opendata-ui).
Puedes usarlo de ejemplo para usar la introspección en vuestras aplicaciones.


## Editor de filtros geográficos

Para filtrar los datos por CCAA o província,
la anterior interfície te forzaba a averiguar el código INE y escribirlo en formato técnico (url query string).
Con el nuevo editor, solo hay que empezar a escribir el nombre y aparecen las opciones posibles.
En las opciones se incluyen, marcados, los diferentes niveles geográficos.
Por ejemplo, si escribimos `Barcelona` nos ofrece como opción para filtrar
la provincia, la ciudad y el grupo local.

Se pueden añadir varios filtros que aparecen como pildoras que puedes tambien eliminar.

![Geofilters]({{site.baseurl}}/assets/images/pantallada-opendata-geofilters.png)


## Mejor visualización de los datos

El YAML y el JSON están bien para procesarlos,
incluso se le puede añadir, como hemos hecho, resaltado de sintaxis.
Pero una tabla es mucho mejor, así que hemos mejorado como se ven
los datos.

Tanto el resaltado de sintaxis como la nueva tabla de datos van un poco lentos
en las consultas más grandes (por municipios, mes a mes).
És lo siguiente que vamos a atacar.

## Mapas animados en SVG

La generación de GIFs para los mapas animados era un cuello de botella bastante crítico de la API.
Generando SVG's animados no solo conseguimos servirlos más rápido,
sinó que ademas los mapas se pueden ampliar a cualquier tamaño
sin pérdida de calidad,
igual que los no animados.


![Ejemplo de Mapa Animado](http://opendata.somenergia.coop/v0.2/map/newmembers/by/state/monthly){:style="width:100%;margin:auto;padding:1em"}


## Menú para la documentación, el GapMinder, y idioma

Diversas opciones están disponibles con el menú de la esquina superior derecha:
Acceder a la documentación, acceder al GapMinder, y cambiar el idioma de la interfície.

Igual que las llamadas a las consultas,
la interficie adopta por defecto el idioma
que hay configurado en el navegador.
Pero igual que las consultas permiten forzar un idioma,
ahora tambien podemos forzar un idioma para la interficie desde este menú.


![Ejemplo de Mapa Animado]({{site.baseurl}}/assets/images/pantallada-opendata-ui-menu.png){:style="width:100%;margin:auto;padding:1em"}

## OpenAPI 3.0

Hemos generado una especificación de la API siguiendo el estandard [OpenAPI 3.0]
y está disponible en el punto de entrada `/spec`.

[OpenAPI 3.0]: https://es.wikipedia.org/wiki/Especificaci%C3%B3n_OpenAPI

La [nueva documentación](https://opendata.somenergia.coop/docs/)
ya se ha generado basandonos en esta epecifiación con la herramienta [ReDoc].
De hecho, una de las ventajas de OpenAPI es que podemos usar
diversas herramientas para generar documentacion o código de cliente en diferentes lenguajes.

## GapMinder extensible

Y por último, pero no por eso menos, hemos mejorado el GapMinder.
Hasta ahora usaba una copia de los datos de la API que había que actualizar regularmente.
Ahora llama a la API directamente, y no solo eso,
también usa la API de descubrimiento para adaptarse a las nuevas métricas que van apareciendo.
Ahora tenemos un montón de métricas nuevas con las que jugar y las que vendrán.

También hemos acortado el intervalo de tiempo abarcado
para que empiece justo cuando se funda la cooperativa
y evitar los meses de 2010 en que simplemente no había nada.


## Créditos

La API ha sido posible gracias al trabajo de las siguientes personas del Equipo Técnico de Som Energia: 
Mar Jenè,
Benjamí Ramos,
Joana Figueira,
Pol Monsó,
Alberto Rastrillo,
David Riera,
David García
y con el soporte de Jordi Pons y Joan Sarola en el despliegue.


