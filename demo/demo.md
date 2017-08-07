# Machine Learning Workshop Demo

## Introduksjon

I denne demoen viser vi hvordan man bygger en modell for � sp� priser p� ulike bilmodeller
ved hjelp av Azure Machine Learning Studio.

Vi skal bruke et ferdig eksempel-datasett for denne demoen.

## Steg-for-steg

### 1. Opprett nytt Machine Learning Workspace
* Logg p� Azure Portal [https://portal.azure.com/] med din Azure-konto.
* Klikk "New" -> S�k etter Machine Learning Workspace -> "Create".
* Skriv inn Worspace name, velg ny resource group, og velg DevTest service plan. 
* Vent til tjenesten har blitt opprettet og velg s� Overview -> Additional Links -> Launch Machine Learning Studio.

### 2. Opprett nytt eksperiment
* Velg New nederst til venstre, velg Experiment -> Blank Experiment.

### 3. Dra inn data
* Velg Saved Datasets -> Samples -> Automibile Price Data.
* Dra datasettet inn p� toppen av eksperimentet.
* H�yreklikk -> Dataset -> Visualize for � se p� hvilke data som finnes i datasettet.

### 4. Velg ut de mest relevante kolonnene
* Vi �nsker � se p� hvilke kolonner (features) som er mest relevante for � bestemme prisen p� en biltype.
* For � f� til dette benytter vi modulen Feature Selection -> Filter Based Feature Selection.
* Dra inn og koble sammen med data. Man ser et r�dt utropstegn dukke opp. Dette betyr at man m� gj�re et valg.
* Velg target column: Price. Det er prisen vi �nsker � finne ut av.
* Lagre og kj�r eksperimentet (Run).
* H�yreklikk p� Filter Based Feature Selection og velg Features->Visualize for � se hvilke kolonner som er mest relevante.
* Velg antall kolonner som skal benyttes videre. I dette tilfellet er 9-10 features som er mest relevante for � bestemme prisen.
* Velg s� derfor 10 i Number of Desired Features som skal brukes videre i modellen.

### 5. Split data i hva som skal brukes til trening og hva som skal brukes til evaluering i etterkant av modellen
* Vi �nsker � bruke en del av dataen til � trene modellen og en del av dataen til � evaluere i etterkant.
* Dra inn modulen Split Data. Koble til Filtered Dataset.
* Velg antall % som skal inn til splittet Dataset1 og Dataset2 og velg Randomized Split.
* Dra inn modulene Train Model og Score Model. 
* Lag kobling mellom Dataset1 fra Split Data til Train Model og fra Dataset2 i Split Data til Score Model.
* Lag en kobling mellom output fra Train Model til Score Model.
* I Train Model velg Score som kolonne.

### 6. Velg algoritme som skal brukes for � trene modellen
* For � velge algoritme bruker vi Machine Learning Algorithm Cheat Sheet [microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf]
* Siden det er en numerisk verdi for pris vi skal pr�ve � forutsi, er det grenen Regression vi m� utforske.
* Her m� vi bare pr�ve oss frem p� de ulike algoritmene for � se hvilken som gj�r det best.
* Vi pr�ver f�rst Line�r regresjon, som er en rimelig enkel statistisk m�te for � beregne en slik type modell.
* Dra inn modulen Linear Regression og koble til Train Data modulen.

### 7. Evaluer modellen
* For � kunne si hvor god modellen v�r er m� vi evaluere resultatet.
* Dra inn modulen Evaluate Model og koble til Score Model modulen.
* Lagre og kj�r modellen.
* H�yreklikk p� Evaluate Model og velg Evaluation Results -> Visualize.
* Vi kan n� se fra Coefficient of Determination hvor god v�r modell er til � forutsi prisen.

### 8. Lag en Webservice for � kunne kalle modellen
* For � kunne bruke modellen m� vi opprette en webservice.
* Velg Set Up Webservice -> Predictive Web Service. Vi f�r n� et nytt Predictive Experiment opprettet.
* Velg Deploy Webservice og test den ved � bruke Request/Response test-page.
* Som vi kan se fra input s� har ogs� urelevante kolonner og price kommet med her. Den �nsker vi ikke med som input.
* For � fjerne urelevant input drar vi inn modulen Select Columns in Dataset inn i v�rt Predictive Experiment.
* Koble til mellom Filter Based Feature Selection og Score Model.
* Velg All Columns, exclude price.
* Lagre, kj�r og Deploy Webservice p� nytt. G� til test-siden, og se at input n� er mer relevant.
* I output �nsker vi heller ikke noe annet enn Scored Labels, s� vi kan fjerne alle andre output
ved � legge til en Select Columns in Dataset modul mellom Score Model og Web Service Output.