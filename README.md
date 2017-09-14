# Machine Learning Workshop

## Introduksjon
I denne workshopen skal vi "hands-on" i bruk av Azure Machine Learning Studio og
bruke reell kundedata for å bygge en machine learning modell.

## Om oss
* Ingvar Ljosland, [ingvar.ljosland@bouvet.no](mailto:ingvar.ljosland@bouvet.no)
* Markus Skallist, [markus.skallist@bouvet.no](mailto:markus.skallist@bouvet.no)
* Kristian Ekle, [kristian.ekle@bouvet.no](mailto:kristian.ekle@bouvet.no)

__Kontakt oss gjerne på Slack__

### Læringsmål
Målene for denne workshopen er å kunne:
1. Lære hvordan Azure Machine Learning Studio fungerer.
2. Importere og manipulere data som input til ML modell.
3. Ta bevisste valg av algoritmer for bruk til å bygge en ML modell.
4. Kunne trene og bygge en ML modell ved bruk av importert data.
5. Å kunne bruke den bygde ML modellen i kode / webservice kall.

## Demo
For å få en liten introduksjon til Azure ML Studio, gjennomfører vi en 
liten demo, slik at vi går gjennom stegene for å bygge en modell i ML Studio.

[Link til demo](https://github.com/ingljo/machine-learning-workshop/blob/master/demo/demo.md).

## Workshop tema - Kan vi spå faren for snøskred?
NVE samler inn data daglig over snøforhold og hvilke snøskred som har gått i området. Dette bruker de
blant annet observatører til. I tillegg bruker de data fra Metrologisk institutt for vær for å kunne
spå hvor stor snøskredfare det er de ulike varslingsregionene.
Du kan til en hver tid se hva faren for snøskred er på http://www.varsom.no/snoskredvarsling/.

For å få til varselet må noen på NVE manuelt se på hvilke observasjoner som er gjort i området kombinert
med hvilket vær det har vært i området den siste tiden.

Er det mulig for oss å kunne spå det samme varselet automatisk ved hjelp av maskinlæring?

### Data
NVE legger ut sine data lisensiert under Norsk lisens for offentlige data [NLOD](https://data.norge.no/nlod/no) og vi har fått
sammenstilt data fra sesongen 2016 til bruk i denne workshopen.

[Her kan du laste ned sammenstilt data for 2016-sesongen til bruk i denne workshopen.](https://github.com/ingljo/machine-learning-workshop/blob/master/data/SeasonDataExtended_All_2016-08-01_2017-06-01.tsv)

#### Beskrivelse av kolonnene
* Date - Dato for innsamlet data
* RegionId - Varslingsregion
* DangerLevel - Varslet faregrad
* ApsTemperature - Gjennomsnittstemperatur siste døgn
* ApsTemperatureMin - Minimumstemperatur siste døgn
* ApsTemperatureMax - Makstemperatur siste døgn
* ApsPercipitation - Nedbør siste døgn
* ApsPercipitationMostExposedArea - Nedbør mest utsatt område siste døgn
* ApsFreezingElevationsTemperature - Grense for frysetemperatur (moh)
* FcObserved - Faceted Crystals – Kantkornet snø
* DhObserved - Depth Hoar – Begerkrystaller
* ShObserved - Surface Hoar – Overflaterim
* NewSnowSlabProblem - Våt nysnø
* NewSnowLooseProblem - Løs nysnø
* WindSlabProblem - Fokksnø
* PersistentWeakLayerProblem - Vedvarende svake lag
* WetSlabProblem - Slush
* WetLooseProblem - Våt løssnø
* GlideSnowProblemProblem - Glidende lag

I tillegg har vi også aggregert opp en del verdier for gjenomsnitt eller antall tilfeller de siste 2, 7, 14 og 30 dagene.


### Hva ønsker vi å finne ut?
Det vi ønsker å kunne spå er utfallet av kolonne nr. 3 "DangerLevel", som er spådd faregrad.
Faregraden er en verdi mellom 1 til 5 hvor 1 er lav og 5 er ekstremt høy fare for snøskred (forekommer svært sjeldent).

[Link til beskrivelse av faregrad](https://github.com/ingljo/machine-learning-workshop/blob/master/faregradskala.pdf).


## Steg for steg

### 1. Opprett nytt Machine Learning Workspace
* Logg på [Azure Portal](https://portal.azure.com/) med din Azure-konto.
* Klikk _New_ -> Søk etter _Machine Learning Workspace_ -> _Create_.
* Skriv inn _Worspace name_, velg _ny resource group_, og velg _DevTest service plan_. 
* Vent til tjenesten har blitt opprettet og velg så _Overview_ -> _Additional Links_ -> _Launch Machine Learning Studio_.

### 2. Opprett nytt eksperiment
* Velg _New_ nederst til venstre, velg _Experiment_ -> _Blank Experiment_.

### 3. Last opp data
* Velg _Datasets_ -> Trykk _+ NEW_ Nederst til høyre.
* Last opp NVE datasett

### 5. Split data i hva som skal brukes til trening og hva som skal brukes til evaluering i etterkant av modellen
* Vi ønsker å bruke en del av dataen til å trene modellen og en del av dataen til å evaluere i etterkant.
* Dra inn modulen _Split Data_. Koble til _Filtered Dataset_.
* Velg antall % som skal inn til splittet _Dataset1_ og _Dataset2_ og velg _Randomized Split_.
* Dra inn modulene _Train Model_ og _Score Model_. 
* Lag kobling mellom _Dataset1_ fra _Split Data_ til _Train Model_ og fra _Dataset2_ i _Split Data_ til _Score Model_.
* Lag en kobling mellom output fra _Train Model_ til _Score Model_.
* I _Train Model_ velg _DangerLevel_ som kolonne.

### 6. Velg algoritme som skal brukes for å trene modellen
* For å velge algoritme bruker vi [__Machine Learning Algorithm Cheat Sheet__](https://github.com/ingljo/machine-learning-workshop/blob/master/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)
* Du kan prøve ulike algoritmer og tune parametere for å se hva som gir best resultat.

### 7. Evaluer modellen
* For å kunne si hvor god modellen vår er må vi evaluere resultatet.
* Dra inn modulen _Evaluate Model_ og koble til _Score Model_ modulen.
* Lagre og kjør modellen.
* Høyreklikk på _Evaluate Model_ og velg _Evaluation Results_ -> _Visualize_.

### 8. Lag en Webservice for å kunne kalle modellen
* Når du er fornøyd med modellen, kan du deploye den som en webservice
* Velg _Set Up Webservice_ -> _Predictive Web Service_. Vi får nå et nytt _Predictive Experiment_ opprettet.
* Velg _Deploy Webservice_ og test den ved å bruke _Request/Response test-page_.

### 9. Konkurranse
* Ta screenshot, beskriv algoritme som er brukt og eventuelt tuning av parametre.
* Send beskrivelsen til oss på slack eller epost.
* På slutten av sesjonen går vi gjennom forslagene og kårer en vinner

