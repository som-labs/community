---
name: Petición de API
about: Necesidad de API para que la implemente el Equipo Técnico de Som

---

## Título
* Frase corta que describa la API.

Ejemplo:
> API para obtener el ámbito geogràfico de los grupos locales

## Contexto y necesidad
* Uso inmediato que se le pretende dar
* Qué valor aporta al socio ese uso
* Otras utilidades se pueden preveer en el futuro

Ejemplo:
> Estamos implementando un widget para los blogs de los Grupos Locales que usa la API de OpenData.
> El problema es que es tedioso generar los filtros para
> limitar el ámbito geografico de los GL es tedioso.
> Además la lista de municipios puede variar con la creación o desaparición de GL.
> Som Energia mantiene la lista de municipios que pertenecen a cada Grupo Local pero no es accesible.
> Nos gustaría que esa lista estuviera publicada mediante una API.
> Ello permitiría configurar el widget indicando el GL en vez de indicar todos los municipios.
> En el futuro se puede usar tambien en el front-end para añadir filtros automaticos

## Enlaces

* Código del proyeco que usará la API
* Si no hay código, idea de la que proviene
* Mockup que emula la funcionalidad de la futur API

> - El widget los estamos implementando en http://github.com/som-labs/widget-meloinvento
> - Parte de la idea http://github.com/som-labs/community/issues/1
> - Esta clase mockea el comportamiento de la futura API http://github.com/som-labs/widget-meloinvento/src/apimockup.php


## Funcionalidad
* Describir un poco más que es lo que habria que implementar

Ejemplo:

> Habria que ofrecer API para:
> 
> - Obtener la lista de grupos locales y sus ids
> - Obtener la lista de municipios incluidos en el ambito geográfico de un grupo local dado

## Propuesta de API
* Que puntos de entrada pondríais, que respuestas os esperariais, que errores habria que gestionar

Ejemplo:

> - /localgroup/list
>   ```yaml
>   data:
>   - name: Baix Montseny
>     id: 1
>   - name: Baix Llobregat
>     id: 2
>   ...
>   ```
> - /localgroup/1/cities
>   ```yaml
>   data:
>   - name: Sant Celoni
>     id: 2434
>   - name: La Batlloria
>     id: 2311
>   ...
>   ```
> - /localgroups/200000/cities
>   ```yaml
>   error: No local group
>   errorcode: badlocalgroup
>   data:
>     id: 200000
>   ```


