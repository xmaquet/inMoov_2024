# inMoov_wROS2
Ce projet a été pensé comme un développement collaboratif avec une ... IA

J'ai travaillé dès le départ avec chatGPT 4.o, dans le but de gagner du temps et de produire un système de qualité.

InMoov est un projet de robot humanoïde open-source ; mon projet est de le contrôler via ROS2 sur un Raspberry Pi5 (initialement un Odroïd N2, mais moins puissant). Ce robot intègre des fonctionnalités avancées telles que la vision par webcam avec reconnaissance faciale, un module d'IA pour l'interaction verbale, et un système de contrôle gestuel. Les actionneurs sont gérés par un MasterBrick Tinkerforge et des bricklets servos pour assurer des mouvements fluides et un contrôle fin des servos. Une interface web développée avec Django permet de surveiller et de contrôler le robot à distance. L'objectif est de créer une plateforme évolutive et interactive, capable de comprendre et de réagir à son environnement en temps réel.

**Description du projet :** 

*Tu vas m'aider à définir (dans un premier temps) une architecture logicielle qui permettra de mettre en œuvre une version personnelle du robot humanoïde InMoov. Je souhaite à terme l'installer sur un Raspberry Pi5 et utiliser ROS2 ; il faudra prévoir un module d'IA pour créer un personnage qui échangera avec le monde environnant ; il faudra gérer la vision avec une webcam et prévoir la reconnaissance faciale ; il y aura un module web pour la gestion des services. Commençons et affinons progressivement une version.*

Mots-clés : architecture logicielle, InMoov, Raspberry Pi5, ROS2, IA, vision, webcam, module web

Pour concevoir une architecture logicielle pour un robot humanoïde InMoov personnalisé, en utilisant un Raspberry Pi5 et ROS2, nous devons prendre en compte plusieurs aspects clés : le matériel, les capteurs, les moteurs, les interfaces logicielles, et les modules d'intelligence artificielle. Voici une première ébauche de l'architecture logicielle que nous pouvons affiner au fur et à mesure.

**Architecture logicielle pour un robot humanoïde InMoov personnalisé**

Pour concevoir une version personnalisée du robot humanoïde InMoov avec un Raspberry Pi 5 et ROS2, nous devons structurer l'architecture logicielle en différents modules, chacun remplissant un rôle précis. Voici une ébauche initiale que nous pourrons affiner au fil du développement.

### 1. Base de l'Architecture : ROS2

ROS2 (Robot Operating System 2) sera le cadre principal qui gère la communication entre les différents composants du robot. Chaque fonctionnalité sera organisée sous forme de nœuds ROS2, facilitant ainsi le développement, la modularité et la maintenance.

### 2. Modules de Base

#### 2.1 Module de Contrôle des Moteurs
- **Nœud ROS2 : `motor_controller_node`**
- **Matériel** : Utilisation d'un Brick Tinkerforge et de bricklets dédiés servos pour interfacer les servomoteurs.
- **Fonctions** :
  - Contrôle des moteurs des membres (bras, mains, tête).
  - Gestion des servos via des messages de type `sensor_msgs/JointState` pour indiquer les positions des articulations.
  - Calibration des servos et retour d'état des positions actuelles.
  - **Master Brick 3.2** : Le Master Brick 3.2 est utilisé pour interfacer les Bricklets dédiés aux servos. Il communique avec le Raspberry Pi 5 via USB, permettant le contrôle des servomoteurs et la gestion des retours d'état. Le `motor_controller_node` communique avec le Master Brick pour transmettre les commandes aux servos, simplifiant ainsi la gestion des actionneurs du robot.
  - **Gestion de la Gestuelle** : La gestuelle est cruciale pour rendre le robot plus interactif. Le `motor_controller_node` gérera des séquences de mouvements prédéfinies (comme des gestes de salutation ou des actions expressives) pour améliorer les interactions humaines. Ces séquences seront déclenchées soit automatiquement via le module d'IA, soit manuellement via l'interface web.

### 2.2 Module de Gestion de la Gestuelle

