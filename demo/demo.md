# Machine Learning Workshop Demo

## Introduksjon

I denne demoen viser vi hvordan man bygger en modell for å spå priser på ulike bilmodeller
ved hjelp av __Azure Machine Learning Studio__.

Vi skal bruke et ferdig eksempel-datasett for denne demoen.

## Steg-for-steg

### 1. Opprett nytt Machine Learning Workspace
* Logg på [Azure Portal](https://portal.azure.com/) med din Azure-konto.
* Klikk _New_ -> Søk etter _Machine Learning Workspace_ -> _Create_.
* Skriv inn _Worspace name_, velg _ny resource group_, og velg _DevTest service plan_. 
* Vent til tjenesten har blitt opprettet og velg så _Overview_ -> _Additional Links_ -> _Launch Machine Learning Studio_.

### 2. Opprett nytt eksperiment
* Velg _New_ nederst til venstre, velg _Experiment_ -> _Blank Experiment_.

### 3. Dra inn data
* Velg _Saved Datasets_ -> _Samples_ -> _Automibile Price Data_.
* Dra datasettet inn på toppen av eksperimentet.
* _Høyreklikk_ -> _Dataset_ -> _Visualize_ for å se på hvilke data som finnes i datasettet.

### 4. Velg ut de mest relevante kolonnene
* Vi ønsker å se på hvilke kolonner (_Features_) som er mest relevante for å bestemme prisen på en biltype.
* For å få til dette benytter vi modulen _Feature Selection_ -> _Filter Based Feature Selection_.
* Dra inn og koble sammen med data. Man ser et rødt utropstegn dukke opp. Dette betyr at man må gjøre et valg.
* Velg target column: _Price_. Det er prisen vi ønsker å finne ut av.
* Lagre og kjør eksperimentet (_Run_).
* Høyreklikk på _Filter Based Feature Selection_ og velg _Features_->_Visualize_ for å se hvilke kolonner som er mest relevante.
* Velg antall kolonner som skal benyttes videre. I dette tilfellet er 9-10 features som er mest relevante for å bestemme prisen.
* Velg så derfor 10 i _Number of Desired Features_ som skal brukes videre i modellen.

### 5. Split data i hva som skal brukes til trening og hva som skal brukes til evaluering i etterkant av modellen
* Vi ønsker å bruke en del av dataen til å trene modellen og en del av dataen til å evaluere i etterkant.
* Dra inn modulen _Split Data_. Koble til _Filtered Dataset_.
* Velg antall % som skal inn til splittet _Dataset1_ og _Dataset2_ og velg _Randomized Split_.
* Dra inn modulene _Train Model_ og _Score Model_. 
* Lag kobling mellom _Dataset1_ fra _Split Data_ til _Train Model_ og fra _Dataset2_ i _Split Data_ til _Score Model_.
* Lag en kobling mellom output fra _Train Model_ til _Score Model_.
* I _Train Model_ velg _Score_ som kolonne.

### 6. Velg algoritme som skal brukes for å trene modellen
* For å velge algoritme bruker vi [__Machine Learning Algorithm Cheat Sheet__](https://github.com/ingljo/machine-learning-workshop/blob/master/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)
* Siden det er en numerisk verdi for pris vi skal prøve å forutsi, er det grenen _Regression_ vi må utforske.
* Her må vi bare prøve oss frem på de ulike algoritmene for å se hvilken som gjør det best.
* Vi prøver først _Lineær regresjon_, som er en rimelig enkel statistisk måte for å beregne en slik type modell.
* Dra inn modulen _Linear Regression_ og koble til _Train Data_ modulen.

### 7. Evaluer modellen
* For å kunne si hvor god modellen vår er må vi evaluere resultatet.
* Dra inn modulen _Evaluate Model_ og koble til _Score Model_ modulen.
* Lagre og kjør modellen.
* Høyreklikk på _Evaluate Model_ og velg _Evaluation Results_ -> _Visualize_.
* Vi kan nå se fra __Coefficient of Determination__ hvor god vår modell er til å forutsi prisen.

### 8. Lag en Webservice for å kunne kalle modellen
* For å kunne bruke modellen må vi opprette en webservice.
* Velg _Set Up Webservice_ -> _Predictive Web Service_. Vi får nå et nytt _Predictive Experiment_ opprettet.
* Velg _Deploy Webservice_ og test den ved å bruke _Request/Response test-page_.
* Som vi kan se fra input så har også urelevante kolonner og price kommet med her. Den ønsker vi ikke med som input.
* For å fjerne urelevant input drar vi inn modulen _Select Columns in Dataset_ inn i vårt _Predictive Experiment_.
* Koble til mellom _Filter Based Feature Selection_ og _Score Model_.
* Velg _All Columns_, exclude _price_.
* Lagre, kjør og _Deploy Webservice_ på nytt. Gå til test-siden, og se at input nå er mer relevant.
* I output ønsker vi heller ikke noe annet enn _Scored Labels_, så vi kan fjerne alle andre output
ved å legge til en _Select Columns in Dataset_ modul mellom _Score Model_ og _Web Service Output_.

### Resultat Training Experiment
![Bilde av Training Experiment](https://github.com/ingljo/machine-learning-workshop/blob/master/demo/TrainingExperiment.JPG)

### Resultat Predictive Experiment
![Bilde av Predictive Experiment](https://github.com/ingljo/machine-learning-workshop/blob/master/demo/PredictiveExperiment.JPG)

### Testing av Webservice
![Bilde av Webservice Test](https://github.com/ingljo/machine-learning-workshop/blob/master/demo/TestWebservice.JPG)
