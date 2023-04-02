# TP6 : Programmation GPU en CUDA

### Objectifs du TP :
---------------------
* Comprendre et observer les différences entre la mémoire constante et la mémoire globale
* Comprendre comment utiliser la mémoire de texture pour les problèmes qui correspondent naturellement à des domaines 2D.

### Informations sur le TP :
----------------------------
* Le TP est à réaliser seul ou en binôme.
* A rendre sur ametice en spécifiant si vous avez fait le TP seul ou en binôme, en mentionnant votre prénom ainsi que votre nom (prénom et nom de chaque membre si en binôme).
* A rendre avant le 7 avril 2023, 24:00.

### Exercice 1 : Ray tracing et mémoire constante
-------------------------------------------------

Pour cet exercice, vous allez optimiser une application simple de ray tracing en passant de la mémoire globale à la mémoire constante.

Le ray tracing est une façon de produire une image 2D à partir d'une scène composée d'objets 3D.

Nous choisissons un endroit de notre scène pour y placer une caméra (un point dans notre scène 3D). Cette contient contient un "capteur de lumière". Pour produire une image, nous devons donc déterminer la lumière qui atteindra ce capteur. Chaque pixel de l'image résultante doit être de la même couleur et de la même intensité que le rayon de lumière qui frappe le capteur.

![ray tracing](ray.png)

Étant donné que la lumière incidente en tout point du capteur peut provenir de n'importe quel endroit de la scène, on va tricher et faire partir les rayon depuis notre caméra (moins complexe). Donc, plutôt que d'essayer de déterminer quel rayon lumineux frappe le pixel en question, imaginons que nous tirons un rayon depuis le pixel vers l'intérieur de la scène. De cette manière, chaque pixel se comporte un peu comme un œil qui "regarde" la scène.

![eye](eye.png)

Pour produire des rendus de haute résolution et plus complexe, les rayons intersectant un objet 3D sont renvoyés en direction de chaque source de lumière pour déterminer sa luminosité (l'objet est-il éclairé ou à l'ombre d'autres objets ?). Cette luminosité, combinée avec les propriétés de la surface de l'objet (sa couleur, sa rugosité, etc.), ainsi que d'autres informations éventuelles (angles entre la normale à l'objet et les sources de lumières, réflexions, transparence, etc...), déterminent la couleur finale du pixel.

Dans le code, les sphère sont modélisé avec une structure de données qui stocke les coordonnées du centre de la sphère (x, y, z), son rayon et sa couleur (r, b, g). la structure possède une fonction appelée sphere_intersect( float ox, float oy, float *n ). Compte tenu d'un rayon tiré à partir du pixel situé à (ox, oy), cette fonction calcule si le rayon coupe la sphère. Si le rayon coupe effectivement la sphère, la fonction calcule la distance de la caméra à laquelle le rayon touche la sphère. Si le rayon touche plus d'une sphère, seule la sphère la plus proche est visible.

Prenez du temps pour comprendre ce que fait le code.

Après avoir terminé les étapes permettant d'utiliser la mémoire constante, modifiez les valeurs de MAX_SPHERE et IMAGE_DIM afin de completer le tableau de temps suivant. Que constatez-vous ?

![tab](tableau_temps.png)

### Exercice 2 : Flou d'image et mémoire de texture
---------------------------------------------------

Dans cet exercice, vous allez expérimenter l'utilisation de la mémoire de texture. Dans cet exemple, nous allons utiliser explicitement la mémoire de texture plutôt que d'utiliser des qualificateurs pour forcer les chargements de mémoire via le cache en lecture seule. Il y a de bonnes raisons de le faire lorsqu'il s'agit de problèmes liés aux images ou de problèmes qui se décomposent naturellement en 2 dimensions. 

Vous allez implémenter un flou d'image. Jetez un œil au code du kernel pour essayer de le comprendre.
Sur guthub vous avez une image input.ppm. Afin de faire fonctionner le code, il faut dans un premier téléversé l'image input.ppm sur google colab.

![upload](upload.png)

Bon TP !
