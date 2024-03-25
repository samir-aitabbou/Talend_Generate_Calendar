
1/ Allez dans le dossier : jGenerateCalendar

2/ Lancez l'invite de commmande

4/ Depuis l'invite de commande, il faut se positionner sur le répertoire : jGenerateCalendar
   
5/ Lancez le script jGenerateCalendar_run.bat (Pour un poste Windows). Sur Linux, il faut lancer le script jGenerateCalendar_run.sh
   Après avoir exécuté cela, vous verrez le fichier Calendrier.xlsx
   
6/ Vous pouvez également modifier la date début et fin de génération du calendrier 
   Pour cela, il faut aller dans le répertoire  \contexts
   Ensuite vous pouvez ouvrir le fichier Default.properties et modifier les dates.
   
    Contenu actuel du fichier : 
    #this is context properties
	#Sat Jan 14 20:08:23 CET 2023
	dtDeb=2023-01-01 00\:00\:00
	dtFin=2023-12-31 00\:00\:00
	prFolder=C\:/path_to_folder_where_you_want_store_your_generated_Calendar.xlsx/
	nameFile=Calendrier.xlsx
	dtDebExecution=2024-03-24 18\:59\:20
	nbJours=0



 ********** Liste des fonctions utilisées ************

** tJava_1 ** (Initialiser variable)
context.nbJours = (int)TalendDate.diffDate(context.dtFin, context.dtDeb, "dd")+1;
context.dtDebExecution = TalendDate.getCurrentDate();
Début de la génération du calendrier = TalendDate.formatDate("dd-MM-yyyy HH:mm:ss",context.dtDebExecution)


tJava_2 (Fin génération)
Fin de la génération du calendrier = TalendDate.formatDate("dd-MM-yyyy HH:mm:ss",TalendDate.getCurrentDate())

Temps d'exécution                  = TalendDate.diffDate(TalendDate.getCurrentDate(), context.dtDebExecution, "mm")+ "min "
									 + TalendDate.diffDate(TalendDate.getCurrentDate(), context.dtDebExecution, "ss")+ "s"


Les colonnes utilisées dans les tMap

DT_JOUR  	  		  = TalendDate.addDate(context.dtDeb, nombreDeLigne.NUM_LIGNE, "dd") 

ID_JOUR  	  		  = Integer.parseInt(TalendDate.formatDate("yyyyMMdd", detailDate.DT_JOUR)) 

NB_ANNEE 	  		  = TalendDate.getPartOfDate("YEAR", detailDate.DT_JOUR) 

NB_SEMESTRE   		  = TalendDate.getPartOfDate("MONTH", detailDate.DT_JOUR)<6?1:2 

NB_TRIMESTRE  		  = TalendDate.getPartOfDate("MONTH", detailDate.DT_JOUR)<3?1:
						TalendDate.getPartOfDate("MONTH", detailDate.DT_JOUR)<6?2:
						TalendDate.getPartOfDate("MONTH", detailDate.DT_JOUR)<9?3:4 
			   
NB_MOIS		          = TalendDate.getPartOfDate("MONTH", detailDate.DT_JOUR)+1 

NB_JOUR_ANNEE         = TalendDate.getPartOfDate("DAY_OF_YEAR", detailDate.DT_JOUR) 	   

NB_JOUR_MOIS  		  = TalendDate.getPartOfDate("DAY_OF_MONTH", detailDate.DT_JOUR) 

NB_JOUR_SEMAINE 	  = TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)-1==0?7: 
						TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)-1 
				  
NB_SEMAINE_ANNEE 	  = TalendDate.getPartOfDate("WEEK_OF_YEAR", detailDate.DT_JOUR) 

LB_MOIS 			  = TalendDate.formatDate("MMMM", detailDate.DT_JOUR).substring(0, 1).toUpperCase() + 
						TalendDate.formatDate("MMMM", detailDate.DT_JOUR).substring(1, TalendDate.formatDate("MMMM", detailDate.DT_JOUR).length()) 
		  
		  
LB_JOUR         	  = TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==2?"Lundi":
						TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==3?"Mardi":
						TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==4?"Mercredi":
						TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==5?"Jeudi":
						TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==6?"Vendredi":
						TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==7?"Samedi":"Dimanche" 	  
			
BL_DERNIER_JOUR_MOIS = 	TalendDate.compareDate(TalendDate.getLastDayOfMonth(detailDate.DT_JOUR), detailDate.DT_JOUR,"yyyy-MM-dd")==0?1:0 

BL_BISSEXTILE 		 = ((double)TalendDate.getPartOfDate("YEAR", detailDate.DT_JOUR)/4)==Math.floor(TalendDate.getPartOfDate("YEAR", detailDate.DT_JOUR)/4)?1:0 

BL_WEEK_END 		 = 	TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==7
				        || TalendDate.getPartOfDate("DAY_OF_WEEK", detailDate.DT_JOUR)==1 ?1:0 


BL_JOUR_FERIE = 0
			
			
NB : Pour la colonne ID_JOUR, il est important de mettre le format yyyyMMdd et non YYYYMMDD.
	
