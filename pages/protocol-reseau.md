# Protocole Réseau

```mermaid
sequenceDiagram
    autonumber
    participant Client
    participant Network
    participant Server

    Note over Client, Server: OSI Layer

    Client->>Client: Layer 7 Ankama Message Protocol
    Client->>Client: Layer 6 Protobuf Serialization
    Client->>Client: Layer 5 TCP Session Management
    Client->>Client: Layer 4 TCP Socket, Port Multiplexing<br>Port: Ephemeral Port
    Client->>Client: Layer 3 IP Dest: dofus2-co-beta.ankama-games.com<br>IP Src: Client IP
    Client->>Client: Layer 2 MAC Addr
    Client->>Client: Layer 1 Physical/Binary

    Client->>Network: Transmit

    Network->>Server: Receive
    Server->>Server: Layer 1 Physical/Binary
    Server->>Server: Layer 2 MAC Addr
    Server->>Server: Layer 3 IP Dest & Src
    Server->>Server: Layer 4 TCP Socket, Port Multiplexing<br>Port: 5555 or 443
    Server->>Server: Layer 5 TCP Session Management
    Server->>Server: Layer 6 Protobuf Serialization
    Server->>Server: Layer 7 Ankama Message Protocol
```

Ankama intègre son propre protocole Applicatif et utilise Protocol Buffer (Protobuf) pour la gestion des structures de données.

Le serveur Dofus Unity Beta est hebergé sur AWS: `dofus2-co-server-beta-client-23df9c5245171cb1.elb.eu-west-1.amazonaws.com`



## Fonctionnement de BepInEx <img src="https://github.com/user-attachments/assets/4e2039db-f281-407e-b6d2-e68998d61b4b" width=5% height=5%>

BepInEx est un framework d'injection de mod pour les jeux développés via Unity. Il permet de modifier et d'étendre les fonctionnalités des jeux en ajoutant des plugins personnalisés.

### Fonctionnalités Principales

#### Injection de Code :

BepInEx permet aux utilisateurs d'injecter du code dans les jeux Unity en utilisant des plugins. Ces plugins peuvent interagir avec le code du jeu, modifier le comportement du jeu, ajouter de nouvelles fonctionnalités, ou corriger des bugs.

<ins>Le framework est conçu spécifiquement pour les jeux développés avec Unity (qui utilisent Mono comme moteur de script).</ins>

#### Gestion des Plugins :
Les utilisateurs peuvent ajouter des plugins à un jeu en plaçant les fichiers DLL du plugin dans le répertoire BepInEx/plugins. Ces plugins peuvent contenir des scripts en C# qui seront exécutés lorsque le jeu se lance.

#### Support des Mods :

BepInEx facilite l'utilisation de mods et de plugins en fournissant des outils pour les développeurs de mods. Les développeurs peuvent écrire des plugins pour ajouter ou modifier des fonctionnalités dans les jeux sans avoir à modifier les fichiers du jeu directement.


## Gestion des structures de données: Protobuf

Qu'est-ce que Protobuf ?
Protobuf est un format de sérialisation de données qui permet de définir des structures de données et de les encoder/décoder en un format binaire. Développé par Google, il est utilisé pour la communication entre différents services, le stockage de données, et dans d'autres situations nécessitant une représentation structurée des données.

<ins>Le format binaire de Protobuf est plus compact que les formats de sérialisation textuels, ce qui réduit la taille des données et améliore les performances en termes de vitesse de traitement et de bande passante.</ins>

![image](https://github.com/user-attachments/assets/6b24b3e5-0696-4201-aedf-46b13b9c3802)

### Définition des Données :

Les structures de données sont définies dans des fichiers de définition `(.proto)` en utilisant une syntaxe simple et déclarative. Ces fichiers décrivent les types de données, les champs, et les relations entre les champs.

#### Exemple de fichier .proto :
```proto
syntax = "proto3";

message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;
}
```
### Sérialisation et Désérialisation :

* Sérialisation : Convertit des objets en un format binaire compact pour le stockage ou le transfert. Ce format est plus petit et plus rapide à traiter que les formats textuels comme JSON ou XML.
* Désérialisation : Reconvertit le format binaire en objets utilisables dans le code. Cela permet de lire et d'utiliser les données après les avoir reçues ou chargées.

Protobuf prend en charge plusieurs langages de programmation, y compris C++, Java, Python, C#, Go, et bien d'autres. Cela facilite l'échange de données entre systèmes développés dans différents langages.
