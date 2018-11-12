# <p style="text-align: center;font-family: georgia,ligth italic;"><FONT SIZE=7><FONT COLOR=#283747><B>Clasificación de imágenes ópticas Sentinel-2 basada en objetos geográficos utilizando software libre</P></B></font></p>
---

<p style="text-align: justify;"><FONT SIZE=4><FONT COLOR=#283747><img class="RIGHT " ALIGN=RIGHT WIDTH=250 HEIGTH=100 src="https://unalmed.ctzen.co/images/logo_unal.png" >
    <EM>Elaborado por: Fredy Martínez</EM> <P><EM>
Maestría en Geomática Universidad Nacional de Colombia. </P></EM> <P><EM>
<a href=”mailto:famartinezal@unal.edu.co”>Email: famartinezal@unal.edu.co</EM> <P> </a></font></p>
 
 ---

## <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=5><FONT COLOR=#283747><B>Introducción</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>En los últimos años la disponibilidad gratuita de imágenes satelitales a aumentado, a la tradicional constelación Landsat se le han sumado los satélites de la misión Sentinel (1 al 5), que proveen una resolución espacial y temporal alta (para el caso de Sentinel-2 un tiempo de revisita de 14 días y 4 bandas en el espectro visible e infrarojo con un tamaño de píxel de 10 m), esto representa una oportunidad sin precedentes para clasificar la cobertura terrestre con intervalos de tiempo más cortos, además de facilitar la identificación de objetos más pequeños, adicionalmente existe una necesidad de información por parte de las entidades gubernamentales y gremiales, por ejemplo la estimación de áreas cultivadas y los estados fenológicos de las plantas; sin embrago la gran mayoría de los trabajos que se han realizado en Colombia en este sentido han utilizado un enfoque de clasificación basado en el análisis de pixeles individuales, de acuerdo con Jensen, (2015), se necesitan algoritmos que tengan en cuenta no sólo las características espectrales de un solo píxel, sino también las de los píxeles vecinos, que brinden información contextual de las características de los objetos para poder identificar áreas o segmentos de pixeles que son homogéneos (Frohn y Hao, 2006)</font></p>

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Por otro lado la variabilidad espacial y temporal de algunos cultivos como el arroz <I>(Oryza sativa)</I> y particularmente en Colombia dificulta el conocimiento del área cultivada, su distribución y su dinámica. En este país Garcia & Martinez, (2010) realizaron un trabajo previo en la zona de estudio utilizando imágenes Landsat 7 e imágenes ASTER, y consistió identificar y cuantificar áreas en arroz con un enfoque de clasificación basado en píxeles obteniendo una exactitud global del 87.5%, ellos recomiendan el uso de imágenes en diferentes estados fenológicos para caracterizar mejor el comportamiento del cultivo.  Este trabajo pretende evaluar la posibilidad de utilizar la clasificación de imágenes ópticas Sentinel-2 basada en objetos geográficos (por sus siglas en inglés GEOBIA) de manera eficiente con el uso de técnicas de aprendizaje de maquina en la exactitud identificación del estado fenológico de lotes cultivados en arroz <I>(O. sativa)</I>, en los municipios de Saldaña y Purificación departamento del Tolima.</font></p>  

## <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=5><FONT COLOR=#283747><B>Materiales y métodos</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>En la figura 1 se muestra el esquema general de todo el procedimiento que se llevó a cabo y que se explicará de manera más detallada en adelante.</font></p>  


---
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4> <I>Figura 1:</I> </font></p> 

