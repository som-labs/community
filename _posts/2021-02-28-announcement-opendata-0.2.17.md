---
layout: post
title: "Novedades en la API Open Data: Más métricas y nueva UI"
author: Som Energia IT
tags: opendata
featured-image: assets/images/announcement-opendata-0.2.13.png
image: assets/images/announcement-opendata-0.2.13.png
---


![Pantallazo del anuncio](/community/assets/images/announcement-opendata-0.2.13.png){:style="width:100%;margin:auto;padding:1em"}

Coincidiendo con el encuentro de grupos locales de Som Energia de 2021,
anunciamos la nueva version 0.2.17 de la API Open Data de Som Energia.
Incluye todas las mejoras añadidas en una serie de versiones cortas
que se han ido haciendo desde el 
[anuncio de la 0.2.10]({% link _posts/2021-02-28-announcement-opendata-0.2.10.md %}),
hace solo un mes:

- Métricas:
	- **Nueva métrica:** Contratos de **autoconsumo**
	- 6 métricas derivadas para **altas y bajas** de las 3 métricas principales: socias, contratos y autoconsumo
	- Se ha **corregido** cómo se obtenía la **fecha de alta de las socias**, a veces salían fechas muy posteriores
	- `/discovery/metrics` incluye una descripción que explica el **significado**, **fuente** y **limitaciones** de cada métrica.
- [Nueva UI basada en React](https://opendata.somenergia.coop/ui) con nuevas funcionalidades.
	- Usa la **api de descubrimiento** para adaptarse a futuras métricas y niveles geográficos
	- Nuevo **selector de filtros geográficos** mucho más práctico
	- Mejora de la visualización de **tablas**
	- **Descarga** de datos como **hoja de cálculo** (formato CSV)
	- **Resaltado de syntaxis** en las vistas YAML y JSON
	- **Selector del idioma** de la interficie (no solo de la salida)
	- Enlace al **GapMinder** en el menú
- Los **mapas animádos** se generan **más rápido** y
  se pueden ampliar **sin pixelarse** (son SVG's animados en vez de GIF's)
- Especificación en **OpenAPI 3.0** y un punto de entrada `/spec` para obtenerla
- GapMinder
	- Obtiene los datos de la API directamente (hasta ahora trabajaba con una copia descargada)
	- También usa la API de descubrimiento para adaptarse a las métricas nuevas

## Métrica de autoconsumo

Una petición de los grupos locales era conocer el progreso del autoconsumo en su zona.
Esta métrica recoge los contratos que tienen activado el autoconsumo.
Considera la fecha de activación que nos ofrece la distribuidora.
Cuando un contrato pasa a Som Energia ya con auto consumo,
se considera la fecha del cambio a SomEnergia.
Por últmo, aclarar también, que se cuentan todos los contratos de autoconsumo,
no solo los que se consiguen con compras colectivas de los Grupos Locales.

![Métricas de autoconsumo en el GapMinder](/community/assets/images/pantallada-opendata-gapminder-autoconsum.png){:style="width:100%;margin:auto;padding:1em"}


## Nuevas métricas derivadas: Altas y bajas

En el cómputo total de contratos y socias, las bajas
quedaban enmascaradas con las altas que solían ser muchas más.
Sin embargo, es interesante conocer este flujo para estar alerta de tendencias negativas.
Las 6 métricas derivadas nuevas separan las altas y bajas para poder detectar mejor
en que momentos y lugares ha habido bajas.

Las altas, sin acomulación ni bajas, dan una idea más clara
del efecto de las acciones de difusión que hace el grupo local,
durante el mes o el año.

## Migración de React de la interfície web

En Som Energia estamos migrando a React todas las interfices web,
(formulario de contratación modificación de contratos...)
que estaban en Angular o Mithril.
Le ha tocado el turno a la inteficie web de OpenData.
Podremos aprovechar la mayor disponibilidad de
librerias, herramientas y comunidad en esa plataforma.

Quedaría pendiente, para próximas versiones, migrar el ejemplo [GapMinder].

[GapMinder]: https://opendata.somenergia.coop/ui/gapminder.html


## Interficie adaptable usando el API de descubrimiento

El nuevo front-end de usuario utiliza la introspección del API de descubrimiento (_discovery_)
para adaptarse a las nuevas métricas que vayamos añadiendo.
Por eso todas las nuevas métricas ya se pueden usar en la interfaz nueva.

También se usa para saber qué niveles geográficos se pueden usar para cada cosa:
Como nivel de detalle en los mapas o en los datos numéricos o como filtro.

El código de la nueva interfície, que está [liberada en github](https://github.com/som-energia/opendata-ui),
también puede servir de ejemplo de como usar la introspección en vuestras aplicaciones.


## Editor de filtros geográficos

Para filtrar los datos por CCAA o província,
la anterior interfície te forzaba a averiguar el código INE y escribirlo en formato técnico (url query string).
Con el nuevo editor, solo hay que empezar a escribir el nombre y aparecen las opciones posibles.
En las opciones se incluyen, marcados, los diferentes niveles geográficos.
Por ejemplo, si escribimos `Barcelona` nos ofrece como opción para filtrar
la provincia, la ciudad y el grupo local.

Se pueden añadir varios filtros que aparecen como pildoras que puedes tambien eliminar.

![Geofilters](/community/assets/images/pantallada-opendata-geofilters.png)


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
como ya pasaba con los mapas no animados.


![Ejemplo de Mapa Animado](http://opendata.somenergia.coop/v0.2/map/newmembers/by/province/monthly){:style="width:100%;margin:auto;padding:1em"}


## OpenAPI 3.0

Hemos generado una especificación de la API siguiendo el estandard [OpenAPI 3.0]
y está disponible en el punto de entrada `/spec`.

[Open API 3.0]: https://es.wikipedia.org/wiki/Especificaci%C3%B3n_OpenAPI

Esa especificación nos ayudara a substituir la documentación antigua
basada en APIDoc que se estaba quedando anticuada.
Hay muchas herramientas de documentación y generación de código para APIDoc
que ahora podremos usar.

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


