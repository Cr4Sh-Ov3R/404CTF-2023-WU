****
**Catégorie**: WEB - **Difficulté**: Intro 
# Le loup et le renard

****

***Synopsis*** :
  > Dans un coin du café, un homme est assis. Une tasse de café ainsi qu'un manuscrit sont posés devant lui. Il observe la salle : les allées-venues des clients, les conversations. Il semble à l'affût de la moindre action, du moindre écart de la part de son sujet d'observation.

  > Alors que son regard parcourt la salle, il s'étonne de voir que vous l'observiez déjà. D'un geste accompagné d'un sourire il vous invite à le rejoindre.
  
  > « Bienvenue ! Prenez place. Il est rare de voir quelqu'un d'attentif à autre chose que sa propre personne ici. C'est dommage, c'est justement ce qu'il y a de plus intéréssant dans ce genre de rassemblement : les autres. Je me présente : Jean de La Fontaine. Votre regard me plait, vous me semblez capable de m'aider sur mon prochain manuscrit. J'écris, voyez-vous ? Des fables, je m'inspire de ce que je vois et j'observe. Pouvez-vous m'aider à écrire la suite de celle-ci ? »

![Screen Chall Web - Intro - Le loup et le renard](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/chall.png)

****
Arrivé sur le site, on trouve un bouton qui nous invite à démarrer l'aventure.

## *Partie 1 - Authentification* :

![Screen Partie 1](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part1.png)

  Une fois cliqué sur le bouton démarrer, nous nous retrouvons sur la partie 1, laquelle contient un formulaire demandant les champs *Nom d'utilisateur / mot de passe*.

  Avant de lancer quoi que ce soit, je décide d'éxaminer le code à la recherche d'indices potentiels *(commentaires, scripts ...)*.   

  En fin de code source, avant la balise fermante </body>, on peut découvrir un script ayant pour fonction de vérifier la validité des champs *username:**admin*** et *password:**h5cf8gf2s5g7d*** ainsi que le liens vers lequel nous serons redirigés */fable/partie-2-cookie* 
![Screen code source de la page](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part1-source.png)


## *Partie 2 - Cookie* :
![Screen Partie 2](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part2.png)
  Une fois la paire username/password renseigné, nous sommes redirigés sur la partie 2.

  Cette fois-ci aucun indice dans le code, mais le titre nous suggère un indice, COOKIE. 

  En parcourant les cookies, nous trouvons un cookie *isAdmin* valant *false*. 
  
  ![Screen Partie 2 cookie false](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part2-cookie.png)
  Je décide donc d'essayer de monter en privilège grâce à la valeur *true* et clique sur le bouton valider du formulaire, en laissant les champs vides.
  ![Screen Partie 2 cookie true](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part2-cookieTrue.png)

## *Partie 3 - Redirect* :
![Screen Partie 3](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part3.png)
  Le clic sur le bouton valider m'a maintenant redirigé vers la partie 3 qui contient à nouveau un formulaire.

  Je reprends donc en premier lieu le code afin d'y trouver une piste potentielle.

  Dans le formulaire je trouve que l'action liée au formulaire qui a une méthode *GET* et redirige vers le path */fable/partie-4-flag-final*. Il ne semble pas y avoir de validation Front, je teste donc un simple clic sur valider, sans valeurs de champs.
  ![Screen Partie 3 code source](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part3-source.png)

  Malheureusement, je vois bien la partie 4 s'afficher, mais une redirection rapide me renvoie vers la partie 3. 

  Avant de sortie l'artillerie, je décide d'outrepasser ce comportement en bloquant les scripts dans mon navigateur.

## *Partie 4 - Flag* :
![Screen Partie 4 ET FLAG](https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/le-loup-renard/part4.png)
  Bingo, le script de redirection m'empêchant d'accéder à la page est bloqué par le navigateur, la partie 4 s'affiche donc et révèle le flag.

  
Flag : *404CTF{N0_frOn1_3nD_auTh3nt1ficAti0n}*


****
**404CTF 2023** - *Cr4Sh*
****
