# Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA

---

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


![alt text](https://github.com/famartinezal/Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA/blob/master/pr2.png)


### <p style="text-align: left;font-family: georgia,ligth italic;"><FONT SIZE=4><FONT COLOR=#283747><B>Área de estudio:</P></B></font></p> 

<p style="text-align: justify;font-family: times"><FONT COLOR=#283747><FONT SIZE=4>La zona está situada, al sur oriente del departamento del Tolima, desde el municipio de Purificación en el extremo más hacia el sur (3°48'03.48'' N, 75°00'02,40'' O) hasta llegar la desembocadura del río Saldaña en el Magdalena en el municipio de Saldaña (4°00'00.83'' N, 74°52'00,75'' O). El clima predominante es cálido seco con temperaturas entre 28 °C, precipitaciones anuales de 1541 mm. El principal uso de la tierra en esta región es la agricultura, esta zona  comprende las areas pertenecientes al distrito de riego Usosaldaña que tiene un tamaño de unas 14.000 has aproximadamente adecuadas para riego donde el cultivo predominante durante todo el año es el arroz <I>(O. sativa)</I>, sin embargo la zona de estudio corresponde a 23.064 ha ya que se consideran otras coberturas que se allí se encuentran, la figura 1 muestra la ubicación donde se llevó a cabo el presente trabajo. </font></p> 



![alt text](https://github.com/famartinezal/Geobia-Sentinel-2-Analisis-Componentes-Principales-R-QGIS-SAGA/blob/master/zona.png)
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
