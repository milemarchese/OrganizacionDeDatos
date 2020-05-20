### Hashtags mas usados en los tweets

Con lo calculado previamente en trending topics por ubicacion, se decidio tambien analizar cuales eran los mas utilizados sin importar la ubicacion. 

![Gráfico](FALTA IMAGEN TOP hashtags)

Podemos observar que, por lejos, #news es el mas utilizado, sin embargo no sabemos con certeza si es por los tweets reales o falsos, para eso podemos realizar un segundo analisis en donde los separamos.

![Gráfico](FALTA IMAGEN WORDCLOUD VERDADEROS)

![Gráfico](FALTA IMAGEN WORDCLOUD FALSOS)


__Conclusión:__  Ahora que obtuvimos los datos separados, podemos concluir que #news viene de ambas partes, sin embargo la mayor cantidad se encuentran en los reales, mientras que el segundo #nowplaying viene casi en su totalidad por el lado de los falsos.


### Palabras mas usadas en los tweets

Para este analisis se decicio separar a los verdaderos y los falsos.

![Gráfico](FALTA IMAGEN PALABRAS MAS USADAS VERDADEROS )


![Gráfico](FALTA IMAGEN PALABRAS MAS USADAS FALSOS )


__Conclusión:__ Podemos ver que las palabras mas usadas en falsos tienen una mayor diferencia en la cantidad con respecto a las verdaderas, puesto que el descenso en la cantidad de repeticiones es mucho mas rapido que el de su contraparte, esto se puede deber a que en los tweets falsos se suelen utilizar muchas mas palabras, por lo que evita concentraciones de unas pocas; mientras que en los verdaderos existe un conjunto de palabras similares que suele repetirse (people/kill/police/disaster).



### Similitudes entre tweets

Tambien, se busco analizar el parecido entre los propios tweets (verdaderos y falsos separados) para ver si existia alguna relacion. Para ello utilizamos LHS y fuimos variando que porcentaje de similitud minimo tenian que tener para ser considerados parecidos.

![Gráfico](FALTA IMAGEN SCATTER TWEETS PARECIDOS )

__Conclusión:__ Se aprecia que cuando el porcentaje de similutd esta por debajo del 10% se obtienen muchos tweets similares, con la particularidad de que en los verdaderos el parecido se da mucha menos veces que en los falsos.
Vemos que sin embargo, este parecido entre tweets no dura mucho, pues cuando el minimo de similitud ronda el 20% decae abruptamente la cantidad de parecidos; sin embargo la cantidad de parecidos de los falsos supera la cantidad de los verdaderos para igual umbral minimo de similitud.


