--------------------
Kiehn_foreninger
--------------------

Et modul, som indeholder nødvendige datasource og presentation for at man kan integrere med Kiehn for "Info" og "Hvad gælder for" for foreninger

--------------------
INSTALLATION
--------------------

1:    Installér modulet

1.a:  kopiér modulet kiehn_foreninger til [cbinfo.config.dir]/modules/septima.

1.b:  Skriv følgende i [cbinfo.modules]:

<module name="kiehn_foreninger" dir="septima/kiehn_foreninger"/>.

2:    Konfigurér modulet

2.a:  Opret en ODBC Kilde i System DSN med navnet "kiehn" som sql server-datakilde og med informationer fra Kiehn om database.

     Skriv følgende i cbinfo.xml
    <!-- =================================== -->
    <!-- Kiehn foreninger                    -->
    <!-- =================================== -->   
    <param name="module.kiehn_foreninger.connect">kiehn</param>
    <param name="module.kiehn_foreninger.user">xxxxx</param>
    <param name="module.kiehn_foreninger.pwd">xxxxx</param>

3:    Eksempel på anvendelse
      
	  Eksisterende targetset ønskes udvidet med information fra Kiehn
	  
3.a:  Datasources

	Eksisterende datasource med geometri og ganske få informationer på grundejerforeninger:
		<datasource endpoint="ep_xxx" name="ds_grundejerf">
			<table geometrycolumn="wkb_geometry"
				name="grundejerf" pkcolumn="ogc_fid"/>
		</datasource>
	
	Der oprettes ny datasource, som joiner lokale grundejerforeninger med Kiehn
	OBS: den gamle bevaredes aht til brug i tema
    <datasource endpoint="ep_xxx" name="ds_grundejerf_join_kiehn">
        <table geometrycolumn="wkb_geometry"
            name="grundejerf" pkcolumn="ogc_fid"/>
		<join datasource="ds_kiehn" command="read-forening">
		   <key name="INDEX_ID">index_id</key>
		</join> 
	</datasource>

3.b:  Targetset
		
		Det eksisterende target udskiftedes med nyt som anvender den nye datasource, samt den presentation, der distribueres i modulet
	
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
		

