# analisisinmobiliario

# Proyecto creado para el proceso de selección de Datalized

# Objetivo
Este proyecto compreende el análisis de un dataset inmobiliario de Argentina extraido de un CRM y tiene como objetivo responder a las siguientes preguntas: 
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

# Modelo de datos
- El modelo final de datos tiene el tipo snowflake, con las tablas hecho f_interacciones y tablas dimensión d_Calendario, d_clientes, d_tipo_propiedad y d_grupo_edad. Se creó una tabla en blanco para almacenar las medidas DAX.

# KPIs
- Tasa de conversión: clientes compradores / total de clientes.
- Total estimado en abierto: valor total de las propiedades de clientes que aun no han comprado.
- Clientes compradores: Clientes que ya han comprado una propiedad.

# Visualizaciones
- Ditribución de clientes por rango etario, gráfico de columnas.
- Cantidad de interacciones con clientes por canal de contacto, gráfico de barras.
- Distribución del monto en abierto por ciudad de cliente.
- Listado detallado de clientes a priorizar. Como critérios para esta definición, se clasificaron clientes que no han comprado cuales el último contacto fue menor que 180 dias, nivel de setisfación es alto o medio y score es superior a 0.3.

# Medidas DAX

Clientes Compradores = 
CALCULATE(
    DISTINCTCOUNT(d_clientes[id_cliente]),
f_interacciones[Aun no compra]=FALSE()
)

Tasa de Conversión = 
 DIVIDE([Clientes Compradores],DISTINCTCOUNT(f_interacciones[id_cliente]))

Total Estimado en Abierto = CALCULATE(SUM(f_interacciones[valor_estimado_propiedades]),f_interacciones[Aun no compra]=True)

# Insights y Recomendaciones (Respuesta del Objetivo)
- En el periodo de datos analizado 808 clientes compraron propiedades, lo que representa una conversión de 68,82% del total de clientes de la organización.
- En cuanto a rango etário, los clientes con mayor probabilidad de compra comprenden el rango 46-55 años, aunque la distribución se muestre uniformizada. Se recomienda que, además de este rango, la organización priorize la captación de clientes con el rango 36-45 años, rango cuyo es mas común la estabilidad/ independencia financiera.
- Referente al canal de contacto, se recomienda que la organización amplie sus esfueros en interacciones via app y email, principales canales por los cuales ha interactuado con sus clientes. Podrían estructurarse campañas de marketing customizadas, direccionadas al rango de 26-45 años. También se recomienda que mantenga constante las interacciones via web y telefono.
- Las principales ciudades por valor de oportunidad en abierto superan los $4M compreenden San Ferando del Valle de Catamarca, Corrientes, Comodoro Rivadavia, Neuquén, Santiago del Estero, Córdoba y Salta. Estas ciudades totalizan 106 (~29%) de los clientes en potencial. Se recomienda que la inmobiliaria centre sus esfuerzos en convertir los clientes en estas ciudades y también aumentar su base de clientes potenciales.
- Por último, utilizando como base el historico detallado de interacciones por no clientes, se recomienda que se establezca un plan de comunicaciones con los clientes que se han contactado en el último mes, en el cual las comunicaciones son más frecuentes para clientes con interacciones más recientes.
- Cómo proximos pasos, se podría realizar un analísis de marketing para evaluar la efectividad de las interacciones en base al plan de comunicaciones.

