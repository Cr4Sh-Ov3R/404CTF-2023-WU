****
**Catégorie**: WEB - **Difficulté**: Medium 
# Fuite en 1791

****

***Synopsis*** :
  > Alors qu'une douce odeur de café commençait à emplir Le Procope, une femme vient interrompre le calme de la chaleureuse pièce. Tout droit sortie du XVIIIème siècle, Olympe de Gouge fait irruption et se précipite vers votre table.

  > « Vous ! s’écria-t-elle

  > — Moi ? répondez-vous.

  > — Oui vous, allez porter cette lettre de toute urgence à Anne-Catherine Helvétius.

  > — Qui ? De quoi s'agit-il ?

  > — Je ne peux vous expliquer maintenant, mais le contenu de cette lettre peut changer le cours de l'histoire, ne perdez pas de temps. »

  > Et avant même que vous puissiez la questionner d'avantage, elle passa la porte et sortit du café.

  > Dans l'incompréhension la plus totale, vous commencez à parcourir ladite lettre…


<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 Chall" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/chall.png">
</p>


****

## Home Page 

Lors de l'arrivée sur la page, on se retrouve avec un texte pour *Anne-Catherine Helvétius* qui nous informe que la *Déclaration des Droits de la Femme et de la Citoyenne* est disponible **sans mot de passe** mais le lien n'a qu'une durée de validité d'une semaine. 

<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 - Home" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/home.png">
</p>


****

## Expiry Link

Une fois cliqué sur le lien, sans suprise, le lien est expiré, rien de surprenant vu que la scène est censé se passer en 1791.

<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 - Expiry Link" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/expiryLink.png">
</p>


> url : https://ddfc.challenges.404ctf.fr/ddfc?expiry=1688245200&signature=wawF6dC4Hz9g5NyCc3j1KCDcfztFE/sp

On retrouve dans le lien un paramètre expiry qui a le timestamp -5625891076 qui correspond au 21-09-1791 12:38:05. 

J'essaie donc de le passer en au 01-07-2023 23:00:00 ce qui nous donne le timestamp 1688245200

Raté, j'ai maintenant l'erreur ***Invalid signature***. Le paramètre signature vérifie donc que le timestamp n'ait pas été modifié.

<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 - Invalid Signature" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/invalidSignature.png">
</p>

Le problème vient de la signature, alors j'essaie de l'enlever, mais je me retrouve avec l'erreur ***Missing signature or expiry***, pareil après avoir supprimé le timestamp.

<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 - Missing parameter" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/missingParams.png">
</p>

Avant de tenter de m'attaquer à la signature qui tiens probablement d'une clef secrète, je tente de chercher s'il existe des moyens de modifier des paramètres url en outrepassant les expiry / signature. 

Je tombe sur des publications parlant de "***HTTP Parameter Pollution***" (ou HPP) et décide de tenter ma chance là dessus. 

Je tente donc la duplication du paramètre expiry en le mettant juste après le premier avec un timestamp au 01-07-2023 23:00:00 comme tout à lheure

> /ddfc?expiry=-5625891076?expiry=1688245200&signature=wawF6dC4Hz9g5NyCc3j1KCDcfztFE/sp

> /ddfc?expiry=-5625891076&expiry=1688245200&signature=wawF6dC4Hz9g5NyCc3j1KCDcfztFE/sp

Même erreur ***Invalid signature***

Je tente désormais de mettre le expiry contenant le timestamp à juillet après la vérification signature, à noter après la paire expiry/signature 

> /ddfc?expiry=-5625891076&signature=wawF6dC4Hz9g5NyCc3j1KCDcfztFE/sp&expiry=1688245200

Je suis passé

****

## FLAG

Je tombe enfin sur la *Déclaration des Droits de la Femme et de la Citoyenne* et peut la parcourir.

<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 - Déclaration des Droits de la Femme et de la Citoyenne" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/declaration.png">
</p>

C'est en arrivant au bas de la page je découvre enfin le flag.

<p align="center">
<img alt="Screen Chall Web - Medium - Fuite en 1791 - Flag Flag" src="https://github.com/Cr4Sh-Ov3R/404CTF-2023-WU/blob/main/assets/fuite_en_1791/flag.png">
</p>

<details>
<summary>Voir le flag :</summary>

FLAG : ***404CTF{l4_p011uti0n_c_3st_m41}***
</details>


****
**404CTF 2023** - *Cr4Sh*
****