### 75.06/95.58 Organización de Datos: Trabajo Práctico 1 
### Análisis Exploratorio
------

## Introducción

### El dataset

Se obtuvo el dataset de: https://www.kaggle.com/c/nlp-getting-started y es el archivo `train.csv`.

Está compuesto por 7613 filas, contiene 5 columnas:

* id: Identificador único del tweet.
* keyword: Palabra clave sobre el texto del tweet. Puede tener o no.
* location: Ubicación de donde el tweet fue mandado. Puede tener o no.
* text : Contenido de un tweet sobre un desastre.
* target: Veracidad del tweet, 1 = real, 0 = falso.

### Primer acercamiento

Respecto a la columna de texto se detecta que hay tweets repetidos con distintos grados de verdad, es decir, el mismo texto es calificado como real y falso. Se decidió eliminar la totalidad de los duplicados ya que no representan una muestra significativa y no hay otro criterio más que el aleatorio para conservar al menos uno de cada grupo de repetidos.

### Cantidad de tweets reales vs. falsos

La distribución reales contra falsos es de un 58% de falsos contra un 42% de reales. 

![Gráfico](img/grafico_distribucion_set.png)

## Desastres

## Texto

### Cantidad de stopwords segun target

Basándose en el concepto de stopword, definido como palabra vacía (tales como las preposiciones, articulos, pronombres, entre otros), derivado del procesamiento de datos en lenguaje natural (NLP) se procedió a encontrar una relación directa entre la cantidad de stopwords por tweet y su correlacion con el target, partiendo de la hipótesis que a mayor cantidad de stopwords más probabilidad de que el tweet sea falso.
Para la implementación del análisis se utilizó la libreria [gensim] que cuenta con una lista de stopwords y se contaron sus apariciones en los diferentes tweets, para luego hacer un recuento total según target, arrojando los datos presentados en el siguiente gráfico

[gensim]:  https://radimrehurek.com/gensim/parsing-preprocessing.html

![Gráfico](img/stopword_por_target.png)


__Conclusión:__ Podemos observar claramente en el gráfico una tendencia a que los tweets verdaderos contienen menos cantidad de las denominadas stopwords (aproximadamente un 30% menos que los falsos ), confirmando la primera aproximación informada.


## Texto: Links

## Texto: Menciones

### Personas más mencionadas

Se busca encontrar las cuentas de twitter más mencionadas y su relación entre tweets sobre desastres reales y falsos. Se ejecuta una regex sobre los tweets que contienen el caracter "@", esta se encarga de filtrar todas las cuentas mencionadas ignorando espacios ya que el arroba también se utiliza como una abreviación de "at" en inglés. 

![Gráfico](img/mas_mencionados_rf.png)

Personas mencionadas en tweets que solo tienen calificación falsa:

![Perfil Ariana Grande](img/perfil_ariana.png)
![Perfil Justin Bieber](img/perfil_justin.png)
![Perfil Michael Parr](img/perfil_mikeparr.png)

__Conclusiones:__ Si bien no hay una cantidad de menciones suficientes para sacar una conclusión que muestre correlación entre menciones y veracidad del tweet se puede observar una tendencia de tweets con calificación falsa dirigidos a personas relacionadas con el mundo del espectáculo, en este caso Ariana Grande y Justin Bieber son artistas pop juveniles y Michael Parr es un actor. 

## Texto: Hashtags

### Trending topics en las ubicaciones mas recurrentes

Para el siguiente análisis se filtró y aisló, del texto de los tweets, los hashtags allí contenidos. Finalmente agrupando por la máxima ocurrencia de tanto los hashtags como las ubicaciones allí twitteadas.

![Gráfico](img/tt_por_pais.png)

Haciendo una observación al gráfico y  una busqueda paralela de datos se encontró una clara relación entre los hashtags más utilizados y el lugar de donde provienen. 
Es el caso de ukfloods, tendencia en el Reino Unido, haciendo referencia a [inundaciones] producidas en noviembre de 2019 que causaron daños de al menos 150 millones de libras.
El hashtag yazidis, trending topic en Rocky Mountains, hace pensar que la ubicación fue mal interpretada, ya que puede tratarse de un exilio masivo sufrida por esa comunidad en el año 2014, a partir de una [ataque militar estadounidense], hacia una zona montañosa en Iraq.
WXKY, radio localizada en el estado de Kentucky.
La presencia de whitehouse como tendencia en Washington DC nos lleva a concluir que los hashtags que más apariciones presentan tienen una relación directa con su ubicación.

[ataque militar estadounidense]:https://www.washingtonpost.com/world/exodus-from-the-mountain-yazidis-flood-into-iraq-following-us-airstrikes/2014/08/10/f8349f2a-04da-4d60-98ef-85fe66c82002_story.html
[inundaciones]:https://en.wikipedia.org/wiki/2019%E2%80%9320_United_Kingdom_floods



