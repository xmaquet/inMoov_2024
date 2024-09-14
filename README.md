# inMoov_wROS2
Ce projet a été pensé comme un développement collaboratif avec une ... IA

J'ai travaillé dès le départ avec chatGPT 4.o, dans le but de gagner du temps et de produire un système de qualité.

InMoov est un projet de robot humanoïde open-source ; mon projet est de le contrôler via ROS2 sur un Odroid N2. Ce robot intègre des fonctionnalités avancées telles que la vision par webcam avec reconnaissance faciale, un module d'IA pour l'interaction verbale, et un système de contrôle gestuel. Les actionneurs sont gérés par deux microcontrôleurs (Arduino Mega ou ESP32) pour assurer des mouvements fluides. Une interface web développée avec Django permet de surveiller et de contrôler le robot à distance. L'objectif est de créer une plateforme évolutive et interactive, capable de comprendre et de réagir à son environnement en temps réel.

**Génèse :** 

*Tu vas m'aider à définir (dans un premier temps) une architecture logicielle qui permettra de mettre en œuvre une version personnelle du robot humanoïde InMoov. Je souhaite à terme l'installer sur un odroid N2 et utiliser ROS2 ; il faudra prévoir un module d'IA pour créer un personnage qui échangera avec le monde environnant ; il faudra gérer la vision avec une webcam et prévoir la reconnaissance faciale ; il y aura un module web pour la gestion des services. Commençons et affinons progressivement une version.*

Mots-clés : architecture logicielle, InMoov, Odroïd N2, ROS2, IA, vision, webcam, module web

Pour concevoir une architecture logicielle pour un robot humanoïde InMoov personnalisé, en utilisant un Odroid N2 et ROS2, nous devons prendre en compte plusieurs aspects clés : le matériel, les capteurs, les moteurs, les interfaces logicielles, et les modules d'intelligence artificielle. Voici une première ébauche de l'architecture logicielle que nous pouvons affiner au fur et à mesure.

