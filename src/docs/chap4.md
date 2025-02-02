# Chapitre 4: Prendre une photo avec une caméra et appliquer des filtres.

### Pour afficher la caméra :

Il est possible d'afficher la caméra dans la fenêtre.

!!! success
    ```python
	robot.afficher_camera(position_x, position_y)
	```

La caméra sera affichée aux coordonnées x et y passées en paramètres.

### Pour enregistrer une photo :

On peut capturer une photo de la caméra avec cette méthode:

!!! success
    ```python
	robot.prendre_photo(nom_fichier)
	```

La photo sera enregistré dans le dossier images au nom_fichier passé en paramètre.

### Pour afficher une image (ex. la photo enregistrée) :

On peut afficher une image avec la méthode:

!!! success
    ```python
	robot.afficher_image(chemin_fichier)
	```

Nous avons besoin du chemin et du nom du fichier (ex: /images/photo.jpg) passés en paramètres, ainsi que des coordonnées x et y ou seront affichées l'image.

### Pour appliquer un filtre sur une photo :

Nous pouvons appliquer un filtre à une image avec la méthode:

!!! success
    ```python
	robot.appliquer_filtre(chemin_fichier, nom_filtre)
	```

Les paramètres attendus sont le chemin et nom du fichier (ex: /images/photo.jpg) ainsi que le nom du filtre (ex: cartoon, alien, miroir...).