## Ubicaciones

### Top ciudades con mayor cantidad de tweets reales y falsos

Una vez que se descartaron las ubicaciones nulas, como primera observación se ve que hay muchas incoherentes o falsas. Para descartar la mayor cantidad de datos falsos se filtró de la siguiente forma:

1. Se ejecuta una regex que captura solamente aquellas ubicaciones que tienen los caracteres de la 'a' a la 'z', comas y espacios. Se decide ignorar ubicaciones que no existen compuestas por símbolos, por ejemplo, "Instagram: trillrebel_".

2. Todo el texto a lower case, una vez que todo está en minúscula desaparece la diferencia entre, por ejemplo, "USA" y "usa".

3. Una vez agrupado por 'location' se puede ver que no se eliminó por completo el problema de lugares redundantes, en el gráfico vemos que está: "new york", "nyc" y "new york, ny". 

![Gráfico](img/top_20_ubicaciones_rep.png)

4. Utilización de la librería [GeoPy]: Esta librería recibe la columna "location" del dataframe y el resultado se almacena en la columna "geodata". Geopy dada la ubicación devuelve un objeto compuesto por "address" y "point". Adress es la dirección completa del lugar, por ejemplo, "City of Melbourne, Victoria, Australia", y point contiene las coordenadas. GeoPy da la opción de utilizar cualquier servicio de geocoding, en este caso se utilizó [Nominatim]. Nominatim es un servicio gratuito y open-source, permite un máximo de un request por segundo y eso hace que sea poco performante en tiempo. Es por eso que se persiste el archivo locations.csv, y solo es necesario correr GeoPy si no se encuentra el csv.
Una vez finalizado el filtrado con GeoPy se observa que no se encontraron 374 locaciones, el 94% de ellas con una sola aparición.

[GeoPy]: https://geopy.readthedocs.io/en/stable/
[Nominatim]: https://nominatim.org/

Una vez filtrado el dataset obtenemos:

![Gráfico](img/grafico_10_loc_real.png)

![Gráfico](img/grafico_10_loc_fake.png)

__Conclusiones:__ A priori los resultados son pequeños en proporción al tamaño del set. No podemos asegurar que haya una relación entre ubicación del tweet y nivel de veracidad. Se puede observar una tendencia a que ciudades de Estados Unidos compongan los dos top 10 pero esto puede ser explicado por las [estadísticas] de la red social Twitter que muestran que Estados Unidos compone el 64.2% de los usuarios, y así también la ciudad de Nueva York, que se encuentra en ambos top 10, es la ciudad más poblada de Estados Unidos. 

[estadísticas]: https://www.statista.com/statistics/242606/number-of-active-twitter-users-in-selected-countries/

### Ubicaciones no encontradas

__Hipótesis:__ Si bien el sistema de geocoding funcionó exitosamente en una gran parte del set habrá ubicaciones reales que no se encontraron.

Las siguientes son las ubicaciones no encontradas por GeoPy mas frecuentes:

* road to the billionaires club
* edinburgh
* america of founding fathers
* bangalore, india
* buy give me my money
* financial news and views
* five down from the coffeeshop
* washington dc
* eastcarolina
* eic
* el dorado, arkansas
* england, united kingdom
* in the potters hands
* mad as hell
* reddit

__Conclusión:__ Se observan ubicaciones reales que el sistema no encontró como "england, united kingdom" pero también se observan ubicaciones falsas peculiares como "mad as hell".

### Paises en el set:

__Hipótesis:__ Por lo antes mencionado Estados Unidos es el país que más cantidad de tweets tienen en su ubicación.

Con el dataset obtenido en el filtrado de locaciones se genera la columna "country". En la primera iteración se encuentra que hay paises redundantes, por ejemplo, "United States" Y "United States of America", por otro lado es necesario eliminar los espacios que también generan redundancias, como "Nigeria" y "Nigeria ". 

* Primera iteración:

| País						| Cantidad de tweets|
| ------------------------- |:-----:|
| United States of America 	| 1705	|
| United Kingdom			| 384	|
| Canada	   				| 239	|
| United States				| 168	|
| Australia					| 106	|
| India						| 84	|
| Nigeria					| 61	|
| Kenya						| 39	|
| Philippines				| 39	|
| Italia					| 34	|
| República Dominicana		| 32	|
| South Africa				| 31	|
| Indonesia					| 29	|
| France					| 25	|
| Ireland					| 25    |


* Segunda iteración:

Para esta parte del análisis se optó por la herramienta [geopandas] para mostrar los paises del dataset en el mapa. El dataframe que provee geopandas está en inglés y se puede observar que Nominatim devolvió ubicaciones en español, por ejemplo, "Italia". Con la [API] de google translator se tradujeron todas las ubicaciones a inglés.

[geopandas]: https://geopandas.org/mapping.html
[API]: https://pypi.org/project/googletrans/

