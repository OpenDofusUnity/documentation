# Injection Rich TextMeshPro dans le chat

La librairie [TextMeshPro](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/manual/index.html) est utilisé pour la gestion de texte utilisé par Dofus dans le jeu. Elle a le gros avantage de permettre du Rich Text, c'est-à-dire pouvoir utiliser des couleurs, de la mise en forme de texte, des sprites dans le texte, ce qui est utilisé dans le cadre d'un mmorpg pour les messages de combat (par exemple affichage de l'élément des attaques via une icone).
Ils ont le désavantage de nécessiter des protections car les balises utilisées peuvent aussi être injecté par l'utilisateur si aucune protection n'est mise en place de la même manière qu'une injection XSS dans une page web.

On peut retrouver toutes les balises utilisables dans les TextMeshPro dans [la documentation officielle de Unity](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/manual/RichText.html)

## Problèmes posés par cette vulnérabilités
Ces injections posent principalement un problème de phishing, mais aussi de lisibilité du chat.

Grâce à ces injections, il est possible de se faire passer par un autre utilisateur, écrire des messages ressemblant à des messages systèmes, mettre des liens sans passer par la popup anti-phishing du client, etc.

Tant que ces bypass existent, faites très important a n'importe quel lien dans le chat (sorts, item, utilisateur, guild, etc.) qui peuvent être des liens malveillant (menant à des sites web de phishing ou de contenu choquant par exemple).

Le second problème est la lisibilité du chat, car il est possible de rendre le chat inutilisable grâce à certaines balises. Certaines balises non partagées dans ce post ont aussi permis de faire buggé l'interface entière du jeu en menant parfois jusqu'au crash du client.

## Bypass trouvés
### Premier bypass (16/08/2024)
Lors de l'arrivée du chat fonctionnel en jeu, un premier bypass était possible en utilisant n'importe quel balise dans le chat :
```html
<mark color="#6897BB">x</mark><mark color="#F2F3F3">x</mark><mark color="#7C0002">x</mark>
```

![image](https://github.com/user-attachments/assets/f070f8de-f216-4048-968c-40582e77ef1f)

### Deuxième byppass (16/08/2024-17/08/2024)
Suite au premier bypass commençant à être abusé dans le chat, un patch a rapidement été publié le soir même.

Ce patch rajoute deux balises, `<noparse>` au début du message et `</noparse>` à la fin. Ces balises sont censés empêcher l'interprétation de balises à l'intérieur mais n'empêche pas les injections.
Il a été rapide de bypass ce patch en utilisant une balise `</noparse>` prématurée.

```html
</noparse><mark color="#6897BB">x</mark><mark color="#F2F3F3">x</mark><mark color="#7C0002">x</mark>
```

![image](https://github.com/user-attachments/assets/f070f8de-f216-4048-968c-40582e77ef1f)

### Troisième bypass (17/08/2024-18/08/2024)
Suite au deuxième bypass commençant à être abusé dans le chat, un patch a été publié.

Ce patch à rajouté principalement deux principes : 
Interdiction de l'envoi d'un message avec la chaine `</` et remplacement de la chaine `<noparse>` par du vide.

Grâce à ce remplacement, on a pu mettre en place un nouveau bypass : 
```html
<<noparse>/noparse><mark color="#6897BB">x</mark><mark color="#F2F3F3">x</mark><mark color="#7C0002">x</mark>
```

![image](https://github.com/user-attachments/assets/f070f8de-f216-4048-968c-40582e77ef1f)

Dans `<<noparse>/noparse>` la chaine `</` n'est pas trouvée donc le message peut être envoyé et `<noparse>` est remplacé par du vide, ce qui fait qu'on se retrouve au final avec une chaine `</noparse>` qui revient à notre deuxième exploit qui avait été patché.

### Quatrième bypass (18/08/2024-Now)
Suite à ce troisième bypass, un troisième patch a été mis en place le 18/08/2024 qui interdit cette fois entièrement les chaînes `<noparse>` et `</noparse>`.

Un autre bypass a été trouvé en rajoutant un simple espace : 
```html
</noparse ><mark color="#6897BB">x</mark><mark color="#F2F3F3">x</mark><mark color="#7C0002">x</mark>
```

![image](https://github.com/user-attachments/assets/f070f8de-f216-4048-968c-40582e77ef1f)

Un patch aurait été mis en place le 20/08/2024 mais n'a pas rendu inopérant le précédent bypass.
