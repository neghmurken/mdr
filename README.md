# MDR #

Messages Décentralisées en Réseau

MDR est un protocole de messagerie décentralisée très simple.

Les exemples suivants montrent une situation où un participant (Alice) tente de rejoindre la conversation de Bob, Carol et David.
Alice ne connait pour l'instant que l'adresse IP de Bob (128.0.0.1)

## Connexion ##

Alice se connecte à 128.0.0.1 (avec son nom) et lui demande son identité (**INIT**)

 - Alice (à 128.0.0.1) : `KIKOO "Alice" ASV`

128.0.0.1 accepte, renvoie son nom et la liste des autres participants (s'il y en a) (**IDENT**)

 - 128.0.0.1 (à Alice) : `OKLM "Bob" / 128.0.0.2 / 128.0.0.3`

Une connexion entre Alice et Bob est maintenant effective.

128.0.0.1 peut refuser la connexion et la communication s'arrête ici (**REJECT**)

 - 128.0.0.1 (à Alice) : `TROPA`

Alice se connecte à chaque autre participant si ce n'est pas déjà le cas.

 - Alice (à 128.0.0.2) : `KIKOO "Alice" ASV`
 - Alice (à 128.0.0.3) : `KIKOO "Alice" ASV`
 - 128.0.0.2 (à Alice) : `OKLM "Carol" / 128.0.0.1 / 128.0.0.3`
 - 128.0.0.3 (à Alice) : `OKLM "David" / 128.0.0.1 / 128.0.0.2`

Alice connait maintenant la liste de tous les participants (Bob, Carol et David)

## Messages ##

Alice envoie un message à chacun des autres participants (**MSG**). Le protocole étant décentralisé, il faut répliquer un même
message à chaque personne.
Chaque message doit être ensuite acknowledge par un `LOL` par chaque destinataure (**ACK**)
L'absence d'acknowlegdement pendant un laps de temps défini (TIMEOUT) clôt la connexion.

 - Alice (à Bob) : `TAVU "Bonjour messieurs dames, comment vous portez-vous ?"`
 - Bob (à Alice): `LOL`
 - Alice (à Carol) : `TAVU "Bonjour messieurs dames, comment vous portez-vous ?"`
 - Carol (à Alice) : `LOL`
 - Alice (à David) : `TAVU "Bonjour messieurs dames, comment vous portez-vous ?"`
 - David (à Alice) : `LOL`


## Rafraichissement ##

Alice peut redemander l'identité d'un participant avec **ASK**. Le participant répond avec **IDENT**

## Déconnexion ##

Alice se déconnecte (**QUIT**)

 - Alice (à Bob) : `JPP`
 - Alice (à Carol) : `JPP`
 - Alice (à David) : `JPP`

L'acknowledgement ici n'est pas obligatoire.

## Gestion des erreurs ##

Si un participant :

 - envoie un message invalide ou non connu,
 - tente d'envoyer autre chose que **INIT** alors que la connexion n'est pas établie

le destinaire renvoie alors `WTF "<explication>"` (**ERR**)

 - Alice (à Bob) : `AZY j'fais n'imp !!!!1!!one!`
 - Bob (à Alice) : `WTF "AZY non reconnu"`

## Référence ##

 - **INIT** : `KIKOO <space> <name> <space> <ASK>`
 - **ASK** : `ASV`
 - **IDENT** : `OKLM <space> <name> (<space> <slash> <space> <ip>)*`
 - **REJECT** : `TROPA`
 - **MSG** : `TAVU <space> <string>`
 - **ACK** : `LOL`
 - **QUIT** : `JPP`
 - **ERR** : `WTF <space> <string>`
 - **space** : `[ ]+`
 - **slash** : `/`
 - **name** : `<string>`
 - **string** : `" (<unicode-char>|"")* "`
 - **unicode-char** : n'importe quel caractère UNICODE
 - **ip** : `<ip-part> <dot> <ip-part> <dot> <ip-part> <dot> <ip-part>`
 - **dot** : `.`
 - **ip-part** : un entier de 0 à 127
