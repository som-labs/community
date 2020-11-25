---
layout: post
title: Novedades en la API de open data: Filtros por grupos locales y API de introspección
---


La nueva version 0.2.10 de la API, compatible hacia atrás con la anterior, añade dos nuevas funcionalidades:

- Se ha añadido un nuevo nivel geográfico para los filtros: el grupo local.

- Se han añadido nuevas consultas para obtener información sobre los diferentes niveles geográficos y métricas que se soportan.
  Las aplicaciones usuarias podrán usar estas consultas para adaptarse a futuras ampliaciones sin necesidad de cambiar su código.

Con la nueva API, a parte de bajarnos datos y mapas, ahora podemos averiguar, por ejemplo, qué grupos locales hay en una provincia o qué municipios están incluidos en un grupo local.

En las próximas semanas seguiremos trabajando para adaptar [la UI](https://opendata.somenergia.coop) a estas nuevas funcionalidades
pero la API y la documentación ya está publicada para que cualquiera pueda crear aplicaciones con ella.
Para nutrir la inspiración os damos alguos ejemplos de uso a continuación.

## Ejemplos de uso

Podemos saber los diferentes niveles geograficos que existen y donde los podemos usar (en mapas, como filtros o como nivell de detalle):

<https://opendata.somenergia.coop/v0.2/discover/geolevel>

```yaml
geolevels:
- text: Global
  mapable: false
  id: world
- text: País
  plural: countries
  parent: world
  mapable: false
  id: country
- text: Comunidad Autonoma
  plural: ccaas
  parent: country
  id: ccaa
[...]
- text: Grupo Local
  plural: localgroups
  parent: world
  detailed: false
  mapable: false
  id: localgroup
```

Vemos aparecen los que ya teniamos: country, ccaa, state (provicia), city, Y, además, el nuevo nivel de grupos locales (localgroup). Podemos saber que grupos locales hay con:

<https://opendata.somenergia.coop/v0.2/discover/geolevel/localgroup>

```yaml
options:
  Alacant: Alacant
  Almeria: Almería
  AltPenedes: Alt Penedès
  Anoia: Anoia
  [...]
```

Si queremos ver los grupos locales que hay solo en barcelona, se pueden filtrar:

<https://opendata.somenergia.coop/v0.2/discover/geolevel/localgroup?state=08>

```yaml
options:
  AltPenedes: Alt Penedès
  Anoia: Anoia
  Badalona: Badalona
  Bages: Bages
  BaixLlobregat: Baix Llobregat
  BaixMontseny: Baix Montseny
[...]
```

Si no sabías que el codigo INE de Barcelona era el 08, podrías haber consultado los códigos de provincia con:

<https://opendata.somenergia.coop/v0.2/discover/geolevel/state>

```yaml
options:
  None: None
  '01': Araba/Álava
  '02': Albacete
  '03': Alicante/Alacant
  '04': Almería
  '05': Ávila
  '06': Badajoz
  08: Barcelona
  09: Burgos
[...]
```

Obtendremos los municipios que estan dentro del ámbito de un grupo local con:

<https://opendata.somenergia.coop/v0.2/discover/geolevel/city?localgroup=BaixLlobregat>

```yaml
options:
  08001: Abrera
  08020: Begues
  08056: Castelldefels
  08066: Castellví de Rosanes
  08068: Cervelló
  08069: Collbató
  08072: Corbera de Llobregat
  08073: Cornellà de Llobregat
  08076: Esparreguera
[...]
```

Para saber que métricas soportamos.

<https://opendata.somenergia.coop/v0.2/discover/metrics>

```yaml
metrics:
- id: members
  text: Members
- id: contracts
  text: Contracts
```

De momento solo hay dos métricas: número de socias (members) y contratos (contracts) pero prometemos añadir muchas mas: produccción, uso de energia...

Y, finalmente, si quieres obtener la evolucion de socias mensual de tu grupo local desgranado por municipios:

<https://opendata.somenergia.coop/v0.2/members/by/city/monthly?localgroup=BaixLlobregat>

```yaml
dates:
- 2010-01-01
- 2010-02-01
- 2010-03-01
- 2010-04-01
- 2010-05-01
- 2010-06-01
[...]
- 2020-10-01
- 2020-11-01
values:
- 0
- 0
[...]
- 3021
- 3035
countries:
  ES:
    name: España
    values:
    - 0
    - 0
    [...]
    - 3021
    - 3035
    ccaas:
      09:
        name: Cataluña
        values:
        - 0
        - 0
        [...]
        - 3021
        - 3035
        states:
          08:
            name: Barcelona
            values:
            - 0
            - 0
            [...]
            - 3021
            - 3035
            cities:
              08001:
                name: Abrera
                values:
                - 0
                - 0
                [...]
                - 46
                - 46
              08020:
                name: Begues
                values:
                - 0
                - 0
                [...]
                - 110
                - 110
              08056:
                name: Castelldefels
                values:
                - 0
                - 0
                [...]
                - 176
                - 177
```

Los grupos locales aun no estan como detalle en los datos ni como division en los mapas, solo como filtros.

### Mapas?

Si, esto se añadió en anteriores versiones.
Si aparte de números quieres ver el mapa:

<https://opendata.somenergia.coop/v0.2/map/members/by/ccaa>
![](https://opendata.somenergia.coop/v0.2/map/members/by/ccaa)

<https://opendata.somenergia.coop/v0.2/map/members/by/state>
![](https://opendata.somenergia.coop/v0.2/map/members/by/state)

También están las versiones animadas y relativas

<https://opendata.somenergia.coop/v0.2/map/members/per/population/by/ccaa/yearly>
![](https://opendata.somenergia.coop/v0.2/map/members/per/population/by/ccaa/yearly)

<https://opendata.somenergia.coop/v0.2/map/members/per/population/by/state/yearly>
![](https://opendata.somenergia.coop/v0.2/map/members/per/population/by/state/yearly)


## Créditos

La API ha sido posible gracias al trabajo de las siguientes personas del Equipo Técnico de Som Energia: 
Mar Jenè,
Benjamí Ramos,
Joana Figueira,
Pol Monsó,
Alberto Rastrillo,
David Riera,
David García
y con el soporte de Jordi Pons en los sistemas.


