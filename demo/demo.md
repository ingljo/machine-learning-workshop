# Machine Learning Workshop Demo

## Introduksjon

I denne demoen viser vi hvordan man bygger en modell for å spå priser på ulike bilmodeller
ved hjelp av Azure Machine Learning Studio.

Vi skal bruke et ferdig eksempel-datasett for denne demoen.

## Steg-for-steg

### 1. Opprett nytt Machine Learning Workspace
* Logg på Azure Portal [https://portal.azure.com/] med din Azure-konto.
* Klikk "New" -> Søk etter Machine Learning Workspace -> "Create".
* Skriv inn Worspace name, velg ny resource group, og velg DevTest service plan. 
* Vent til tjenesten har blitt opprettet og velg så Overview -> Additional Links -> Launch Machine Learning Studio.

### 2. Opprett nytt eksperiment
* Velg New nederst til venstre, velg Experiment -> Blank Experiment.

### 3. Dra inn data
* Velg Saved Datasets -> Samples -> Automibile Price Data.
* Dra datasettet inn på toppen av eksperimentet.
* Høyreklikk -> Dataset -> Visualize for å se på hvilke data som finnes i datasettet.

### 4. Velg ut de mest relevante kolonnene
* Vi ønsker å se på hvilke kolonner (features) som er mest relevante for å bestemme prisen på en biltype.
* For å få til dette benytter vi modulen Feature Selection -> Filter Based Feature Selection.
* Dra inn og koble sammen med data. Man ser et rødt utropstegn dukke opp. Dette betyr at man må gjøre et valg.
* Velg target column: Price. Det er prisen vi ønsker å finne ut av.
* Lagre og kjør eksperimentet (Run).
* Høyreklikk på Filter Based Feature Selection og velg Features->Visualize for å se hvilke kolonner som er mest relevante.
* Velg antall kolonner som skal benyttes videre. I dette tilfellet er 9-10 features som er mest relevante for å bestemme prisen.
* Velg så derfor 10 i Number of Desired Features som skal brukes videre i modellen.

### 5. Split data i hva som skal brukes til trening og hva som skal brukes til evaluering i etterkant av modellen
* Vi ønsker å bruke en del av dataen til å trene modellen og en del av dataen til å evaluere i etterkant.
* Dra inn modulen Split Data. Koble til Filtered Dataset.
* Velg antall % som skal inn til splittet Dataset1 og Dataset2 og velg Randomized Split.
* Dra inn modulene Train Model og Score Model. 
* Lag kobling mellom Dataset1 fra Split Data til Train Model og fra Dataset2 i Split Data til Score Model.
* Lag en kobling mellom output fra Train Model til Score Model.
* I Train Model velg Score som kolonne.

### 6. Velg algoritme som skal brukes for å trene modellen
* For å velge algoritme bruker vi Machine Learning Algorithm Cheat Sheet [microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf]
* Siden det er en numerisk verdi for pris vi skal prøve å forutsi, er det grenen Regression vi må utforske.
* Her må vi bare prøve oss frem på de ulike algoritmene for å se hvilken som gjør det best.
* Vi prøver først Lineær regresjon, som er en rimelig enkel statistisk måte for å beregne en slik type modell.
* Dra inn modulen Linear Regression og koble til Train Data modulen.

### 7. Evaluer modellen
* For å kunne si hvor god modellen vår er må vi evaluere resultatet.
* Dra inn modulen Evaluate Model og koble til Score Model modulen.
* Lagre og kjør modellen.
* Høyreklikk på Evaluate Model og velg Evaluation Results -> Visualize.
* Vi kan nå se fra Coefficient of Determination hvor god vår modell er til å forutsi prisen.

### 8. Lag en Webservice for å kunne kalle modellen
* For å kunne bruke modellen må vi opprette en webservice.
* Velg Set Up Webservice -> Predictive Web Service. Vi får nå et nytt Predictive Experiment opprettet.
* Velg Deploy Webservice og test den ved å bruke Request/Response test-page.
* Som vi kan se fra input så har også urelevante kolonner og price kommet med her. Den ønsker vi ikke med som input.
* For å fjerne urelevant input drar vi inn modulen Select Columns in Dataset inn i vårt Predictive Experiment.
* Koble til mellom Filter Based Feature Selection og Score Model.
* Velg All Columns, exclude price.
* Lagre, kjør og Deploy Webservice på nytt. Gå til test-siden, og se at input nå er mer relevant.
* I output ønsker vi heller ikke noe annet enn Scored Labels, så vi kan fjerne alle andre output
ved å legge til en Select Columns in Dataset modul mellom Score Model og Web Service Output.