![alt text](https://github.com/famartinezal/Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA/blob/master/pr2.png)

 ---


### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=4><FONT COLOR=#283747><B>Área de estudio:</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>La zona está situada, al sur oriente del departamento del Tolima, desde el municipio de Purificación en el extremo más hacia el sur (3°48'03.48'' N, 75°00'02,40'' O) hasta llegar la desembocadura del río Saldaña en el Magdalena en el municipio de Saldaña (4°00'00.83'' N, 74°52'00,75'' O). El clima predominante es cálido seco con temperaturas entre 28 °C, precipitaciones anuales de 1541 mm. El principal uso de la tierra en esta región es la agricultura, esta zona  comprende las areas pertenecientes al distrito de riego Usosaldaña que tiene un tamaño de unas 14.000 has aproximadamente adecuadas para riego donde el cultivo predominante durante todo el año es el arroz <I>(O. sativa)</I>, sin embargo la zona de estudio corresponde a 23.064 ha ya que se consideran otras coberturas que se allí se encuentran, la figura 2 muestra la ubicación donde se llevó a cabo el presente trabajo. </font></p> 



### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=4><FONT COLOR=#283747><B>Proyección</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Las imágenes satelitales utilizadas y el conjunto de datos den entrenamiento y validación fueron referenciados en un sistema de coordenadas proyectado. El sistema de coordenadas geográficas utilizado es el Sistema Geodésico Mundial 84 (WGS 84) y la proyección seleccionada es Universal Transverse Mercator zona 18N (UTM zona 18N). El identificador de la proyección es EPSG 32618. </font></p> 
    

### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=4><FONT COLOR=#283747><B>Datos</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Para la búsqueda y selección de las imágenes ópticas se utilizó la plataforma Google Earth Engine, se  cargó una capa que delimitaba la zona estudio, se llamó la colección de Sentinel-2, se realizó un filtro por fecha incluyendo los meses de julio y agosto de 2018, se ordenó la colección en orden descendente por la variable 'CLOUDY_PIXEL_PERCENTAGE' con el fin de obtener la menor cobertura de nubes, posteriormente fue creada una variable que contaba el número de escenas que cumplían con las condiciones, de estas se seleccionaron las correspondientes al 18 de marzo de 2018, se puede acceder al código haciendo clic  <A HREF="https://code.earthengine.google.com/2c6c3dfbb38f33317148c7be3e775df4">aquí</A> posteriormente las imágenes fueron descargadas de la página del <A HREF="https://earthexplorer.usgs.gov/">Servicio Geológico de Estados Unidos (USGS)</A> </font></p> 


#### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=3><FONT COLOR=#283747><B>Sentinel 2</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
Las imágenes utilizadas para este trabajo provienen de una constelación de vigilancia terrestre de dos satélites de la misión Sentinel-2 que forma parte de del programa Copérnico de la Unión Europea para la Agencia Espacial Europea (ESA, por sus siglas en inglés) y proporcionan imágenes ópticas de alta resolución espacial (10 m en el espectro visible e infrarrojo cercano) para el monitoreo de la tierra y está diseñado como una constelación de dos satélites: Sentinel-2A y Sentinel-2B que permiten obtener información de cualquier punto sobre la superficie de la tierra, con una frecuencia de 5 días. Sentinel-2A se lanzó el 23 de junio de 2015 y el Sentinel-2B se lanzó el 7 de marzo de 2017. Cada imagen proporcionada por los satélites tiene una cobertura de 290 km cuadrados.
Una vez se encontraron la imágenes fue seleccionada la escena correspondiente al tile NWK y NVK en, Nivel 1C, lo que significa que esos productos son orto-imágenes en proyección UTM/WGS84, con mediciones radiométricas por píxel proporcionadas en la reflectancia en el techo de la atmósfera (TOA) del inglés o Top Of the Atmosphere.</font></p>  
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
El pre-procesamiento de datos de la imagen SLC de Nivel 1 se realizó utilizando los métodos de procesamiento estándar de la SNAP Sentinel Application Platform,  se uso el procesador SEN2COR de la ESA para la corrección atmosférica y del terreno de la imagen Sentinel-2A de Nivel Superior de Atmósfera 1C para obtener una imagen de reflectancia a nivel de superficie terrestre (Nivel-2A)(ESA, 2016) . Se realizó un re-muestreo basado en interpolación bilineal a una resolución espacial de 10 × 10 m2 en las bandas 5, 6, 7, 8A, 11 y 12 para lograr la misma resolución en las bandas 2, 3, 4 y 8 y se  remuestreo a una resolución espacial de 10 × 10 m2, adicionalmente se realizó una mascara para extraer la nubes, este procesamiento completo se esquematiza en la figura 2.</A> </font></p> 

---
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4> <I>Figura 2:</I> </font></p> 

![alt text](https://github.com/famartinezal/Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA/blob/master/zona.png)
<TABLE BORDER  width="100%">

 ---


<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
El algoritmo para la corrección atmosférica que convierte los datos de reflectancia del nivel 1-C (TOA) a nivel 2-A (BOA), se basa en el propuesto en la metodología de corrección atmosférica/topográfica de imagen satelital (ATCOR) de Richter y Schlaepfer (2011). Este estima el “Aerosol Optical Thickness (AOT)” eliminando el vapor de agua contenido y corrigiendo según la superficie del terreno.  El AOT es indicativo de cuán transparente es la atmósfera y se construye mediante la correlación de la reflectancia de las bandas 12 (SWIR), 4 (rojo) y 2 (azul), de acuerdo a lo propuesto por Müller-Wilm, (2016). Complementariamente, corrige la presencia de nubes 2 cirrus en la banda 10.  Finalmente, la reflectancia en superficie (BOA, en inglés) es calculada para las bandas 1 a 12, para un producto de nivel de procesamiento 2-A y se remuestrearon todas la bandas a 10 m para facilitar el proceso y exportadas al formato <I>GeoTIFF</I>.</A> </font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
Posteriormente  se procedió a generar los puntos de entrenamiento y validación en el software QGIS a partir de los datos provenientes de la Encuesta Nacional de Arroz Mecanizado (ENAM) del primer semestre de 2018 con datos de siembra mensual y coordenadas de los lotes muestreados, adicionalmente la información se complementó para adicionar otras coberturas a partir de la observación visular de la composición RGB de la imágen, se tomaron un total de 95 puntos agrupados en 9 clases como se muestra en la tabla 1. </A> </font></p> </A> </font></p> 

<TABLE BORDER  width="100%">
   
<CAPTION ALIGN=top> <p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Tabla 1: Información sobre las clases utilizadas.</font></p> </CAPTION>
	<TR>
		<TH><p style="text-align: center;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>ID</TH>
		<TH><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>CLASES</TH>
        <TH><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3># PUNTOS</TH>
		<TH><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>OBSERVACIONES</TH>
    </TR> 
     <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>1</TD>
         <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Enero</TD>
           <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>11</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Siembras en el mes de enero</TD>
	</TR>
   <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>2</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Febrero</TD>
         <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>13</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Siembras en el mes de febrero</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>3</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Inundado</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>12</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Suelos inundados para la fecha de toma de la imágen</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>4</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Húmedo</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>10</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Suelos húmedos para la fecha de toma de la imágen</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>5</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Seco</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>13</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Suelos secos para la fecha de toma de la imágen</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>7</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Urbano</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>8</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Zonas con algún tipo de construcción</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>8</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Matorrales</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>11</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>No hay pastos en el suelo y son comunes los parches de arbustos con hojas pequeñas</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>9</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Malezas</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>9</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Lotes en estado de reposo pero que tienen algún tipo de vegetación</TD>
	</TR>
             <TR>
         <TD><p style="text-align:center;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>10</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Bosque</TD>
             <TD><p style="text-align:  left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>8</TD>
         <TD><p style="text-align: left;font-family: times"><FONT COLOR=#283747><FONT SIZE=3>Con presencia de árbloles sin importar si son frutales o bosques</TD>
		</TR>
</TABLE>


#### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=3><FONT COLOR=#283747><B>Análisis de Componentes principales (ACP)</P></B></font></p> 


<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
A continuación, en el software R se realizó en Análisis de Componentes Principales (ACP)
El ACP es una técnica de transformación de datos usada en percepción remota cuando existe una alta correlación entre variables en este caso las bandas, que permite resumir en un número más pequeño, fácil de interpretar y de procesar y que  en conjunto trata explican la mayor parte de la variabilidad (Good et al., 2012). El componente principal número uno representa la proporción máxima de la varianza del stack de las bandas y los componentes siguientes ortogonales a este van representando una proporción de la varianza restante (Zhao y Maclean, 2000; Viscarra Rossel y Chen, 2011), el código utilizado, los resultados obtenidos y la discusión de los mismos se encuentran como un archivo notebook llamado ACP.ipynb en este repositorio sin embargo el siguiente <A HREF="https://github.com/famartinezal/Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA/blob/master/ACP.ipynb">enlace al código ACP</A> también permite visualizarlo.   
</A> </font></p> 

#### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=3><FONT COLOR=#283747><B>Segmentación</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
Para la creación de los segmentos se utilizó el algoritmo de región de crecimiento en SAGA primero se definen unos puntos semilla que se comparan con los vecinos en base a un umbral que el usuario puede definir, de no ser así el software las define automáticamente. Según Bechtel, Ringeler, & Böhner, (2008) el proceso de crecimiento de la región comienza con las semillas como conjunto inicial de los clúster y se construye una lista inicial de pixeles de borde que contienen 4 vecinos y las regiones se cultivan entonces desde estos puntos de siembra hasta puntos adyacentes dependiendo del criterio de membresía de la región, este criterio se aplica mediante la distancia euclidiana entre el valor de la semilla y el rango definido por el usuario, se puede utilizar el histograma para definir los rangos.    
</A> </font></p> 


<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
    Para realizar el procedimiento se utilizó la versión de SAGA 6.4 siguiendo la siguiente ruta:</A> </font></p> 
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Geoprocessing > Imagery > Segmentation > Object Based Image Segmentation</A> </font></p> 
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Los parámetros usados despúes de realizar varias pruebas fueron para el ancho de banda para la generación de semillas fue de 2.5 y la generalización 1, un ejemplo del resultado se puede observar en la figura 3

</A> </font></p> 



---
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4> <I>Figura 3:</I> </font></p> 

![alt text](https://github.com/famartinezal/Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA/blob/master/segmentos.png)

 ---


<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
Una vez calculada la capa de segmentos se guarda dando clic derecho sobre la misma para continuar con el proceso. 
  Posteriormente en el software QGIS versión 3.4.1 se desplegó la capa resultante y se calcularon los atributos de la mediana, la media, desviación estándar, moda y varianza a partir de la imagen resultante del ACP para cada uno de los segmentos, el procedimiento es el siguiente: </A> </font></p> 
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Dentro de QGIS Procesos > Ráster > Estadísticas de zona</A> </font></p> 
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Una vez allí en la casilla que dice “capa ráster” se selecciona la capa generada en el análisis ACP, donde dice “banda ráster” se van seleccionando una a una las bandas de la imagen ACP, donde dice capa vectorial que contiene las zonas se coloca la capa con los segmentos generada en SAGA, donde dice prefijo de la columna de salida se colocó cp1_, cp2_, cp3_ y cp4_ según se iba cambiando la banda, es importante resaltar que en realizar este procedimiento para la totalidad de la zona se emplearon aproximadamente 3 minutos.

</A> </font></p> 


<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>
A partir de los puntos de entrenamiento y validación se generó una capa buffer circular de 50 metros: </A> </font></p> 
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>QGIS Procesos > Geometría vectorial > Buffer</A> </font></p> 
<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>Distancia 50 metros, Segmentos 25 

</A> </font></p> 

