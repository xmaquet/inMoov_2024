**Pile Logicielle de Base pour le Projet InMoov_wROS2**

Cette pile logicielle est essentielle pour mettre en œuvre les fonctionnalités du projet InMoov_wROS2 sur les deux plateformes matérielles du projet : l'ODROID N2 et le Raspberry Pi 4. Voici les logiciels à installer pour assurer un fonctionnement optimal du système.

### 1. ODROID N2 4G

#### Système d'Exploitation
- **Ubuntu Server 22.04 LTS** : Version légère et optimisée pour ROS2, offrant une grande stabilité.

#### ROS2
- **ROS2 Humble** : Installation du système de base, incluant les outils de développement (colcon, rosdep).
- **rosdep** : Pour la gestion des dépendances.
- **colcon** : Pour compiler les packages ROS2.
- **Packages ROS2 spécifiques** :
  - `ros2_control` : Pour la gestion des servos et des actionneurs.
  - `image_transport` : Pour le streaming des flux vidéo de la webcam.
  - `tf2` : Pour la gestion des transformations spatiales (suivi de position).

#### Tinkerforge
- **Bibliothèques Tinkerforge** : Bibliothèques nécessaires pour interfacer le Master Brick et les Bricklets servos.
- **Brick Daemon** : Service de communication entre les briques Tinkerforge et l'ODROID N2.

#### Développement et Gestion des Nœuds ROS2
- **Python 3.10** : Langage utilisé pour la majorité des scripts ROS2.
- **C++ (optionnel)** : Pour les nœuds nécessitant une exécution plus rapide.
- **Outils de calibration des servos** : Scripts spécifiques pour calibrer et tester les servomoteurs.

#### IA et Vision
- **OpenCV** : Pour l'acquisition et le traitement d'images.
- **Facial Recognition Library (par exemple dlib ou face_recognition)** : Pour la reconnaissance des visages.
- **Modèle NLP (GPT)** : Pour l'interaction verbale.

### 2. Raspberry Pi 4 avec Écran Tactile

#### Système d'Exploitation
- **Ubuntu Server 22.04 LTS** : Version légère pour assurer des performances optimales.

#### Réseau et Connexion
- **Hostapd et Dnsmasq** : Pour transformer le Raspberry Pi 4 en point d'accès Wi-Fi et gérer le routage vers le réseau local.
- **Netplan** : Pour configurer l'interface réseau Ethernet et Wi-Fi.

#### Interface Web
- **Django** : Framework utilisé pour créer l'interface web permettant la gestion et la surveillance des différentes fonctionnalités du robot.
- **Nginx** : Serveur web pour héberger l'interface Django.
- **Gunicorn** : Serveur d'application WSGI pour exécuter le projet Django.

#### Développement et Gestion des Nœuds ROS2
- **ROS2 Humble** : Installation du système de base, incluant les outils de développement pour les nœuds ROS2, notamment `web_interface_node`.

#### Docker (Optionnel)
- **Docker et Docker Compose** : Pour conteneuriser certains services, comme l'interface web, afin de simplifier la gestion et les mises à jour.

### 3. Outils Communes aux Deux Plateformes

#### Gestion des Versions et Déploiement
- **Git** : Pour le contrôle de version des fichiers du projet.
- **VS Code (ou Vim)** : Éditeurs de code pour le développement sur les plateformes.
- **SSH** : Pour accéder à distance aux deux plateformes et les gérer.

#### Monitoring et Logs
- **Prometheus et Grafana (Optionnel)** : Pour la surveillance en temps réel des ressources système et la visualisation des performances du robot.
- **Rsyslog** : Pour centraliser les journaux de chaque composant et faciliter la maintenance.

### 4. Installation des Dépendances
Un script d'installation automatisé sera créé pour déployer la pile logicielle sur les deux plateformes. Il prendra en charge l'installation des dépendances, la configuration des services, et le lancement des nœuds ROS2. Cela garantira une mise en place rapide et homogène du système.

**Objectif**
L'objectif de cette pile logicielle est d'assurer une infrastructure robuste et flexible pour le robot humanoïde InMoov, permettant une communication fluide entre les différents composants, une gestion efficace des actionneurs, et une interaction avancée avec l'environnement.

