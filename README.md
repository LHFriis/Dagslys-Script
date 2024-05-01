# Grasshopper Dagslys script V1
SScriptet er udviklet med henblik på at kunne beregne sDA igennem en revit model. Dog kan scriptet, med viden inden for grasshopper og ladybug tools, omskrives til at lave udtræk direkte fra Rhino. På denne måde, vil man kunne have et dagslys script, der nemt kan bruges i de helt tidlige designfaser også. 

# Dansk Guide

For at dette script skal fungere er det vigtigt både at have Rhino.Inside.Revit [RIR] til at køre, og at scriptet åbnes igennem sit grasshopper vindue i Revit. Hvis man åbner scriptet igennem Rhino -> Grasshopper, vil alle RIR komponenter ikke vises. RIR er et gratis plugin til Revit.
Download link til Rhino.Inside.Revit:
https://www.rhino3d.com/inside/revit/1.0/ 
Udover RIR er det vigtigt at du har en aktiv Autodesk Revit og Rhino Licens. Derudover er der brugt et udvalg af grasshopper plugins: 
-	Ladybug Tools (Dagslyssimulering): https://www.food4rhino.com/en/app/ladybug-tools 
-	Rhino.inside.revit (indhentning af revit geometri til grasshopper)
-	Human (Overføre grasshopper geometri til Rhino lagstruktur): https://www.food4rhino.com/en/app/human  
-	Metahopper (layout): https://www.food4rhino.com/en/app/metahopper
-	Clipper (forskydning af analyseflader): https://www.food4rhino.com/en/app/clipper-grasshopper-and-rhino 

