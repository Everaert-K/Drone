# Inventory Management System using Autonomous Drones

## Overview

- Analyse: Diagrams
- Docker: Conains the back-end server and the database 
- Project: Containt the complete application. This application consists out of a front-end webapplication, and a back-end server which communicates with the (future) drone. This backend retrieves data from the drone and stores this data to a database. The webapp is written in the Angular-framework and the server in Express. Both make use of the Node Package Manager. 

### Executing the application locally
#### Installation-manual for local execution

Requirements:
•	Download the source-code (clone the github directory)
•	Install MongoDB (https://www.mongodb.com/download-center/community) and create the database location  Minimum version 4.0
•	Node.js (https://nodejs.org/en/download/) and the Node Package Manager Minimum version 10.15.1
•	Angular, Express (open CMD and go to folder where the project is saved, navigate to /project/backend/management-server, execute command 'npm install', to use a mockup-drone navigate to /project/drone-mockup-node and execute command 'npm install'
Minimum version 4.16.0 for Express and version 7.3.2 for Angular
•	Node-RED. Minimum version 0.20.3
•	Mosquitto broker. Minimum version 1.5.8 (https://mosquitto.org/download/ )
 
#### Executing
-	Start front-end by navigating to /project/web-ui/drone-control-center in a CMD-promt and execute 'ng serve'
-	Start back-end by navigating to /project/backend/management-server and execute 'npm start' 
-	Start mockup drone by navigating to /project/drone-mockup-node and execute 'npm start' 
-	Start database by navigating to the installation folder of MongoDB and continue to  /Server/4.0/bin. Here execute 'mongod' 
-	Start the mosquitto broker by navigating to the mosquitto folder and execute 'mosquitto -v'
-	During first use Node-RED has to be configured see paragraph below.

In Browser http://localhost:4200/dashboard to use app.  
 
 
### Node-RED configuration for first use
After you have executed the webapplication press on the Node-RED button. Load in all the flows via the extra options > Import > Library, here import every flow. de webapplicatie heeft uitgevoerd, moet deze op de Node-RED knop drukken. Vervolgens haalt de gebruiker alle flows op door via de extra opties   naar Import en vervolgens naar Library te gaan en hier elke flow importeert. 

# Rest of the info is of less importance and is only available in Dutch

### Gebruikershandleiding
Vervolgens komt men op het dashboard scherm. Hierbij zal er een map ingeladen worden van de database of, indien deze leeg is, een nieuwe map worden aangemaakt. Via het dashboardscherm ziet men een plattegrond van het magazijn of grootwarenhuis met hierop de scanlocaties aangeduid via een blauwe cirkel en de obstakels via een rode rechthoek. Naast de scanlocaties en de obstakels wordt ook de huidige locatie van de drone aangeduid samen met zijn rotatie. Via de knoppen op de plattegrond kan men scanlocaties en obstakels toevoegen, scanlocaties en obstakels aanpassen of verwijderen, kan men een vliegroute tekenen en kan men het centrum van de map zich laten focussen op de locatie van de drone. 

Onder de map zijn er 4 tabbladen te vinden. Eén voor de drone data: hier kan men de naam, de radius, het batterij niveau, de positie, de snelheid, de acceleratie en de pitch, roll en yaw van de drone bekijken. 
In het tweede tabblad vindt de gebruiker de drone configuratie waar hij de naam en de radius van de drone kan configureren. Deze configuratie wordt nadien opgeslagen in de database. 
Op het derde tabblad kan men de sensoren van de drone aanzetten of uitschakelen. 
Het laatste tabblad bevat vervolgens nog enkele visuele grafieken waar de data van de drone visueel wordt voorgesteld.
Via de knoppen boven de plattegrond kan men de getekende vluchtroute valideren, de drone het pad laten afvliegen en de drone tijdens zijn vluchtroute laten pauzeren, terug verder laten vliegen of de drone volledig laten stoppen met het vliegen van zijn vluchtroute.

Links op het scherm kan men naast het dashboard de andere functionaliteiten van de webapplicatie bekijken. Het inventory scherm bevat een lijst met alle producten die gekoppeld zijn aan de huidige geselecteerde map. Hier kan een gebruiker met admin rechten ook producten aan toevoegen of deze weer verwijderen.
Via het Admin scherm kan een gebruiker met admin rechten kiezen om alle mappen en drones te verwijderen uit de database. 
En als laatste is er ook een Node-RED tabblad voorzien zodat een gebruiker de flows kan bekijken en aanpassen.

In de rechterbovenhoek vindt de gebruiker 3 knoppen waarmee hij een plattegrond kan selecteren, zijn profiel kan bekijken en waarmee hij kan uitloggen uit de webapplicatie.


## Testen

### Unit testen
Om de verschillende API calls voor het toevoegen, aanpassen en verwijderen van de verschillende mappen, producten en droneconfiguraties en het valideren en corrigeren van een vluchtroute is een unit test meegegeven. Met behulp van Mocha en Chai kan iemand gemakkelijk de back-end testen. Deze test kan uitgevoerd worden door via een CMD-venster naar /project/backend/management-server te navigeren en het commando npm test uit te voeren. Hiermee wordt het testbestand /project/backend/management-server/test/testAPI.js uitgevoerd. Voor deze testen moet de database server zijn opgestart.

Bij het ontwikkelen werd vooral getest met het programma Postman waarmee de verschillende endpoints van express kunnen worden aangesproken. Eerst werd er een map opgeslagen, vervolgens kunnen er verschillende GET-operaties worden uitgevoerd op de geëmbedde objecten om te zien om deze wel correct werden opgeslagen, en het express endpoint werkt. Tot slot kan dan worden getest of deze objecten kunnen worden aangepast door PUT en DELETE operaties. Bij PUT moet natuurlijk de body van de http-request wel correct worden ingevuld met de vervangende data.

### Invoercontrole
Elk invoerveld is voorzien van een controle zodat er in velden waar enkel numerieke waardes mogen ingevoerd worden enkel nummers kunnen ingevoerd worden. Ook wanneer er geen waarde ingevoerd wordt, wordt er een correcte foutboodschap op het scherm getoond.

 
### Integration testen (manueel)
#### Testen van de vliegroutes
Om te testen of de vliegroutes correct berekend worden, kan de gebruiker een aantal waypoints aanduiden op de kaart. Onder het Flightpath Options tabblad bij het dashboard kan de gebruiker elke optie testen. Zo kan er nagegaan worden of paden door een obstakel worden afgekeurd indien de Validate path optie aanstaat. Dit pad zou wel moeten goedgekeurd worden indien de “Don’t validate path” optie is geselecteerd of goedgekeurd en aangepast worden indien de optie “Validate and correct path” was geselecteerd. Om de Calculate optimal path optie te testen selecteert de gebruiker het beste verschillende scanlocaties in een volgorde dat zeker niet de beste is, dit is bijvoorbeeld eerst een locatie ver van de drone, gevolgd door een locatie dicht bij de drone en opnieuw een locatie ver van de drone. Bij het klikken van de Validate knop moet de volgorde van de waypoints zijn aangepast. Met de Return to start optie zou het gevalideerde pad dezelfde begin- als eindlocatie moeten hebben.

#### Testen van de Sensor Configuratie
Door onder het Sensor Configuration tabblad bij het dashboard verschillende sensoren uit te schakelen en vervolgens in het Drone Data venster te kijken of de uitgeschakelde sensoren hun data verdwenen is, kan de gebruiker het subscriben op de verschillende MQTT-topics testen. Indien de gebruiker de sensoren terug aanschakelt, zou de data opnieuw moeten verschijnen.

#### Database testen
Op het inventory venster kan de gebruiker controleren welke producten er al reeds in de database zouden moeten zitten. De gebruiker kan hier ook testen of de connectie met de database correct is opgesteld door een product toe te voegen aan deze lijst en vervolgens manueel in de database te kijken of het nieuwe product is toegevoegd. Dit kan bijvoorbeeld gecontroleerd worden door een API call. Meer uitleg kan gevonden worden in de readme over het Inventory Management Server onder drone1/project/backend/README.md

#### Usability testen
Om de webapplicatie op gebruiksvriendelijkheid te testen, heeft een ander team gedurende 5 tot 10 minuten de webapplicatie getest. Hierbij werd duidelijk dat het inladen van de correcte flows in Node-RED niet voor de hand ligt. 
Wanneer de gebruiker een waypoint plaatst dat te dicht bij een obstakel staat toont deze een bericht dat de vliegroute ongeldig is. Voor de gebruiker was dit niet helemaal duidelijk dat het probleem lag aan het feit dat de waypoint te dicht bij de muur lag. De leaflet kaart bevat vele knoppen, maar doordat deze veel informatie tonen indien de gebruiker over deze knoppen bij mouseover veel informatie tonen, is er weinig documentatie nodig over dit onderdeel van de webapplicatie.
Na het dashboard heeft de gebruiker weinig bijkomende informatie nodig omdat het inventory scherm geen ingewikkelde informatie of functies bevat. 

## Meer info

Voor meer info wordt verwezen naar de verschillende readme's in de verschillende mappen van het project. 
* In de map project is er een readme voorzien die kort de verschillende componenten aan haalt.
* Bij project/backend vind men alle info over de backend. Het bevat een overzicht van de verschillende mappen, de API-endpoints van express en info over de vier node-red flows, alsook tekortkomingen en mogelijke uitbreidingen.
* Bij project/web-ui ontbreekt er momenteel nog een readme van de front-end.

Sommige delen de code zijn ook gedocumenteerd.
