# analisisinmobiliario

# Proyecto creado para el proceso de selección de Datalized

# Objetivo
Este proyecto compreende el análisis de un dataset inmobiliario extraido de un CRM y tiene como objetivo responder a las siguientes preguntas: 
- Cuales clientes tienen mayor probabilidad de compra?
- Como podemos priorizar el contacto para aumentar la conversión?

La fuente utilizada fue un archivo CSV, importado a BigQuery para limpieza preliminar y generación del dataset inicial y posterior ingesta en Power BI a través del conector Google Bigquery

# Carga y Limpieza en Bigquery:
- En Bigquery fue creado un Dataset, y a este fue cargada la fuente de datos inicial.
- Se observó que los datos contenian valores de clientes duplicados, por lo cual se generó una nueva tabla maestra con los clientes únicos. También en esta fase se removieron columnas juzgadas inecesarias para el análisis.

# Tranformación de datos en Power Query:
- Trás la carga de los datos utilizando el conector Google Bigquery y modo Import, se hizo un proceso de normalización de la tabla de hechos nombrada interacciones, duplicándola y quitando ids duplicados para la creación de las tablas dimensionales tipo de propiedad y clientes. Principales pasos aplicados: Filas ordenadas, otras columnas quitadas, columnas con nombre cambiado, tipo cambiado.
- También se cargó un archivo csv de apoyo con los nombres y coodinadas geograficas de la ubicación de los clientes.
- Además, se creó manualmente una tabla con rangos etarios que pudiese ser ordenada por una columna de id para que posteriormente fuese posible representar graficamente en orden lógica una clasificación de clientes por rango etario.


La pregunta que busca responder el panel
Las fuentes de datos utilizadas
El proceso de limpieza y transformación de los datos