#### 2.2.1 Nœud ROS2 : `gesture_control_node`
- **Fonctions** :
  - **Contrôle de la gestuelle** : Ce nœud est responsable de l'animation des mouvements complexes des bras, des mains, et de la tête pour exprimer des gestes. Par exemple, des mouvements de salutation, des pointages d'objets, ou des expressions non verbales comme un hochement de tête.
  - **Prédéfinition des gestes** : Stocker et gérer une bibliothèque de gestes prédéfinis, chaque geste étant associé à une séquence spécifique de mouvements des articulations.
  - **Gestes dynamiques** : Capacité à générer des gestes dynamiques en fonction du contexte, par exemple, pointer vers un objet détecté par le module de vision.

#### 2.2.2 Bibliothèque de Gestes
- **2.2.2.1 Gestes Prédéfinis**
  - Création d'une bibliothèque de gestes avec des séquences prédéfinies pour les actions courantes comme :
    - Saluer
    - Montrer ou pointer
    - Applaudir
    - Hausser les épaules
    - Lever la tête pour montrer de l'attention
- **2.2.2.2 Création de Nouveaux Gestes**
  - Interface pour ajouter de nouveaux gestes via le module Web (`web_interface_node`), permettant aux utilisateurs de créer et personnaliser des gestes en spécifiant les séquences de mouvements.

#### 2.2.3 Intégration avec le `ai_node`
Le `gesture_control_node` est étroitement intégré avec le `ai_node`. Lorsqu'une interaction verbale se produit, le `ai_node` peut déclencher des gestes pour accompagner les paroles du robot.
- **Exemple** : Si le robot dit "Salut !", il pourrait automatiquement lever la main en même temps pour saluer.

#### 2.2.4 Interaction avec le `vision_node`
Le `gesture_control_node` peut aussi réagir aux informations du `vision_node`, par exemple, en détectant un visage humain, le robot pourrait automatiquement lever la main pour saluer ou se tourner vers la personne.
- **Suivi des objets** : Le robot pourrait suivre des objets ou des visages en utilisant à la fois le contrôle des moteurs et la gestuelle.

### 2.3 Module de Vision
- **Nœud ROS2 : `vision_node`**
- **Matériel** : Webcam connectée au Raspberry Pi 5.
- **Fonctions** :
  - Acquisition d'images en temps réel.
  - Détection et reconnaissance d'objets grâce à des modèles d'IA.
  - Sous-module de reconnaissance faciale pour identifier et suivre les visages.

