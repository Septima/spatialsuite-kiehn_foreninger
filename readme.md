spatialsuite-kiehn-foreninger
=======================


# Licens

Dette modul er udgives under MIT licensen

Første version er finansierret af Ishøj Kommune

The MIT License (MIT)

Copyright (c) 2013 Septima

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


#Beskrivelse:

Et modul, som indeholder nødvendige datasource og presentation for at man kan integrere med Kiehn for "Info" og "Hvad gælder for" for foreninger

Ishøj Kommune har finasieret 1 udgave af dette modul

#Installation

1:    Installér modulet

1.a:  kopiér modulet kiehn_foreninger til [cbinfo.config.dir]/modules/septima.

1.b:  Skriv følgende i [cbinfo.modules]:
```xml
    <module name="kiehn_foreninger" dir="septima/kiehn_foreninger"/>
```
2:    Konfigurér modulet

2.a:  Opret en ODBC Kilde i System DSN med navnet "kiehn" som sql server-datakilde og med informationer fra Kiehn om database.

Skriv følgende i cbinfo.xml:
```xml
    <!-- =================================== -->
    <!-- Kiehn foreninger                    -->
    <!-- =================================== -->   
    <param name="module.kiehn_foreninger.connect">kiehn</param>
    <param name="module.kiehn_foreninger.user">xxxxx</param>
    <param name="module.kiehn_foreninger.pwd">xxxxx</param>
```
3:    Eksempel på anvendelse
      
   Eksisterende targetset ønskes udvidet med information fra Kiehn
	  
3.a:  Datasources

	Eksisterende datasource med geometri og ganske få informationer på grundejerforeninger:
```xml
		<datasource endpoint="ep_xxx" name="ds_grundejerf">
			<table geometrycolumn="wkb_geometry"
				name="grundejerf" pkcolumn="ogc_fid"/>
		</datasource>
```
	Der oprettes ny datasource, som joiner lokale grundejerforeninger med Kiehn
	OBS: den gamle bevaredes aht til brug i tema
```xml
    <datasource endpoint="ep_xxx" name="ds_grundejerf_join_kiehn">
        <table geometrycolumn="wkb_geometry"
            name="grundejerf" pkcolumn="ogc_fid"/>
		<join datasource="ds_kiehn" command="read-forening">
		   <key name="INDEX_ID">index_id</key>
		</join> 
	</datasource>
```
3.b:  Targetset
		
		Det eksisterende target udskiftedes med nyt som anvender den nye datasource, samt den presentation, der distribueres i modulet
```xml
		<!-- ***** grundejerforeninger *** -->
		<!-- udkommenteret af kpc (Septima). Ertstattet med integration med Kiehn
		<target themecondition="theme-grundejerf" displayname="Grundejerforeninger" maxresult="100" name="grundejerf" presentation="custom/pres-grundejerf">
			<datasource name="ds_grundejerf" />
		</target>
		 -->
        
		<!-- ***** grundejerforeninger *** -->
		<target themecondition="theme-grundejerf" displayname="Grundejerforeninger" maxresult="100" name="grundejerf_kiehn" presentation="[module:kiehn_foreninger.dir]/presentations/pres-grundejerf_join_kiehn">
			<datasource name="ds_grundejerf_join_kiehn" />
		</target>
		
```