![Gráfico](img/grafico_mapa_paises.png)


![Gráfico](img/grafico_top_paises.png)

Según grado de veracidad del tweet:

![Gráfico](img/grafico_top_paises_veracidad.png)

__Conclusión:__ La hipótesis fue confirmada, Estados Unidos es el país con mayor cantidad de tweets del set. Por otro lado se puede observar que hay una gran cantidad de paises presentes en el mapa dejando en evidencia la alta penetración que tiene Twitter alrededor del mundo siendo parte del [top 15] de redes sociales mas utilizadas.

[top 15]: https://datareportal.com/reports/digital-2020-global-digital-overview

### Tweets con y sin ubicación o keyword nula

* Los tweets con keyword nula, son solo 56 de un set de más de 7600. No representa una muestra significativa para sacar una conclusión.

* Respecto a los tweets de ubicación nula los reales y los falsos continuan manteniendo la proporción de 58%-42%. Se decide aplicar un filtro más agrupándolos por largo del tweet, como se analizó previamente la totalidad del texto del set para buscar una relación entre cantidad de caracteres y nivel de veracidad según la falta de ubicación. A su vez se agrega la comparación segun cantidad de caracteres al subset de tweets con ubicación real.

![Gráfico](img/tweets_con_sin_ubicacion.png)

__Conclusión:__ Como se observa en el gráfico no se encuentra una relación entre grado de veracidad según cantidad de caracteres y si el tweet tiene una ubicación o no, y que ambos análisis de subsets de tweets tienen un comportamiendo análogo entre sí y con el dataset completo dando un indicio de uniformidad en cuanto a la distribución de tweets reales y falsos del dataset.


### Hashtags mas usados en los tweets
Con lo calculado previamente en trending topics por ubicacion, se decidio tambien analizar cuales eran los mas utilizados sin importar la ubicacion.

![Gráfico](img/top_hashtags.png)

__Conclusión:__ Podemos observar que, por lejos, "#news" es el mas utilizado, sin embargo no sabemos con certeza si es por los tweets reales o falsos, para eso podemos realizar un segundo analisis en donde los separamos y analizamos lo mismo.

![Gráfico](img/top_hashtags_separados.png)

__Conclusión:__ Ahora que obtuvimos los datos separados, podemos concluir que "#news" viene de ambas partes, sin embargo la mayor cantidad, casi 2/3, se encuentran en los reales; mientras que del segundo mas usado al cuarto, la cantidad de veces usado en falsos  es casi identica a la cantidad de veces usado en los verdaderos.
Para una mayor distincion, se grafican, separados por veracidad, los hashtags mas usados por cada uno.

![Gráfico](img/top_hashtags_reales.png)

![Gráfico](img/top_hashtags_falsos.png)

Para estos ultimos graficos se utilizó la herramienta [wordcloud] e [imageio].

[wordcloud]: https://pypi.org/project/wordcloud/
[imageio]: https://pypi.org/project/imageio/

### Palabras mas usadas en los tweets
Para este analisis se decicio separar a los verdaderos y los falsos, y analizar si existia alguna diferencia entre las palabras que mas aparecen.

![Gráfico](img/top_palabras_falsos.png)

![Gráfico](img/top_palabras_reales.png)

__Conclusión:__ Podemos ver que las palabras mas usadas en falsos tienen una mayor diferencia en la cantidad con respecto a las verdaderas, puesto que el descenso en la cantidad de repeticiones en los falsos es mucho mas rapido que en el de su contraparte, esto se puede deber a que en los tweets falsos se suelen utilizar muchas mas palabras,evitando asi concentraciones de unas pocas; mientras que en los reales existe un conjunto de palabras similares que suele repetirse (por ejemplo: "people/kill/police/disaster" / "gente,asesinar,policia,desastre" ).

### Similitudes entre tweets
Se busco analizar el parecido entre los propios tweets (verdaderos y falsos separados) para ver si existia alguna relacion. Para ello utilizamos LHS y fuimos variando que porcentaje de similitud minimo tenian que tener para ser considerados parecidos. Para obtener una mayor cantidad de valores decidimos variar esta similitud desde 1% hasta 99% escalando de a 1%, y en cada paso calcular el promedio general de cuantos parecidos tienen los tweets.

![Gráfico](img/parecidos_tweets.png)

__Conclusión:__ Se aprecia que cuando el porcentaje de similitud esta por debajo del 10% se obtienen muchos tweets similares, con la particularidad de que en los verdaderos el parecido se da mucha menos veces que en los falsos. Vemos que sin embargo, este parecido entre tweets no dura mucho, pues cuando el minimo de similitud ronda el 20% decae abruptamente la cantidad de parecidos; sin embargo la cantidad de parecidos de los falsos supera la cantidad de los verdaderos para igual umbral minimo de similitud.
