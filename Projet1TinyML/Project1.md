# Projet 1 TP TinyML  
Objectif : 
Nous sommes ammenée à écrire un programme Arduino en utilisant TensorFlowLite sur la carte Nano 33 Sense BLE.
Le but du programme c'est de détecter le signe Yes (✓) ainsi que le signe (X) via un processus de reconaissance de gestes / mouvements.

# Etape 1 : Préparer le modéle ==> Collecte de données 

Pour la collecte de données on va ce qu'on appelle IMU "Inertial Measurement Unit"
C'est Quoi IMU ?
Il s'agit comme son nom l'indique , l'Unité de mesure inertielle.
La carte nano 33 sense BLE utilise le ST Micro LSM9DS1 dispose de : 
1. Gyroscope - mesure la vitesse angulaire, c'est-à-dire "à quelle vitesse et selon quel axe est-ce que je tourne ?
Accéléromètre - mesure l'accélération, qui indique la vitesse à laquelle la vitesse change - "à quelle vitesse j'accélère ou je ralentis ?".
2. Magnétomètre - qui mesure la puissance et la direction des champs magnétiques.
Nous n'utilisons que l'accéléromètre et le gyroscope pour ce projet.

La bibliothèque Arduino permet au capteur de rapporter 119 points de données par seconde, ce qui signifie qu'un nouvel ensemble de données est reçu toutes les 8,4 ms.
Chaque entrée a 3 axes = X, Y, Z.
Les entrées du modèle seront constituées de 1 seconde de données = 119 échantillons.
Chaque échantillon sera [Ax, Ay, Az, Gx, Gy, Gz].
Chaque enregistrement de geste contient 119 rangées de 6 points de données enregistrées en une seconde.

1. Pour visualiser et collecter les données, on commence par connecter la carte , d'exécuter et téléverser le code IMU_Capture.ino  qui existe sous cette arboréscence :  
tinyml-workshop/ArduinoSketches/IMU_Capture/IMU_Capture.ino , on visualise les données sur le port série.
2.On dispose de deux mouvements True et False , on commence par enregistrer les données pour chaque mouvement: 
On tiens la carte en effectuant chaque mouvement : Correct/False.
Pour ce cas j'ai effectué 20 fois le mouvement True et 20 fois le mouvement False.


3. On copie colle les données dans un fichier .csv correspondant pour chaque mouvement : correct.csv et false.csv

# Etape 2 : Machine Learning  ==> Entrainer la base de données 
Nous allons utiliser Google Colab pour former notre modèle d'apprentissage automatique.
On utilise google collab pour exécuter notre modèle d'apprentissage automatique dans un navigateur Web.
On exécute le colab Projet1_TinyML , localisé dans ce dossier.
On met les fichiers  .csv dans l'arborescence du colab , on exécute le code pour entrainer la base de donnée.
une fois le modéle entrainé , on le convertit au format TFlite.
La derniére étape consiste à encoder le modèle dans un fichier header arduino.

# Etape 3 : Classifier les données IMU
On exécute le code situé dans le chemin : tinyml-workshop/ArduinoSketches/IMU_Classifier/IMU_Classifier.ino
On remplace le contenu de model.h par la version que nous avons téléchargée de Colab.
Pour répondre à la consigne du projet , on modifie la version initiale du code IMU_Classifier.ino
dans la boucle Void loop() , on commence par poser la premiére question : "Vous êtes etudiant en Instrumentation ?".
Par la suite on exécute la boucle void Answer() qui lit la réponse :attend que les données d'acceleration gyroscope soient disponible, une fois la réponse recue, on affiche la deuxiéme question :"" Vous suivez le cours de TinyML ?""  , une fois la boucle void Answer() donne une réponse , on affiche la 3éme question  "Vous aimez bien cette matiére ?"".
à la fin des 3 questions , on réexécute le débur du void loop() donc la premiére question.
 



































