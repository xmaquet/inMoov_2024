**Projet InMoov_wROS2 : Architecture logicielle et matérielle**

Le projet InMoov_wROS2 est un développement collaboratif, initié avec l'aide de ChatGPT, visant à créer une version personnalisée du robot humanoïde InMoov. Ce projet utilise ROS2 (Robot Operating System 2) pour gérer les différents composants du robot et a pour but de développer une plateforme évolutive, interactive, et capable de comprendre son environnement en temps réel.

**Configuration Matérielle**

1. **ODROID N2 4G** : L'ODROID N2 est utilisé pour gérer les actionneurs du robot. Il est associé à un Brick Tinkerforge et deux Bricklets servos qui assurent le contrôle des moteurs des membres du robot (bras, mains, tête). Cela permet un contrôle fin et fluide des mouvements du robot. Les ESP32 et Arduino, précédemment envisagés, sont abandonnés pour la gestion des servos, mais restent des options pour d'autres fonctionnalités, telles que des jeux de lumières ou des capteurs PIR.

2. **Raspberry Pi 4 avec écran tactile** : Un Raspberry Pi 4 est couplé à un écran tactile et assure la connexion à un réseau Wi-Fi extérieur en jouant le rôle de routeur. Le Raspberry Pi 4 et l'ODROID N2 sont connectés en réseau local Ethernet, facilitant la communication entre les deux systèmes.

**Fonctionnalités du Robot**

- **Contrôle des Actionneurs** : Le contrôle des servos est assuré par l'ODROID N2, avec un Master Brick Tinkerforge et deux Bricklets servos pour la gestion des membres (bras, mains, tête). Le nœud ROS2 correspondant, nommé `motor_controller_node`, permet d’envoyer les commandes de positionnement des articulations, ainsi que de suivre l’état des servos. Ce module gère également la calibration des servos et la mise en œuvre des séquences de mouvements prédéfinis.

- **Vision et Reconnaissance Faciale** : Le robot est équipé d'une webcam connectée à l'ODROID N2, permettant la vision en temps réel. Un module ROS2 spécifique, nommé `vision_node`, est chargé de l'acquisition d'images, de la détection et de la reconnaissance faciale. Ce nœud envoie des informations de suivi des visages et des objets à d'autres nœuds ROS2, permettant au robot d'interagir dynamiquement avec son environnement.

- **Interaction Verbale** : Un module d'intelligence artificielle est intégré pour la gestion du dialogue avec les humains. Ce module ROS2, nommé `ai_node`, s'appuie sur un modèle NLP (comme GPT) pour comprendre et générer des réponses, adaptant son comportement en fonction des situations. Ce nœud coordonne également les interactions avec d'autres nœuds, comme le `gesture_control_node`, pour rendre les échanges plus immersifs.

- **Contrôle Gestuel** : Le robot possède une gestuelle prédéfinie, avec la possibilité de créer de nouveaux gestes via une interface web. Le nœud ROS2 correspondant, nommé `gesture_control_node`, est responsable de l'animation des mouvements complexes des bras, des mains, et de la tête. Il intègre une bibliothèque de gestes prédéfinis (salutations, pointages, hochements de tête, etc.) et peut également générer des gestes contextuels en fonction des informations fournies par le `vision_node` et le `ai_node`.

- **Interface Web** : Une interface web, développée avec Django, est déployée sur le Raspberry Pi 4. Cette interface est gérée par un nœud ROS2 nommé `web_interface_node`. Elle permet la surveillance et le contrôle à distance du robot, y compris la création de nouveaux gestes et la gestion des paramètres des différents modules. L'interface fournit également une visualisation en temps réel des données du robot, comme le flux vidéo et les positions des moteurs.

**Communication et Synchronisation**

La communication entre les nœuds ROS2 (`motor_controller_node`, `vision_node`, `ai_node`, `gesture_control_node`, `web_interface_node`) est gérée par des topics, services, et actions ROS2. Par exemple, le `vision_node` transmet les détections de visages au `ai_node`, qui peut alors déclencher des interactions verbales et gestuelles appropriées. La synchronisation entre les modules permet de coordonner les gestes et le dialogue du robot en réponse aux événements détectés en temps réel.

**Système d’Exploitation**

Le système tourne sur Ubuntu Server 22.04 LTS, optimisé pour ROS2. Docker est utilisé pour conteneuriser certains services, simplifiant le déploiement, la maintenance, et la mise à jour des différents composants du système.

**Avantages et Inconvénients de Docker**

- **Avantages** :
  - **Isolation des Services** : Docker permet d'isoler chaque service (comme l'IA, la vision, ou l'interface web) dans des conteneurs distincts, évitant ainsi les conflits de dépendances et améliorant la stabilité globale du système.
  - **Facilité de Déploiement** : Grâce à Docker, chaque composant peut être déployé de manière indépendante, rendant le système plus modulable et simplifiant la mise à jour des différentes parties sans perturber l'ensemble.
  - **Portabilité** : Les conteneurs Docker peuvent être facilement déployés sur d'autres machines, facilitant le partage du projet et sa reproductibilité pour d'autres développeurs ou utilisateurs.
  - **Gestion des Versions** : Docker permet de garder différentes versions des conteneurs, rendant possible le retour à une version précédente en cas de problème avec une mise à jour.

- **Inconvénients** :
  - **Consommation de Ressources** : Bien que Docker soit plus léger que les machines virtuelles, l'exécution de plusieurs conteneurs peut tout de même consommer une quantité importante de ressources, ce qui peut être critique sur un appareil comme le Raspberry Pi.
  - **Complexité Supplémentaire** : L'ajout de Docker ajoute une couche supplémentaire de complexité, nécessitant une certaine expertise pour la gestion des conteneurs, la configuration réseau, et la sécurité.
  - **Accès Matériel** : L'accès direct à certains périphériques matériels peut être plus compliqué depuis un conteneur Docker, nécessitant des configurations spécifiques pour permettre l'accès aux périphériques USB ou aux caméras, par exemple.

**Objectif**

L'objectif de ce projet est de créer une plateforme évolutive, capable d'interagir avec son environnement en temps réel, d'apprendre des échanges avec les humains et de réaliser des tâches variées, qu'il s'agisse de gestuelle, d'expressions verbales, ou de réactions aux stimuli de l'environnement. Le projet est conçu pour être accessible et modulaire, permettant des évolutions futures tant au niveau logiciel que matériel.

