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
