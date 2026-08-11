
layout: post
title: "Litige avec Chip'n Modz – Un écran PSVita "neuf & original" qui s'avère être une contrefaçon dangereuse"
date: 2026-08-07
categories: [hardware, modding, litige]
---

## Contexte

Passionné de rétro-ingénierie et de développement sur console portable, j'ai récemment acheté un écran AMOLED complet pour PSVita 1000 sur le site Chip'n Modz. Le produit était décrit comme **"neuf & original"**,
avec une photo montrant les deux bagues chromées sous les joysticks.

**Commande passée le :** [date]  
**Prix :** 64,90 € (59,90 + 5 € de port)
![Annonce web](images/AnnonceWeb.JPG)  

---

## Ce que j'ai reçu

Dès l'ouverture du colis, j'ai constaté plusieurs anomalies :

### 1. Absence des bagues chromées
![Absence des bagues](images/DiffOldNew.JPG)  
![Mesure PAC avec bagues](images/WithBGOld.JPG)  
![Mesure PAC sans bagues](images/WithBGNew.JPG)  
*Photo de l'écran reçu – les bagues sont absentes, contrairement à la photo du site.*

### 2. Soudure de la nappe amateur
![Nappe brasée de travers](images/NappeNew.JPG)  
*La nappe est brasée manuellement, avec un décalage d'environ 0.6 mm. Du flux non nettoyé est encore présent.*

### 3. Défauts critiques détectés au microscope
- **Piste 17 (GND) :** non reliée (coupée), cette piste est négligeable
- **Pistes 12 (SPI_CS) et 13 (INT) :** pontées par un excès d'étain

![Pont de soudure](images/NappeNew.JPG)  
*Court-circuit visible entre les pistes 12 et 13.*

![PSvita Original soudure machine](images/NappeOld.JPG) 
*Travail fait par les labo de Sony, propre*
---

## Diagnostic technique complet

J'ai utilisé mon **Analog Discovery Studio** pour effectuer des tests de continuité et de court-circuit.

Voici les ref' Pinout pour la nappe : 

| Broches | Signal | fonction |
|.........|........|..........|
| 1 |	GND |	Masse |
| 2 | AVDD |	Alimentation analogique |
| 3 |	AVDD | Alimentation analogique |
| 4 |	VDD |	Alimentation numérique |
| 5 |	VDD |	Alimentation numérique |
| 6 |	GND |	Masse |
|7 | GND |	Masse |
|8 | SPI_CLK |	Horloge SPI |
|9 | GND |	Masse |
|10 | SPI_MISO | Data SPI (Master In Slave Out) |
|11 | SPI_MOSI | Data SPI (Master Out Slave In) |
|12 | SPI_CS | Chip Select (sélection du composant) |
|13 | INT |	Interruption (signal d'événement) |
|14 | RESET |	Réinitialisation |
|15 | SDA |	I2C Data (ou Data SPI) |
|16 | SCL |	I2C Clock (ou SPI Clock) |
|17 |GND |	Masse |

le Datasheet de l'ecran : 

![Datasheet Samsung AMOled](images/AMS495QA04_datasheet_V5.PDF)  

### Résultats des mesures

| Test | Résultat | Statut |
|------|----------|--------|
| Continuité piste 17 | Coupée | ❌ |
| Court-circuit piste 12-13 | Pont présent | ❌ |
| Absence de bagues | Constaté | ❌ |

### Risque pour la console

Brancher cet écran tel quel sur une carte mère PSVita, c'est :
- Court-circuiter les lignes de communication I2C (risque de griller le contrôleur d'affichage)
- Laisser entrer de la poussière par les trous des joysticks (absence de bagues)

**J'ai refusé de le brancher** et j'ai contacté le service client.

---

## Échanges avec le service client

Voici mes messages (anonymisés pour respecter le secret des correspondances) :

> **Mon premier message :**  
> *"Bonjour, j'ai reçu l'écran mais il manque les bagues chromées et la nappe présente des défauts de soudure critiques. Je demande un remboursement intégral."*

> **Leur réponse (résumée) :**  
> *"Nos produits sont neufs et originaux, directement issus des usines Sony. Vous devez récupérer les bagues sur votre ancien écran. Nous ne remboursons pas les produits endommagés par l'utilisateur."*

> **Mon deuxième message :**  
> *"J'ai des photos macro des défauts. Une piste est coupée, deux sont pontées. C'est un danger pour la console. Je ne demande pas un geste commercial, mais un remboursement pour non-conformité."*

> **Leur dernière réponse (résumée) :**  
> *"Si vous n'êtes pas capable de retirer de simples bagues, ne vous lancez pas dans la réparation de console."*

---

## Réparation et preuve de l'amateurisme

Plutôt que de renvoyer l'écran (et perdre ma seule preuve), j'ai décidé de le réparer moi-même tout en documentant chaque étape.
Je me moque de leurs 60€, là n'est pas le problème ! C'est la ferveur qu'ils mettent a faire passer les clients pour des navets ! 
Et le souci de dangerosité pour la console qui je le rappel, devait recevoir cet écran pour restauration et non pour déterrioration !
J'ai tout simplement un problème avec les gens de mauvaises fois !
Donc réparons cette beauté et voyons si la dalle est bien une Samsung, car en effet la nappe sortante pui est très mal brasé, comporte bien le logo Samsung,
mais ne nous y fions pas pour l'instant ! 

### Matériel utilisé
- Metcal MX-5000 (station de soudure professionnelle)
- Alcool isopropylique 99%
- Fil de liaison (wire-bonding)

### Étapes de la réparation
1. **Nettoyage** du flux résiduel à l'alcool.
2. **Dépontage** des pistes 12 et 13 avec un fer fin.
3. **Réparation** de la piste 17 avec un fil de liaison.
4. **Contrôle** final avec l'Analog Discovery Studio.

![Réparation de la piste](images/reparation_piste.jpg)  
*La piste 17 réparée avec un fil de liaison – test de continuité OK.*

### Test final
Une fois réparé, l'écran a été branché sur ma PSVita et fonctionne parfaitement.

![Écran fonctionnel](images/ecran_allume.jpg)  
*L'écran réparé en fonctionnement – aucune anomalie.*

---

## Ce que cela m'aurait coûté si je l'avais branché

| Élément | Coût estimé |
|---------|-------------|
| Carte mère PSVita | 140 € |
| Main d'œuvre pour réparation | 50-100 € |
| **Total** | **130-220 €** |

J'ai évité ce désastre grâce à mon expérience et mes outils. Un novice aurait grillé sa console.

---

## Conclusion et avertissement

**Chip'n Modz vend des produits :**
- ❌ Non conformes à la description
- ❌ Dangereux pour le matériel
- ❌ Avec un service client méprisant

**Je ne demande pas de remboursement.** Je veux que la communauté soit informée.

**Si vous achetez un écran chez eux :**
- Testez-le impérativement avec un multimètre avant de le brancher
- Vérifiez la présence des bagues
- Inspectez la nappe à la loupe

**Liens utiles :**
- [Mon avis sur Trustpilot](https://www.trustpilot.com/...)
- [SignalConso – signalement officiel](https://signal.conso.gouv.fr/)

---

*Documenté et rédigé par [Ton Pseudo] – Passionné de rétro-ingénierie depuis 20 ans.*
