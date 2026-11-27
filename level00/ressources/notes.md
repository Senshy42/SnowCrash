pwd = level00. 
ls -a = on trouve plusieurs fichiers cachés :
- bashrc : fichier qui configure bash dans son aspect visuel et interactif, s'execute à chaque nouveau terminal (alias, fonctions courtes, historique...).
- bash_logout : script qui détermine le comportement de bash à la fermeture du terminal ?
- profile : s'execute en une seule fois au lancement de Linux, configure les variables d'environnement et les programmes lancés au démarrage.

Je comprends qu'à la croisée de ces fichiers se trouve la solution mais à première vue, rien ne me saute aux yeux.
Je cherche dans env, rien à signaler.

Dans les fichiers de config, on m'invite à prendre exemple sur un fichier mais en respectant le path je ne trouve pas le fichier en question. D'après l'IA, c'est normal, 
je trouve des fichiers bashrc, bash_logout et profile dans etc/skel/, je les compare à ceux présents dans level00 avec la commande diff = aucune différence trouvée. 
Je n'en suis pas certaine à ce stade, mais je pense comprendre que les fichiers de configuration sont correctement écrits.

Retour à la case départ.
Je fais une recherche du fichier en question, au cas où, mais après recherches, j'ai la confirmation que ce n'est qu'une config par défaut donc j'abandonne cette piste.

Mon collègue me demande si on ne doit pas ajouter un path, celui indiqué dans .profile. A mon sens c'est déjà exécuté puisque le script se lance au démarrage du terminal, mais en relisant bien, 
je réalise que la variable PATH semble incomplète et que le script n'a pas l'air d'avoir été exécuté. J'approfondis mais à perte.
J'abandonne complètement ces fichiers, pour moi ils sont standards et conformes, je vais chercher ailleurs.

Dans le dossier ssh, je trouve les fichiers known_host et known_hosts.old, je vérifie que l'ancien n'appartient pas à un autre utilisateur mais rien à signaler.

Après renseignements, je vois que je m'y prends mal, je cherche une aiguille alors que je peux procéder par entonnoir. 
Je commence par chercher tous les dossiers et les fichiers qui appartiennent à mon utilisateur actuel, soit level00 puis flag00 auquel je n'ai pas encore accès mais qui est lié à mon niveau. 
J'utilise "find / -user level00 2>/dev/null.

Level00 m'indique une liste interminable, mais flagg00, beaucoup moins. Je trouve un dossier snowcrash, avec un seul dossier : level00. Je sens que je chauffe ?
Un seul fichier, un exécutable, que j'exécute... Et ça fonctionne. J'entre dans un univers minimal, dans lequel je suis flag00 et plus level00. 
L'environnement est capitonné, cd n'existe pas, ls affiche l'équivalent de ls -la, je sens que les permissions sont très restreintes.

Deux fichiers, mais je n'ai pas les permissions. Je ne sais pas si je suis entrée dans le fichier trop tôt et s'il me manque quelque chose à faire avant.

La session de flag00 a un timeout, au bout d'un certain moment j'en suis exclue. Je fais strings <executable> | grep timeout pour en savoir plus, et par la même occasion je grep d'autres mot-clés. 
Ainsi je découvre que le résultat de mon ls dans le terminal est hardcodé puisqu'il est écrit en clair dans le fichier.
Cette fois, je suis quasi certaine de tenir la bonne piste, je vais analyser l'exécutable.

Je trouve des fichiers mais il n'existent pas dans le système :
var/log/snowcrash/hiro.log 
etc/snowcrash/hiro.conf

1h plus tard, je me retrouve à lire de l'assembleur pour comprendre le binaire, je cherche les strcmp et tente de déchiffrer de l'héxa. Je suis à peu près sûre que c'est pas ce qu'on attend de moi, 
je suis en train de réaliser que j'ai l'air de hacker le projet au lieu de chercher au bon endroit donc je me calme et reprend mes esprits.

Il est 5h12, cette histoire a commencé vers minuit et ce qui devait être mes notes commence à ressembler à un journal de bord. 

cd /opt/snowcrash/level00 -> hiro

Les seules commandes qui fonctionnent dans flag00 sont whoami et id, mais en faisant une recherche strings, ils sont également hardcodés.
Néanmoins, je trouve aussi des fonctions appartenant à la GLIBC, dont geteuid, j'imagine donc que je dois pouvoir trouver le vrai id avec une autre commande que id ?
Rien de concluant, level00 n'a aucun processus particulier, j'abandonne la piste, retourne à analyser les strings du binaire. 

Pendant ce temps, mon collègue porte son attention sur une section flag_data. 
Avec la commande objdump -s -j (filtre pour accéder à la ligne de flag_data, afficher son adresse mémoire, ses octets et traduction ASCII), on tombe sur une suite de caractères qui après vérification n'est pas encodée.

On teste, le mot de passe fonctionne. 

Niveau terminé. 