### 2.4 Module d'Intelligence Artificielle
- **Nœud ROS2 : `ai_node`**
- **Fonctions** :
  - Gestion du personnage : dialogue et interaction avec les humains.
  - Utilisation d'un modèle NLP (par exemple, GPT) pour comprendre et générer des réponses.
  - Adaptation aux situations contextuelles et apprentissage continu basé sur les interactions avec les utilisateurs.
  - Coordination avec le `motor_controller_node` et le `gesture_control_node` pour déclencher des gestes appropriés lors des interactions (par exemple, saluer lorsqu'une personne est détectée).

### 2.5 Module Web
- **Nœud ROS2 : `web_interface_node`**
- **Framework** : Django
- **Fonctions** :
  - Interface web pour la gestion des paramètres du robot (moteurs, vision, IA).
  - Serveur web embarqué sur le Raspberry Pi 5 pour accéder aux données et configurations à distance.
  - Visualisation en temps réel des données (flux vidéo, positions des moteurs, état du système).
  - **Création de Nouveaux Gestes** : Permet aux utilisateurs de créer et personnaliser de nouveaux gestes en spécifiant des séquences de mouvements via l'interface web.

### 3. Communication entre les Modules
La communication entre les nœuds sera gérée par ROS2 à l'aide de topics, services et actions. Par exemple, le `vision_node` enverra des informations sur les visages détectés au `ai_node` pour permettre une interaction personnalisée avec les humains. De même, l'`ai_node` pourra envoyer des commandes au `motor_controller_node` et au `gesture_control_node` pour déclencher des mouvements gestuels spécifiques.

### 4. Gestion des Capteurs
- **Capteurs** :
  - Capteurs de proximité (ultrasons, infrarouges) pour la détection d'obstacles.
  - Microphones pour la reconnaissance vocale.
- Chaque capteur sera géré par un nœud ROS2 spécifique qui transmettra les données pertinentes aux autres modules.

### 5. Gestion de la Performance
Pour optimiser l'utilisation du Raspberry Pi 5, nous utiliserons des modèles IA légers (comme TensorFlow Lite) et veillerons à limiter la consommation des ressources par les nœuds ROS2. L'objectif est de maintenir un équilibre entre fonctionnalité et efficacité.
- **Système d'exploitation recommandé** : Utilisation d'**Ubuntu Server 22.04 LTS** sur le Raspberry Pi 5. Ubuntu Server est optimisé pour les projets ROS2, offrant une grande compatibilité et des performances stables, notamment avec les versions ROS2 Humble et Rolling. Ubuntu permet également l'utilisation de Docker pour une gestion simplifiée des nœuds et des conteneurs.

### 6. Déploiement et Maintenance
- **Conteneurisation** : Utilisation de Docker pour conteneuriser les nœuds afin de simplifier le déploiement et la gestion des versions.
- **Surveillance** : Mise en place de mécanismes de logging et de monitoring pour superviser le fonctionnement du robot en temps réel.

### 7. Script d'Installation
Pour automatiser la mise en place de l'environnement logiciel sur le Raspberry Pi 5, nous décrivons un script d'installation structuré en plusieurs phases :

#### 7.1 Préparation du Système
- **Mise à jour du Système** : Mettre à jour les packages pour garantir la stabilité.
- **Installation des Dépendances** : Installer des outils de développement de base (Python, Docker, etc.).
- **Configuration du Réseau** : Paramétrer l'accès réseau pour permettre les téléchargements et les mises à jour.

#### 7.2 Installation de ROS2 (Humble)
- **Ajout des clés et des dépôts APT** : Ajouter les sources nécessaires pour installer ROS2 Humble.
- **Installation de ROS2** : Installer les composants principaux de ROS2, incluant les outils de développement (`colcon`, `rosdep`).
- **Configuration de ROS2** : Créer un espace de travail ROS2, configurer `rosdep`, et installer les dépendances.

#### 7.3 Installation des Composants Tinkerforge
- **Téléchargement des Bibliothèques Tinkerforge** : Installer les bibliothèques Python nécessaires.
- **Configuration du Master Brick** : Tester la communication avec les Bricklets.

#### 7.4 Déploiement des Nœuds ROS2
- **Clonage des Repositories** : Cloner le code du projet pour chaque nœud.
- **Compilation du Code** : Compiler les nœuds ROS2 en utilisant `colcon build`.
- **Configuration des Lancements** : Créer des fichiers de lancement pour démarrer les nœuds.

#### 7.5 Installation de Docker
- **Installation de Docker** : Installer Docker pour la conteneurisation des modules.
- **Création des Conteneurs** : Créer des images Docker pour chaque nœud, si nécessaire, pour permettre une gestion plus souple et une mise à jour simplifiée des versions.

#### 7.6 Installation du Serveur Web (Django)
- **Installation de Python et Django** : Installer Python 3 et le framework Django.
- **Déploiement du Module Web** : Créer l'interface pour la gestion des paramètres et la création de nouveaux gestes.
- **Configuration des Permissions** : S'assurer que le serveur web peut accéder aux fichiers nécessaires et communiquer avec les autres nœuds.

#### 7.7 Configuration des Services de Démarrage
- **Création de Services Système** : Créer des services (`systemd`) pour démarrer automatiquement les nœuds ROS2, le serveur Django, et d'autres processus critiques lors du démarrage du Raspberry Pi.
- **Test de l’Autonomie** : Tester le redémarrage automatique pour vérifier que l'ensemble du système est opérationnel après un cycle d'arrêt/redémarrage.

#### 7.8 Tests et Validation
- **Tests de Communication** : Valider la communication entre le Raspberry Pi et les Bricks Tinkerforge.
- **Tests des Nœuds ROS2** : Lancer des tests de bout en bout pour vérifier que les nœuds sont fonctionnels, que la communication est établie entre eux et que les gestes sont bien exécutés.
- **Mise en Place du Monitoring** : Installer des outils de supervision pour vérifier la consommation des ressources et assurer la stabilité du système.

---

Cette architecture est une première base que nous pourrons affiner et ajuster en fonction des besoins évolutifs du projet. Quels aspects souhaitez-vous approfondir ou modifier à ce stade ?



**Étapes d'installation et de déploiement**
