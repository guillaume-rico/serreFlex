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

### Pilotage manuel 

Etape par étape :

L'utilisateur tourne le bouton dans le sens de la montée :

![Bouton montée](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/bouton_montee.png)

Cela permet au courant de parcourir la branche de gauche du circuit de pilotage. Ce faisant, il alimente la bobine du contacteur :

![Commande montée](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/commande_montee.png)

La bobine enclenche l'alimentation du moteur :

![Puissance montée](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/puissance_montee.png)

Le moteur s'enclenche et ouvre le volet latéral de la serre. Lorsque le volet est completement ouvert un contact fin de course vient ouvrir le circuit de commande pour arreter le moteur

![Arret montée](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/fin_de_course_montee.png)

Et a contrario lors de la descente :

![Bouton descente](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/bouton_descente.png)

![Commande descente](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/commande_descente.png)

L'alimentation de deux des phases du moteur est inversée permettant la rotation dans l'autre sens :

![Puissance descente](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/puissance_descente.png)

Le deuxième contact fin de course permet de stopper le moteur :

![arret descente](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/fin_de_course_descente.png)

### Pilotage par le raspberry

La logique est la même que pour le pilotage manuel. Le circuit de commande se monte en parrallèle du circuit manuel.
 

# Réalisation 

## Schéma électrique

Les schémas électrique sont disponibles au format QET et au format PDF.
Le format QET est modifiable par QElectrotech (Logiciel gratuit et open source) [disponible ici](https://qelectrotech.org/).

Retrouvez le schéma électrique de l'ensemble ici :
 * Format [QET](https://github.com/guillaume-rico/serreFlex/raw/master/sch/ouverture_serre.qet) 
 * Format [1.0 PDF](https://github.com/guillaume-rico/serreFlex/raw/master/sch/ouverture_serre_1.0.pdf)

 
![Schéma électrique folio 3 sur 6](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/sch_elec_3.png)
![Schéma électrique folio 4 sur 6](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/sch_elec_4.png)
![Schéma électrique folio 5 sur 6](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/sch_elec_5.png)
![Schéma électrique folio 6 sur 6](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/sch_elec_6.png)
 
 
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
|  | 1 | Presse étoupe RJ45 |  |  |
|  | 1 | Presse étoupe capteur de température |  |  |
|  | 1 | Boitier | |  |
| SAU | 1 | Arret d'urgence | Eaton 216516 - M22-PV/K11 |  |
| SAU | 1 | Etiquette Arret d'urgence | Eaton 216465 - M22-XAK1 |  |
|  | 1 | Capteur de température |  |  |

# Code

# Autre

## Acquition d'une donnée 4-20mA

Pour réaliser la mesure d'une donnée 4-20mA avec un raspberry, il faut réaliser deux actions :

 * Convertir la donnée courant en tension
 * Réaliser la mesure de la tension 
 
### Conversion courant tension 

En général, les capteurs industrielles 4/20mA demandent une alimentation 24V. Il y  a deux type de capteurs : les capteurs 2 fils et 3 fils. La différence tient dans l'ajout d'un fil de masse.

On alimente le capteur en 24V puis on va lire le courant passant par la pate de sortie. Pour générer une tension, on fait passer le courant par une résistance :

![Branchement electrique](https://raw.githubusercontent.com/wiki/guillaume-rico/serreFlex/img/branchement_4_20mA.png)

Calcul de la valeur de la résistance. Le composant utilisé classiquement est l'ADS1X15 (ADS1015, ADS1115). La tension de lecture maximum est de 4.096V pour un courant de 20mA.

En appliquant U = R x I => R = U / I => R = 4.096 / 0.02 => R = 205 Ohm

Ce n'est pas une valeur standard. On choisit la valeur de 200Ohm ce qui donnera une tension maximum de U = R x I = 200 * 0.02 = 4V

### Acquisition

Plusieurs modules ADC permettent de réaliser la mesure de la tension. Un modèle classique est l'ADS1X15 (ADS1015, ADS1115).

Il communique avec la lisaison I2C du raspberry.

Un bon guide pour faire la mesure : https://cdn-learn.adafruit.com/downloads/pdf/adafruit-4-channel-adc-breakouts.pdf

On construit la config :

| Bit de config |    Valeur  |
| ------------- | ---------- |
| CONFIG_CQUE_NONE       | 0x0003 |
| CONFIG_CLAT_NONLAT     | 0x0000 |
| CONFIG_CPOL_ACTVLOW    | 0x0000 |
| CONFIG_CMODE_TRAD      | 0x0000 |
| CONFIG_MODE_SINGLE     | 0x0100 |
| Concatenation de ces registres :  | 0x0103 (259) |
| Sélection de la vitesse |  |
| CONFIG_DR_1600SPS      | 0x0080 |
| Concatenation          | 0x0183 |
| Set PGA/voltage range, defaults to +-6.144V |  |
| CONFIG_PGA_4_096V      | 0x0200 |
| Concatenation          | 0x0383 |
| Select channel |  |
| CONFIG_MUX_SINGLE_0    | 0x4000 |
| Concatenation           | 0x4383 |
| CONFIG_MUX_SINGLE_1    | 0x5000 |
| CONFIG_MUX_SINGLE_2    | 0x6000 |
| CONFIG_MUX_SINGLE_3    | 0x7000 |
| Start acquisition | |
| CONFIG_OS_SINGLE       | 0x8000 |
| Concatenation  4.096    | 0xC383 (50051) |
| 4.096 Voie 1 | 0xC383 |
| 4.096 Voie 2 | 0xD383 |
| 4.096 Voie 3 | 0xE383 |
| 4.096 Voie 4 | 0xF383 |

En résumé, pour lancer l'acquisition d'une des 4 voies :
 * Voie 1 : /usr/sbin/i2cset -y 1 0x48 0x01 0xC3 0x83 i
 * Voie 2 : /usr/sbin/i2cset -y 1 0x48 0x01 0xD3 0x83 i
 * Voie 3 : /usr/sbin/i2cset -y 1 0x48 0x01 0xE3 0x83 i
 * Voie 4 : /usr/sbin/i2cset -y 1 0x48 0x01 0xF3 0x83 i

Ensuite On attend 10ms

lecture du registre qui contient les entrées :

/usr/sbin/i2cget -y 1 0x48 0x00 w

Maintenant qu'on a la valeur, il suffit de faire la conversion pour avoir la tension :

Tension = (Valeur du registre) / 32768.0 * 4.096

Exemple :

/usr/sbin/i2cset -y 1 0x48 0x01 0xC3 0x83 i

/usr/sbin/i2cget -y 1 0x48 0x00 w

Résultat : 0x3008 soit 12296

Tension = 12296 /32768 * 4.096 = 1,537V