OBS! For at scriptet skal fungere, er det vigtigt (!), at dine vinduer og døre er defineret til hhv. glass, frame / mullion, panel osv. Hvis dette ikke er defineret i modellens specifikke ’Families’s - ’Identity Data’, kan scriptet ikke frasortere glasset (Aperture) i Honeybee modellen, og simuleringen kan derfor ikke definere hvordan solen påvirker bygningen iht. Dagslys. 
![Identity data](https://github.com/LHFriis/Dagslys-Script/assets/166735139/d34ea5f5-4054-4ce1-a831-3a38e33533c8)


![Scrip oversigt](https://github.com/LHFriis/Dagslys-Script/assets/166735139/d2989037-2e9b-4b67-8dda-49121e32acaa)

#### 1. Opsætning af analyse:
Det er i denne del analysemodellen defineres, og analysen startes, igennem 7 trin.

##### 1.1 Indstilling / forbindelse til analyse filer
Her indsættes sti til hhv. mappen der skal indeholde analyseresultatet, EPW-filen (vejr data) og sti til Excel filen indeholdende antal soltimer der skal med i analysen. Alle disse filer ligger i uploadet, og hvis du henter hele mappen, skulle den gerne selv indhente filen, hvis du åbner grasshopper-filen fra den specifikke mappe (Python- komponenten, går automatisk ind og henter filerne fra grasshopper-filens placering).

![1 1](https://github.com/LHFriis/Dagslys-Script/assets/166735139/0a8dbd99-4a83-4e57-b302-6e7b6a41bbe8)

##### 1.2 Udvælgelse af geometri
Her vælges hvilken mængde geometri, på ’Levels’ niveau, der skal medregnes i analysen. Derudover kan du vælge hvilke yderligere oplysninger som skal være med i modellen (disse elementer tænder og slukker for elementerne de tilsluttes, til den samlede Honeybee-model.  

![1 2](https://github.com/LHFriis/Dagslys-Script/assets/166735139/2044ff6d-6dc3-46a2-8c12-5b114355e60a)

##### 1.3 Indstilling af Nord retning
Her indstilles Nordretningen. Ved at skifte på slideren, kan du tjekke om nord vender den rigtige retning på din model. 

![1 3](https://github.com/LHFriis/Dagslys-Script/assets/166735139/8525caa0-852e-434f-89cb-ad2ea867b6da)

##### 1.4 Udvælg eventuelle vægge i glas
Hvis man har modelleret et ’Wall’-element, men som er en glasvæg, kan du her vælge typen, hvor den så vil indgå som et vindue. 

![1 4](https://github.com/LHFriis/Dagslys-Script/assets/166735139/468e894b-faa6-4652-a68f-1437cf6384ba)

##### 1.5 Definer type af simulering
Her kan man vælge imellem hvilken type simulering man ønsker at udføre. Man kan ændre på følgende parametre 

1. DGNB eller BR18 analyse (indflydelse på grid størrelse)
2. Fuld bygningsanalyse eller rum analyse
3. Ved rum analyse kan man vælge mellem to typer analyse: Man kan vælge at skrive navnet på de rum man gerne vil analysere på. Disse rum kan inddeles i rum, der skal analysere i hhv. 500 mm, 850 mm og 100 mm over gulvniveau. Eller der kan vælges udvalgte rum i et udvalgt niveau over gulvniveau. 

![1 5](https://github.com/LHFriis/Dagslys-Script/assets/166735139/31cf18c3-9e0c-408f-8801-9c2ba7f389e5)

##### 1.6 Udvælg LT-værdi på vinduer
Her kan LT-værdi bestemmes ud fra udvalgte vindues ’types’. 

![1 6](https://github.com/LHFriis/Dagslys-Script/assets/166735139/99ae2bba-2859-4b6f-9ee2-d62429215895)

##### 1.7 Påbegynd simulering
Ved at skifte ’Boolean Toggle’ til ’True’ vil analysen køre. Alt afhængelig af størrelsen af modellen og analysefladen kan dette tage længere tid. 

![1 7](https://github.com/LHFriis/Dagslys-Script/assets/166735139/8eeacd94-47ee-4be8-a07c-a43874c3afbf)


#### 2. Analyse af resultat
Du får her et overblik over resultaterne fra modellen. Her kan udvælges for et specifikt rum der analyseres, en oversigt over alle rum, samt et gennemsnit af de 80% højeste sDA-værdier. Denne bruges til DGNB ved en fuld bygnings analyse. 
Her kan også vælges hvilket lux niveau der skal bruges for i skalaen. Ved ændring af denne værdi, ændrer grænseværdien for beregning af sDA samt ’heatmap’et sig også.  

![2](https://github.com/LHFriis/Dagslys-Script/assets/166735139/88054e08-a27e-420a-9085-fa73cc234824)


#### 3. Visning af model
Her kan man tænde / slukke for den udvalgte bygningsgeometri. Der kan tændes / slukkes for følgende grupper: Skala, Analyse flade, Analyse grid, Glas, Ramme/panel, Ydervæg overfalde, Indervæg overflade, Samlet vinduer og døre, Samlet vægge, Gulv samt en samlet fuldbygningsvisning. 

![3](https://github.com/LHFriis/Dagslys-Script/assets/166735139/97a384ae-afa4-4a3c-ae0e-a7a132b8353e)


#### 4. Overfør til Rhino lag
Her kan man ’Bake’ sin indhentede Revit geometri til sin Rhino-fil i de udvalgte lag. Enten kan du oprette lag med samme (!) navn i din Rhino-fil, eller bruge den vedlagte Rhino-fil i dette download. Dette kan gøres, hvis du fortrækker at analysere på din geometri fra Rhino fremfor Revit. Husk at åben Rhino igennem dit Revit vindue, for at få dette til at fungere. 

OBS! Hvis du analyserer på denne måde, er det vigtigt at opdatere forbindelserne til pkt. 6 ’Dannelse af Honeybee model’, da denne er sat om til Revit geometrien.  

#### 5. Overførelse af Revit geometri til grasshopper
Denne del af scriptet udvælger de specifikke kategorier fra Revit og omdanner disse til Grasshopper data. Det er vigtigt at have Rhino.Inside.Revit til at køre, for at denne del fungerer. Det er denne del som gør, at vi har mulighed for at forbinde grasshopper til Revit modellen. 
Der er 3 områder som stikker ud i denne del af scriptet.
1.	Ydersiden af ydervæggen bliver separeret fra resten af geometrien, da denne skal have en anden overfladereflektans end indvendig overflade.
2.	2. Vinduer, døre med glas, curtain walls og udvalgte glasvægge bliver eksploderet og en enkelt overflade af glasruden bliver udvalgt. Derved frasorteres karm areal, så der kun analyseres på glasflade. Derudover bliver glasfladen isoleret til en flade, der giver de mest optimale analyseforhold. 
3.	Ved udvælgelsen af Rooms / analyseflade er den delt ind i en fuld bygningsanalyse, detaljeret analyse og specifikt rum analyse. Derfor opstår der 3 forskellige analyseflade dannelser, der aktiveres ved parametrene i pkt. 1.5

#### 6. Dannelse af Honeybee model
Honeybee modellen der skal bruges til at lave dagslysanalysen bliver dannet, ved at angive overfladereflektanser til hver af bygningsdelsgrupperne. Scriptet er indstillet jf. BR18, men en ’Value list’ med DGNB 2023 overfladereflektanser er oprettet og kan kopieres ved brug. Denne værdi skal sættes i ’_reflect’ input i ’OpaqueMod’ der definerer overfladereflektansen. 

![6](https://github.com/LHFriis/Dagslys-Script/assets/166735139/6d7185f4-3f27-4f0e-9e6e-aaabfea8df2e)


#### 7. Beregning af sDA
I denne del beregnes dagslysmængden i modellen. Her kan indstilles på detaljeringsgraden af analysen, om du skal bruge resultatet fra en tidligere analyse, og rette på skalaen, hvis ud vil have andre farver. 
Derudover er det også her man kan indstille det grafiske layout samt indstilling af overførelsen af analyseresultatet til din Revit model. OBS! Husk af markere ”Generic Models” i ’Value Picker’, eller en anden type geometri du ønsker resultatet komme ud i, i din Revit model. Hvis du ikke ønsker en Revit visning, men nøjes med visning i dit Rhino vindue, slukker du bare for komponenterne ’Material’ og ’G-shape’ 

![7](https://github.com/LHFriis/Dagslys-Script/assets/166735139/9bc58904-6e25-4be6-95fb-edb63129d890)


#### 8. Beregning af analyseresultat
I denne del bliver analyseresultaterne behandlet, og omdannes til den form som er at finde under pkt. 2. 

# English Guide
To make this script work, it's important to have both Rhino.Inside.Revit [RIR] running and to open the script through the Grasshopper window in Revit. If you open the script through Rhino -> Grasshopper, all RIR components will not appear. RIR is a free plugin for Revit.

Download link for Rhino.Inside.Revit: https://www.rhino3d.com/inside/revit/1.0/ 

In addition to RIR, it's important to have an active Autodesk Revit and Rhino license. Furthermore, a selection of Grasshopper plugins has been used:
Ladybug Tools (Daylight Simulation): https://www.food4rhino.com/en/app/ladybug-tools 
Rhino.inside.revit (retrieve Revit geometry into Grasshopper)
Human (Transfer Grasshopper geometry to Rhino layer structure): https://www.food4rhino.com/en/app/human 
Metahopper (layout): https://www.food4rhino.com/en/app/metahopper 
Clipper (offsetting analysis surfaces): https://www.food4rhino.com/en/app/clipper-grasshopper-and-rhino 

NOTE! For the script to work, it's crucial (!) that your windows and doors are defined as glass, frame/mullion, panel, etc. If this is not defined in the model's specific 'Families' - 'Identity Data', the script cannot filter out the glass (Aperture) in the Honeybee model, and the simulation cannot define how the sun affects the building in terms of daylight.

Furthermore you can see the pictures, which belongs to the respective parts under the Danish translation above.

#### 1. Analysis Setup
In this section, the analysis model is defined, and the analysis is initiated through 7 steps.

##### 1.1 Setting up / connecting to analysis files:
Here, paths to the respective folders containing the analysis result, EPW file (weather data), and path to the Excel file containing the number of sunlight hours to be included in the analysis are inserted. All these files are located in the uploaded folder, and if you download the entire folder, it should automatically fetch the file if you open the Grasshopper file from that specific folder (the Python component automatically retrieves the files from the Grasshopper file's location).

##### 1.2 Selection of geometry:
Here, the amount of geometry at the Revit 'Levels' to be included in the analysis is chosen. Additionally, you can select which additional information should be included in the model (these elements toggle on and off for the elements they are connected to, for the overall Honeybee model). I would recommend to select all levels. 

##### 1.3 Setting the North direction:
Here, the North direction is set. By adjusting the slider, you can check if the North direction aligns correctly with your model.

##### 1.4 Selecting any glass walls:
If a Revit 'Wall' element has been modeled but is a glass wall, you can select the type here, where it will then be included as a window.

##### 1.5 Define type of simulation:
Here, you can choose between different types of simulations. You can modify the following parameters:
1. DGNB or BR18 analysis (impact on grid size)
2. Full building analysis or room analysis
3. For room analysis, you can choose between two types: You can choose to specify the names of the rooms you want to analyze. These rooms can be divided into rooms that should be analyzed at 500 mm, 850 mm, and 100 mm above floor level. Alternatively, selected rooms on a selected level above floor level can be chosen.

##### 1.6 Select LT- value for windows:
Here, the LT- value can be determined based on selected Revit window types.

##### 1.7 Start simulation:
By toggling the 'Boolean Toggle' to 'True', the analysis will run. Depending on the size of the model and analysis surface, this may take some time.

#### 2. Analysis of results
Here, an overview of the results from the model is provided. You can select a specific room for analysis, an overview of all rooms, and an average of the 80% highest sDA values. This is used for DGNB in a full building analysis.
Additionally, you can choose which lux level to use for the scale. Changing this value will also change the threshold for calculating sDA and the heatmap.

#### 3. Model visualization
Here, you can toggle on/off the selected building geometry. You can toggle on/off the following groups: Scale, Analysis surface, Analysis grid, Glass, Frame/panel, Exterior wall surface, Interior wall surface, Total windows and doors, Total walls, Floor, and a total full building view.

#### 4. Bake to Rhino layers
Here, you can 'Bake' your retrieved Revit geometry into your Rhino file in the selected layers. You can either create layers with the same (!) names in your Rhino file or use the attached Rhino file in this download. This can be done if you prefer to analyze your geometry from Rhino rather than Revit. Remember to open Rhino through your Revit window to make this work.
NOTE! If you analyze in this way, it's important to update the connections to step 6 'Formation of Honeybee model', as this is set to Revit geometry.

#### 5. Bake of Revit geometry to Grasshopper
This part of the script selects specific categories from Revit and converts them into Grasshopper data. It's important to have Rhino.Inside.Revit running for this part to work. This part enables you to connect Grasshopper to the Revit model.
There are 3 areas that stand out in this part of the script:
1. The exterior of the exterior wall is separated from the rest of the geometry because it requires a different surface reflectance than the interior surface.
2. Windows, glass doors, curtain walls, and selected glass walls are exploded, and a single surface of the glass pane is selected. This filters out the frame area so that only the glass surface is analyzed. Additionally, the glass surface is isolated to a plane that provides the most optimal analysis conditions.
3. When selecting Rooms/analysis surface, it is divided into a full building analysis, detailed analysis, and specific room analysis. Therefore, 3 different analysis surface formations occur, activated by the parameters in step 1.5.

#### 6. Defining of Honeybee model
The Honeybee model used for daylight analysis is formed by specifying surface reflectances for each of the building component groups. The script is set according to BR18, but a 'Value list' with DGNB 2023 surface reflectances is created and can be copied for use. This value should be set in the '_reflect' input in 'OpaqueMod' which defines the surface reflectance.

#### 7. Calculation of sDA
In this part, the amount of daylight in the model is calculated. Here, you can adjust the level of detail of the analysis, whether you need the result from a previous analysis, and adjust the scale if you want different colors. Additionally, this is where you can adjust the graphical layout and set up the transfer of the analysis result to your Revit model. NOTE! Remember to select "Generic Models" in the 'Value Picker', or another type of geometry you want the result to come out in, in your Revit model. If you don't want a Revit view but only want to view it in your Rhino window, just turn off the 'Material' and 'G-shape' components.

#### 8. Calculation of analysis result
In this part, the analysis results are processed and converted into the form found in step 2.

