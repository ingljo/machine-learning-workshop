# Machine Learning Workshop

## Introduksjon
I denne workshopen skal vi "hands-on" i bruk av Azure Machine Learning Studio og
bruke reell data for � pr�ve � bygge en maskinl�ringsmodell vi kan relatere noe
vi kjenner til i dagliglivet.

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

### Hva �nsker vi � finne ut?
Det vi �nsker � kunne sp� er utfallet av kolonne nr. 3 "DangerLevel", som er sp�dd faregrad.
Faregraden er en verdi mellom 1 til 5 hvor 1 er lav og 5 er ekstremt h�y fare for sn�skred (forekommer sv�rt sjeldent).

## Steg for steg

### 1. Opprett et nytt ML Studio Worspace.


