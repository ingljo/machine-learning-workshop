# Machine Learning Workshop

## Introduksjon
I denne workshopen skal vi "hands-on" i bruk av Azure Machine Learning Studio og
bruke reell kundedata for � bygge en machine learning modell.

## Om oss
* Ingvar Ljosland, [ingvar.ljosland@bouvet.no](mailto:ingvar.ljosland@bouvet.no)
* Markus Skallist, [markus.skallist@bouvet.no](mailto:markus.skallist@bouvet.no)
* Kristian Ekle, [kristian.ekle@bouvet.no](mailto:kristian.ekle@bouvet.no)

__Kontakt oss gjerne p� Slack__

### L�ringsm�l
M�lene for denne workshopen er � kunne:
1. L�re hvordan Azure Machine Learning Studio fungerer.
2. Importere og manipulere data som input til ML modell.
3. Ta bevisste valg av algoritmer for bruk til � bygge en ML modell.
4. Kunne trene og bygge en ML modell ved bruk av importert data.
5. � kunne bruke den bygde ML modellen i kode / webservice kall.

## Demo
For � f� en liten introduksjon til Azure ML Studio, gjennomf�rer vi en 
liten demo, slik at vi g�r gjennom stegene for � bygge en modell i ML Studio.

[Link til demo](https://github.com/ingljo/machine-learning-workshop/blob/master/demo/demo.md).

## Workshop tema - Kan vi sp� faren for sn�skred?
NVE samler inn data daglig over sn�forhold og hvilke sn�skred som har g�tt i omr�det. Dette bruker de
blant annet observat�rer til. I tillegg bruker de data fra Metrologisk institutt for v�r for � kunne
sp� hvor stor sn�skredfare det er de ulike varslingsregionene.
Du kan til en hver tid se hva faren for sn�skred er p� http://www.varsom.no/snoskredvarsling/.

For � f� til varselet m� noen p� NVE manuelt se p� hvilke observasjoner som er gjort i omr�det kombinert
med hvilket v�r det har v�rt i omr�det den siste tiden.

Er det mulig for oss � kunne sp� det samme varselet automatisk ved hjelp av maskinl�ring?

### Data
NVE legger ut sine data lisensiert under Norsk lisens for offentlige data [NLOD](https://data.norge.no/nlod/no) og vi har f�tt
sammenstilt data fra sesongen 2016 til bruk i denne workshopen.

[Her kan du laste ned sammenstilt data for 2016-sesongen til bruk i denne workshopen.](https://github.com/ingljo/machine-learning-workshop/blob/master/data/SeasonDataExtended_All_2016-08-01_2017-06-01.tsv)

#### Beskrivelse av kolonnene
* Date - Dato for innsamlet data
* RegionId - Varslingsregion
* DangerLevel - Varslet faregrad
* ApsTemperature - Gjennomsnittstemperatur siste d�gn
* ApsTemperatureMin - Minimumstemperatur siste d�gn
* ApsTemperatureMax - Makstemperatur siste d�gn
* ApsPercipitation - Nedb�r siste d�gn
* ApsPercipitationMostExposedArea - Nedb�r mest utsatt omr�de siste d�gn
* ApsFreezingElevationsTemperature - Grense for frysetemperatur (moh)
* FcObserved - Faceted Crystals � Kantkornet sn�
* DhObserved - Depth Hoar � Begerkrystaller
* ShObserved - Surface Hoar � Overflaterim
* NewSnowSlabProblem - V�t nysn�
* NewSnowLooseProblem - L�s nysn�
* WindSlabProblem - Fokksn�
* PersistentWeakLayerProblem - Vedvarende svake lag
* WetSlabProblem - Slush
* WetLooseProblem - V�t l�ssn�
* GlideSnowProblemProblem - Glidende lag

I tillegg har vi ogs� aggregert opp en del verdier for gjenomsnitt eller antall tilfeller de siste 2, 7, 14 og 30 dagene.


### Hva �nsker vi � finne ut?
Det vi �nsker � kunne sp� er utfallet av kolonne nr. 3 "DangerLevel", som er sp�dd faregrad.
Faregraden er en verdi mellom 1 til 5 hvor 1 er lav og 5 er ekstremt h�y fare for sn�skred (forekommer sv�rt sjeldent).

[Link til beskrivelse av faregrad](https://github.com/ingljo/machine-learning-workshop/blob/master/faregradskala.pdf).


## Steg for steg

### 1. Opprett nytt Machine Learning Workspace
* Logg p� [Azure Portal](https://portal.azure.com/) med din Azure-konto.
* Klikk _New_ -> S�k etter _Machine Learning Workspace_ -> _Create_.
* Skriv inn _Worspace name_, velg _ny resource group_, og velg _DevTest service plan_. 
* Vent til tjenesten har blitt opprettet og velg s� _Overview_ -> _Additional Links_ -> _Launch Machine Learning Studio_.

### 2. Opprett nytt eksperiment
* Velg _New_ nederst til venstre, velg _Experiment_ -> _Blank Experiment_.

### 3. Last opp data
* Velg _Datasets_ -> Trykk _+ NEW_ Nederst til h�yre.
* Last opp NVE datasett

### 5. Split data i hva som skal brukes til trening og hva som skal brukes til evaluering i etterkant av modellen
* Vi �nsker � bruke en del av dataen til � trene modellen og en del av dataen til � evaluere i etterkant.
* Dra inn modulen _Split Data_. Koble til _Filtered Dataset_.
* Velg antall % som skal inn til splittet _Dataset1_ og _Dataset2_ og velg _Randomized Split_.
* Dra inn modulene _Train Model_ og _Score Model_. 
* Lag kobling mellom _Dataset1_ fra _Split Data_ til _Train Model_ og fra _Dataset2_ i _Split Data_ til _Score Model_.
* Lag en kobling mellom output fra _Train Model_ til _Score Model_.
* I _Train Model_ velg _DangerLevel_ som kolonne.

### 6. Velg algoritme som skal brukes for � trene modellen
* For � velge algoritme bruker vi [__Machine Learning Algorithm Cheat Sheet__](https://github.com/ingljo/machine-learning-workshop/blob/master/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)
* Du kan pr�ve ulike algoritmer og tune parametere for � se hva som gir best resultat.

### 7. Evaluer modellen
* For � kunne si hvor god modellen v�r er m� vi evaluere resultatet.
* Dra inn modulen _Evaluate Model_ og koble til _Score Model_ modulen.
* Lagre og kj�r modellen.
* H�yreklikk p� _Evaluate Model_ og velg _Evaluation Results_ -> _Visualize_.

### 8. Lag en Webservice for � kunne kalle modellen
* N�r du er forn�yd med modellen, kan du deploye den som en webservice
* Velg _Set Up Webservice_ -> _Predictive Web Service_. Vi f�r n� et nytt _Predictive Experiment_ opprettet.
* Velg _Deploy Webservice_ og test den ved � bruke _Request/Response test-page_.

### 9. Konkurranse
* Ta screenshot, beskriv algoritme som er brukt og eventuelt tuning av parametre.
* Send beskrivelsen til oss p� slack eller epost.
* P� slutten av sesjonen g�r vi gjennom forslagene og k�rer en vinner

