# serreFlex

# Introduction

Ce projet a pour but de piloter l'ouverture de serres.
On retrouve ce genre de produit dans le commerce. Par exemple le C1MA/SA/Auto .

## Fonctionnement

Cet outil doit permettre de répondre aux contraintes suivantes :
 * Ouverture / fermeture des deux panneaux latérales de la serre. Les deux panneaux sont pilotés ensemble
 * Fonctionnement en mode manuel : l'utilisateur a acèès à un bouton rotatif permettant de forcer l'ouverture ou la fermeture 
 * Fonctionnement automatique : Pilotage par un automate, un logiciel de supervision ou a distance
 * Utilisation de capteur fin de course permettant de détecter la position haute et basse de l'ouverture

## Logique de pilotage

Le moteurs utilisés sont des moteurs triphasés de 250W (0.76A). En tourant dans un sens ils ouvrent les serres, dans l'autre sens il ferment la serre.
Pour ouvrir, il suffit d'alimenter les moteurs. Pour fermer, les moteurs sont alimentés en inversant deux phases.

# Réalisation 

## Schéma électrique

Les schémas électrique sont sidponibles au format QET et au format PDF.
Le format QET est modifiable par QElectrotech (Logiciel gratuit et open source) [disponible ici](https://qelectrotech.org/).

Retrouvez le schéma électrique de l'ensemble ici :
 * Format [QET](https://github.com/guillaume-rico/serreFlex/raw/master/sch/ouverture_serre.qet) 
 * Format [1.0 PDF](https://github.com/guillaume-rico/serreFlex/raw/master/sch/ouverture_serre_1.0.pdf)

## Nomenclature

Retrouvez ici la liste du matériel nécéssaire :

| Code | Nombre | Matériel                 | Exemple de Référénce | Remarque |
| ---- | ------ | -------------------------| -------------------- | -------- |
| KM11 KM12 KM21 KM22 | 4 | Contacteur 3P + Aux. NC  | Siemens - 3RT2015-1BB | Il serait souihaitable de prendre des contacteurs mécaniquement bloqués |
| Q10 Q20  | 2 | Disjoncteur de puissance | Siemens - 3RV2011-0K |
| S1H/S1B S2H/S2B | 2 | Switch 3 positions | Eaton - 216520 |  |
| S1H/S1B S2H/S2B | 2 | Porte-Ètiquette |  Eaton 216485 | Trouver un porte étiquette avec des fleches |
| F1   | 1 | Disjoncteur général | Siemens - 5SY4310-6 |  |
| AL24 | 1 | Alimentaion 24VDC 0.5A | PHOENIX 2868635 |  |
| AL5 | 1 | Alimentaion 5VDC | TDK-Lambda - DSP-10-5 |  |
| RPI | 1 | Raspberry Pi | Raspberry Pi 3 B+ |  |
| GPO | 1 | Bloc relai | |  |
|  | 2 | Presse étoupe D16 | LAPP SKINTOP CLICK 16 RAL 7035 |  |
|  | 3 | Presse étoupe D25 | LAPP SKINTOP CLICK 25 RAL 7035 |  |
|  | 1 | Boitier | |  |
| SAU | 1 | Arret d'urgence | Eaton 216516 - M22-PV/K11 |  |
| SAU | 1 | Etiquette Arret d'urgence | Eaton 216465 - M22-XAK1 |  |

# Code

