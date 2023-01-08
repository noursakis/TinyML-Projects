# Projet 2 TP TinyML  
Objectif : 
Nous sommes ammenée à utiliser la plateforme Edge Impulse, sur la carte Arduino Nano33 Sense Ble, dans le but de détecter les mots suivants: résistance, transistor, FPGA, microcontrôleur.
Par la suite l'objectif est de faire clignoter la led L graduellement à chaque fois qu’on prononce un mot particulier. 
C'est quoi Edge Impulse ? 
Edge Impulse est la principale plateforme de développement pour l'apprentissage automatique sur les périphériques, gratuite pour les développeurs et reconnue par les entreprises.

Avant de commencer le projet , il faut aller sur le site de Edge Impulse https://www.edgeimpulse.com/ et de crée un compte.
# Etape 1 : Connecter la carte sur la plateforme Edge Impulse 

1. On installe python3 et node.Js v14 
2. On installe Arduino CLI qui est une solution tout-en-un qui fournit des gestionnaires de cartes/bibliothèques, un constructeur de croquis, un détecteur de cartes, un téléchargeur et de nombreux autres outils nécessaires pour utiliser n'importe quelle carte et plateforme compatible Arduino à partir d'une ligne de commande ou d'une interface machine.
 l'installation des CLI tools ce fait via la commande :
```shell script
npm install -g edge-impulse-cli --force
```
3. On connecte la carte à l'ordinateur ,et on met à jour le firmware .
on télécgarge la derniére version du firmware depuis ele site de edge impulse et on le flash depuis flash_windows.bat

4.On lance la commande : 
```shell script
edge-impulse-daemon
```
Cela lancera un assistant qui vous demandera de vous connecter et de choisir un projet Edge Impulse ==> La carte est connectée sur la plateforme Edge Impulse.

# Etape 2 : Collecter les données 
Dans l'onglet Data Acquisition , on choist l'option microphone.
On commence par enregistrer les mots  résistance, transistor, FPGA, microcontrôleur, chaque enregistrement dure 10 secondes.
(J'ai fait plusieurs enregistrement pour chaque mot pour élargir la base de données et pour avoir un meilleur score).
Par la suite on réalise ce qu'on appelle 'split sample' pour avoir que les séquences qui constituent chaque mot.
par exemple si on prononce le mot FPGA 10 fois pendant l'enregistrment , on va avoir 10 échantillons aprés l'étape 'split Sample'
# Etape 3 : Entrainement
là on s'interesse à la partie Impulse design: 
Create impulse , choisir l'option audio pour le type de data , et le type du classifier par la suite fixer les paramétres de MFCC .
on finit par chosir les paramétres liés au réseau de neurones tel que le Nombre de cycles d'entraînement , Taux d'apprentissage ,Taille de l'ensemble de validation... et on lance l'entrainement.
Une fois l'entrainement terminé , on obtiens les résultats du test du modéle (ici 20% pour le test et 80% pour l'entrainement) qui nous renseinge sur l'exactitude des résulat obtenue depuis la partie consacré pour le test.
dans cette partie on obtient une visibilité sur les données d'entrée qui n'ont pas été efficace dans la partie test.
# Etape 4 : Deployer le modéle 
On transforme l'impulsion en un code source optimisé pour qu'il soit exécuté sur arduino , on crée alors une librairie arduino. on obtient un fichier.zip .
# Etape 5 : Deployer le modéle sur arduino
Sur l'IDE arduino , on ajoute la libraire générée par Edge Impulse depuis la section inclure une bibliothèque
dans la partie exemples on trouve "speech_detection Inferencing" on sélectionne l'exemple "nano_ble_33_sense_microphone_continuous".
on exécute le code et on  le téléverse dans la carte ,dans le Serial Monitor, on regarde le modèle ML fonctionner.
Afin de s'assurer qu'il fonctionne correctement, après les étiquettes des mots-clés (résistance, transistor, FPGA, microcontrôleur), on observe les prédictions s'imprimer à l'écran.
Pour faire clignoter la Led , dans la boucle Void loop() si la valeur des résultats de classification données par la variable "result.classification[2].value" dépasse la valeur 0.8 on allume la Led progressivement en alternant des ON et OFF séparés de delay qui diminuent de 500 ms à 50 ms.































