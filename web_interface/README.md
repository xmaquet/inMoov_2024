Le module web (`web_interface_node`), basé sur Django, communiquera avec le Master Brick 3.2 via des nœuds ROS2 et des services RESTful. Voici comment cette communication sera mise en œuvre :

### 1. Utilisation de ROS2 pour la Communication
- **Interface de Communication** : Le `web_interface_node` interagira avec les autres nœuds ROS2 (par exemple, le `motor_controller_node`) en utilisant les topics et services ROS2. Le `motor_controller_node`, qui est responsable du contrôle des servomoteurs, communique directement avec le Master Brick 3.2 via USB.
- **Topics et Services** : Le `web_interface_node` peut publier des messages sur des topics appropriés pour commander les moteurs (par exemple, demander un mouvement spécifique). Ces messages seront acheminés via le `motor_controller_node`, qui transmettra ensuite la commande au Master Brick.

### 2. API RESTful pour l'Interaction avec ROS2
- **API RESTful** : Une API RESTful sera exposée par le module web pour permettre des commandes à distance via l'interface web. Les utilisateurs pourront ainsi envoyer des commandes pour contrôler les mouvements du robot, ajouter de nouveaux gestes, ou ajuster les paramètres du système.
- **Bridge entre Django et ROS2** : Pour relier Django aux nœuds ROS2, un **bridge** sera mis en place pour permettre à Django d'envoyer des requêtes HTTP qui seront converties en commandes ROS2 via des scripts Python. Par exemple, lorsqu'un utilisateur clique sur un bouton de l'interface web pour faire bouger un bras, une requête est envoyée à un service Django, qui appelle ensuite une fonction Python communiquant avec ROS2.

### 3. Communication Indirecte avec le Master Brick
- Le Master Brick n'est pas directement accessible depuis le module web. Au lieu de cela, il est contrôlé par le `motor_controller_node`. Le rôle du module web est de servir d'interface utilisateur et de déclencher les commandes ROS2 appropriées.
- **Pipeline de Communication** :
  - **Utilisateur → Interface Web** : L'utilisateur effectue une action sur l'interface web (par exemple, déclencher un geste).
  - **Django → ROS2** : Django appelle un script Python qui publie une commande sur un topic ROS2 ou envoie une requête de service.
  - **ROS2 → Master Brick** : Le `motor_controller_node` reçoit la commande et communique avec le Master Brick 3.2 pour exécuter l'action.

Ce système de communication permet de gérer l'interaction avec le Master Brick de manière modulaire et distribuée, en gardant une séparation claire entre l'interface utilisateur (module web) et les composants bas niveau (contrôle des moteurs et gestion des servomoteurs).
