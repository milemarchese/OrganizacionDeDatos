### 75.06/95.58 Organización de Datos: Trabajo Práctico 1 
### Análisis Exploratorio
------

### El dataset

El dataset está compuesto por 7613 filas, contiene 5 columnas:
* id
* keyword
* location
* text : Contenido de un tweet sobre un desastre.
* target: Veracidad del tweet, 1 = real, 0 = falso.

No todos los tweets tienen asociado una locación o palabra clave. Respecto a la columna de texto se detecta que hay tweets repetidos con distintos grados de verdad, es decir, el mismo texto es calificado como real y falso. Se decidió eliminar la totalidad de los duplicados ya que no representan una muestra significativa y no hay otro criterio más que el aleatorio para conservar uno de cada grupo de repetidos.

### Cantidad de tweets reales vs. falsos

La distribución reales contra falsos es de un %58 de falsos contra un %42 de reales. 

### Top ciudades con mayor cantidad de tweets reales/falsos

Una vez que se descartaron las locaciones nulas, como primera observación se ve que hay muchas locaciones incoherentes o falsas. Para descartar la mayor cantidad de datos falsos se filtró de la siguiente forma:

1. Todo el texto a lower case, una vez que todo está en minúscula desaparece la diferencia entre, por ejemplo, 'USA' y 'usa'.

2. Se ejecuta una regex que captura solamente aquellas que tienen los caracteres de la 'a' a la 'z', comas y espacios. Se decide ignorar locaciones que no existen compuestas por símbolos, por ejemplo, 'Instagram: trillrebel_'.

3. Una vez agrupado por location se puede ver que no se eliminó por compleo el problema de lugares 'sinónimos', en el gráfico vemos que está: 'new york', 'nyc' y 'new york, ny'. 

![Gráfico](grafico_locaciones_repetidas.png)

Agrupando las locaciones de 3946 pasan a 2271.

4. Utilización de la librería [GeoPy]: Esta librería recibe la columna 'location' del dataframe y el resultado se almacena en la columna 'geodata'. Geopy dada la locación devuelve (principalmete) address y point. Adress es la dirección completa del lugar, por ejemplo, 'City of Melbourne, Victoria, Australia', y point contiene las coordenadas. GeoPy da la opción de utilizar cualquier servicio de geocoding, en este caso se utilizó [Nominatim]. Nominatim es un servicio gratuito y open-source, permite un máximo de un request por segundo y eso hace que sea poco performante en tiempo. Es por eso que se persiste el archivo locations.csv, así no es necesario correr GeoPy cada vez que se levanta el notebook.
Una vez finalizado el filtrado con GeoPy se observa que no se encontraron 374 locaciones, el 94% de ellas con una sola aparición. Podría volver a pasarse el servicio de geocoding pero se decidió descartarlas.

[GeoPy]: https://geopy.readthedocs.io/en/stable/
[Nominatim]: https://nominatim.org/

Una vez filtrado el dataset obtenemos:

![Gráfico](grafico_10_loc_real.png)
![Gráfico](grafico_10_loc_fake.png)

__Conclusiones:__ TODO

### Top paises participantes:

Con el dataset obtenido en el filtrado de locaciones se genera la columna 'country'. En la primera iteración se encuentra que hay paises 'sinónimos', por ejemplo, 'United States' Y 'United States of America', por otro lado es necesario eliminar los espacios que también generan redundancias, como 'Nigeria' y 'Nigeria '. Se filtra de nuevo sobre esta lista de paises con un servicio de geocoding y se eliminan las repeticiones.

Primera iteración:

| country					| total	| real | fake |
| ------------------------- |:-----:| ----:|-----:|
| United States of America 	| 1705	| 700  | 1005 |
| United Kingdom			| 384	| 143  | 241  |
| Canada	   				| 239	| 114  | 125  |
| United States				| 168	| 103  | 65   |     
| Australia					| 106	| 57   | 49   |
| India						| 84	| 54   | 30   |
| Nigeria					| 61	| 43   | 18   |
| Kenya						| 39	| 15   | 24   |
| Philippines				| 39	| 15   | 24   |
| Italia					| 34	| 10   | 24   |
| República Dominicana		| 32	| 21   | 11   |
| South Africa				| 31	| 10   | 21   |
| Indonesia					| 29	| 16   | 13   |
| France					| 25	| 12   | 13   |
| Ireland					| 25    | 9	   | 16   |