- [voir documentation pour la liste complète des filtres](/ref#les-filtres)

## Exemple 4 :

!!! warning
    Il est recommandé de ne pas copier l'exemple mais de chercher par vous-même des utilisations possibles.

```python
from pybot import Robot
robot = Robot()

long = 1200
haut = 1000

blanc = (255, 255, 255)
noir = (0, 0, 0)
rouge = (235, 64, 52)
rouge_sombre = (117, 23, 16)
bleu = (52, 164, 235)
bleu_sombre = (30, 93, 133)
vert = (105, 230, 83)
vert_sombre = (59, 135, 46)
jaune = (237, 212, 66)
jaune_sombre = (171, 128, 19)

bouton_camera = None
bouton_capture = None
bouton_photo = None
bouton_stop = None
bouton_filter_1 = None
bouton_filter_2 = None
bouton_filter_3 = None
bouton_filter_4 = None
bouton_filter_5 = None
bouton_filter_6 = None
bouton_filter_7 = None
bouton_filter_8 = None

afficher_camera = False
afficher_photo = False

mettre_a_jour_affichage = True

def preparer_programme():
    global bouton_camera, bouton_capture, bouton_photo, bouton_stop, \
    bouton_filter_1, bouton_filter_2, bouton_filter_3, bouton_filter_4, \
    bouton_filter_5, bouton_filter_6, bouton_filter_7, bouton_filter_8
    robot.creer_fenetre(long, haut)
    robot.changer_titre("Bonjour camera!")
    robot.couleur_fond(noir)

    robot.ajouter_evenement("echap", "stop")
    bouton_camera = robot.creer_bouton(180, 60, 10, 10, bleu)
    bouton_camera.ajouter_texte("camera - allumer", 5, 20)
    bouton_capture = robot.creer_bouton(180, 60, 10, 80, rouge)
    bouton_capture.ajouter_texte("capture photo", 10, 10, 20)
    bouton_photo = robot.creer_bouton(180, 60, 10, 150, jaune)
    bouton_photo.ajouter_texte("afficher photo", 10, 10, 16)
    bouton_filter_1 = robot.creer_bouton(200, 60, 15, 220, jaune_sombre)
    bouton_filter_1.ajouter_texte("filtre: ocean", 10, 10, 16)
    bouton_filter_2 = robot.creer_bouton(200, 60, 15, 290, jaune_sombre)
    bouton_filter_2.ajouter_texte("filtre: cartoon", 10, 10, 16)
    bouton_filter_3 = robot.creer_bouton(200, 60, 15, 360, jaune_sombre)
    bouton_filter_3.ajouter_texte("filtre: alien", 10, 10, 16)
    bouton_filter_4 = robot.creer_bouton(200, 60, 15, 430, jaune_sombre)
    bouton_filter_4.ajouter_texte("filtre: rose", 10, 10, 16)
    bouton_filter_5 = robot.creer_bouton(200, 60, 15, 500, jaune_sombre)
    bouton_filter_5.ajouter_texte("filtre: flou", 10, 10, 16)
    bouton_filter_6 = robot.creer_bouton(200, 60, 15, 570, jaune_sombre)
    bouton_filter_6.ajouter_texte("filtre: noir et blanc", 10, 10, 16)
    bouton_filter_7 = robot.creer_bouton(200, 60, 15, 640, jaune_sombre)
    bouton_filter_7.ajouter_texte("filtre: tourner", 10, 10, 16)
    bouton_filter_8 = robot.creer_bouton(200, 60, 15, 710, jaune_sombre)
    bouton_filter_8.ajouter_texte("filtre: vernis", 10, 10, 16)
    bouton_stop = robot.creer_bouton(180, 60, 10, 900, vert)
    bouton_stop.ajouter_texte("quitter", 10, 10, 20)

def dessiner_fenetre():
    global mettre_a_jour_affichage
    if mettre_a_jour_affichage:
        robot.afficher_fond()
        bouton_camera.afficher()
        bouton_photo.afficher()
        bouton_filter_1.afficher()
        bouton_filter_2.afficher()
        bouton_filter_3.afficher()
        bouton_filter_4.afficher()
        bouton_filter_5.afficher()
        bouton_filter_6.afficher()
        bouton_filter_7.afficher()
        bouton_filter_8.afficher()
        bouton_stop.afficher()
        if afficher_camera:
            bouton_capture.afficher()
        mettre_a_jour_affichage = False

def verifier_boutons():
    global mettre_a_jour_affichage, afficher_camera, afficher_photo
    if bouton_camera.est_actif():
        afficher_camera = not afficher_camera
        if afficher_camera:
            bouton_camera.ajouter_texte("camera - eteindre")
        else:
            bouton_camera.ajouter_texte("camera - alumer")
        if afficher_photo:
            bouton_photo.ajouter_texte("cacher photo")
        else:
            bouton_photo.ajouter_texte("afficher photo")
        mettre_a_jour_affichage = True
    if afficher_camera:
        if bouton_capture.est_actif():
            robot.prendre_photo("photo")
    if bouton_photo.est_actif():
        afficher_photo = not afficher_photo
        mettre_a_jour_affichage = True
    if bouton_stop.est_actif():
        robot.desactiver()
    if bouton_filter_1.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "ocean")
        mettre_a_jour_affichage = True
    if bouton_filter_2.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "cartoon")
        mettre_a_jour_affichage = True
    if bouton_filter_3.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "alien")
        mettre_a_jour_affichage = True
    if bouton_filter_4.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "rose")
        mettre_a_jour_affichage = True
    if bouton_filter_5.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "flou")
        mettre_a_jour_affichage = True
    if bouton_filter_6.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "noir_et_blanc")
        mettre_a_jour_affichage = True
    if bouton_filter_7.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "tourner")
        mettre_a_jour_affichage = True
    if bouton_filter_8.est_actif():
        robot.appliquer_filtre("/images/photo.jpg", "vernis")
        mettre_a_jour_affichage = True

def boucle_programme():
    while robot.est_actif():
        events = robot.verifier_evenements()
        if "stop" in events:
            robot.fermer_fenetre()
        dessiner_fenetre()
        if afficher_camera:
            robot.afficher_camera(300, 10)
        if afficher_photo:
            robot.afficher_image("/images/photo.jpg", 300, 500)
            robot.afficher_image("/images/fier.png", 1000, 500)
        verifier_boutons()
        robot.actualiser_affichage()

if __name__ == "__main__":
    preparer_programme()
    boucle_programme()
```
