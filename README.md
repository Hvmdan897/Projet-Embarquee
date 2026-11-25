# Lampe d'ambiance intelligente

## But du projet
Une lampe qui s'adapte à la lumière de la pièce, détecte les mouvements, peut être réglée à la main avec un potentiomètre et s'éteint quand on le veut. Elle a quatre modes, changés avec un bouton, un écran OLED qui affiche l'état et un buzzer qui signale les erreurs.

## Membres
- Hamdan (seul membre, autorisé par le prof)

## Liste du matériel
- Arduino Nano ESP32 
- Photocell Sensor (capteur analogique)
- Potentiomètre [Analog Rotation Sensor] (capteur analogique)
- PIR Motion Sensor (capteur numérique)
- Push button (changement de mode)
- White LED Module (sortie PWM, la lampe)
- RGB LED Module (sorties marche/arrêt, indicateur du mode)
- Passive Buzzer (sortie PWM, signal d'erreur)
- 128x32 OLED Display with I2C (composant nouveau)
- Résistances [selon les modules, à confirmer]

## Branchements et broches

| Composant | Broche imprimée | Type |
-![Diagramme visuelle](<diagramme.png>)
-
| Photocell     | A0 |  Entrée analogique                  |
| Potentiomètre | A1 |  Entrée analogique                  |
| PIR           | D2 |  Entrée numérique                   |
| Bouton        | D3 |  Entrée numérique (pull-up interne) |
| White LED     | D5 |  Sortie PWM                         |
| RGB, rouge    | D6 |  Sortie numérique                   |
| RGB, vert     | D7 |  Sortie numérique                   |
| RGB, bleu     | D8 |  Sortie numérique                   |
| Buzzer        | D9 |  Sortie PWM                         |
| OLED SDA      | A4 |  I2C                                |
| OLED SCL      | A5 |  I2C                                |



## Rôle de chaque composant
- **Arduino Nano ESP32** : il lit les capteurs, applique les modes et commande les sorties.
- **Photocell** : mesure la lumière de la pièce. Elle règle la luminosité en mode Auto.
- **Potentiomètre** : règle la luminosité de 0 à 100 % en mode Manuel.
- **PIR** : détecte un mouvement. En mode Mouvement, la lampe s'allume quand quelqu'un passe.
- **Push button** : change de mode à chaque appui.
- **White LED** : la lampe elle-même, dont la luminosité est réglée en PWM.
- **RGB LED** : affiche le mode par sa couleur (vert en Auto, bleu en Mouvement, jaune en Manuel, éteint en Éteint).
- **Buzzer** : émet un bip quand la photocell ou le potentiomètre est débranché ou hors plage.
- **OLED 128x32** : affiche le mode, la luminosité et les messages d'erreur.


## Modes de fonctionnement
Le bouton fait passer d'un mode au suivant, dans cet ordre : Auto, Mouvement, Manuel, Éteint, puis Auto.

- **Auto** : la luminosité suit la lumière de la pièce mesurée par la photocell. La RGB est verte.
- **Mouvement** : la lampe s'allume à pleine intensité quand le PIR détecte un mouvement, et s'éteint après un délai sans mouvement. La RGB est bleue.
- **Manuel** : le potentiomètre règle la luminosité de 0 à 100 %, affichée sur l'OLED. La RGB est jaune.
- **Éteint** : la lampe et le buzzer sont coupés, et la RGB est éteinte.

## Fonctions principales
- `lire_luminosite()` : lit la photocell et convertit la valeur en pourcentage.
- `lire_potentiometre()` : lit le potentiomètre et convertit la valeur en pourcentage.
- `detecter_mouvement()` : lit le PIR et gère le délai d'extinction.
- `lire_bouton()` : détecte un appui et change de mode.
- `mode_auto()`, `mode_mouvement()`, `mode_manuel()`, `mode_eteint()` : gèrent la lampe selon le mode.
- `afficher_etat()` : écrit le mode et la luminosité sur l'OLED.
- `afficher_couleur_mode()` : allume la RGB selon le mode.
- `verifier_capteurs()` : détecte un capteur débranché ou une valeur hors plage, et active l'alarme.
- `mettre_a_jour_alarme()` : fait bipper le buzzer sans bloquer le programme.

Le programme utilise `ticks_ms()` et `ticks_diff()` pour ne jamais bloquer la boucle principale.

## Limites connues
- La luminosité en mode Auto est calibrée pour une pièce précise. Les seuils doivent être ajustés si la lumière change beaucoup.
- Le PIR détecte un mouvement, pas une présence immobile.
- Le PIR a besoin de 30 à 60 secondes pour se stabiliser au démarrage.
- [Autres limites à compléter après les tests]

## Utilisation de l'IA
- Outil utilisé : Claude (Anthropic).
- Usages principaux : compréhension de la fiche technique de l'OLED, proposition de la structure du programme, aide à la rédaction du README et vérification de la logique des modes.
- Le code a été relu, compris et testé par l'auteur, qui est responsable de son fonctionnement.
