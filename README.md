# Machine Learning Workshop

## Introduksjon
I denne workshopen skal vi "hands-on" i bruk av Azure Machine Learning Studio og
bruke reell data for å prøve å bygge en maskinlæringsmodell vi kan relatere noe
vi kjenner til i dagliglivet.

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

### Hva ønsker vi å finne ut?
Det vi ønsker å kunne spå er utfallet av kolonne nr. 3 "DangerLevel", som er spådd faregrad.
Faregraden er en verdi mellom 1 til 5 hvor 1 er lav og 5 er ekstremt høy fare for snøskred (forekommer svært sjeldent).

## Steg for steg

### 1. Opprett et nytt ML Studio Worspace.


