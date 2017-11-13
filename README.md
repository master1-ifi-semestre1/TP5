# TP5
## Introduction
Vous le savez peut-être, un programme Java est composé de classes qui sont chargées dynamiquement lorsqu'elles sont nécessaires. Certaines classes proviennent de votre disque local, d'autres proviennent peut-être du réseau (applet, etc...), certaines sont chargées automatiquement, d'autres peuvent l'être "à la demande".
Attention, les questions sur le chargement des classes sont à réaliser :
soit hors d'Eclipse (ligne de commandes)
soit dans Eclipse mais en désactivant le "build automatique"
pour mieux maitriser la compilation des classes et le déplacement des fichiers .class nécessaires aux expérimentations.


## Les classes ne sont pas toutes chargées en même temps : vérifions !
Avant tout, testons quelques exemples qui prouvent que toutes les classes utilisées par une appli Java ne sont pas chargées en mémoire avant que l'appli ne démarre.
• Récupèrez donc les exemples de chargement dans cette archive.
• Etudiez les classes A.java, B.java, C.java, D.java. Elles comportent toutes un bloc static qui affiche un message dès que la classe est chargée en mémoire. Compilez toutes les classes. Exécutez Exemple1.
• Observez bien les chargements de chaque classe, et ce qui provoque ces chargements.
• Remarquez (ClasseB n'est jamais chargée) que déclarer une variable d'un certain type ne suffit pas pour déclencher le chargement de ce type (classe ou interface). Les classes ne sont chargées qu'au moment où on en a vraiment besoin (chargement de ClasseD après les 5 secondes d'attentes). On appelle ça le "lazy loading" (chargement paresseux).
• Remarque : pour des classes quelconques (sans bloc static comme celui de des classes A, B, C et D) on peut s'aider de l'option -verbose de la commande java pour observer les classes qui sont chargées.


## Utiliser le chargement dynamique pour faire des plugins
En Java il est facile de charger une classe pendant l'exécution alors qu'on ne connait pas son nom quand on écrit le code. Ce nom peut être donné par l'utilisateur ou, le plus souvent, fourni par le contexte d'exécution. On peut ainsi parcourir un répertoire du disque local et charger toutes les classes du répertoire (celles qui correspondent à un fichier se terminant par ".class"). C'est le principe des plugins : on dépose des classes dans un répertoire et l'application les découvre et les charge au début de son exécution. Ces classes ajouteront des fonctionnalités à l'application.

###### 1. Étudiez la classe Exemple2. Étudiez en particulier les méthodes de la classe Class utilisée pour le chargement dynamique des classes (forName) et pour créer des instances (newInstance). Exécutez Exemple2.
###### 2. Exemple3 introduit la notion de plug.plugin ; elle utilise l'interface Plugin et la classe Plugin1 (un plug.plugin). Étudiez comment on peut faire appel à une méthode d'un plug.plugin sans savoir au moment où on écrit le code le nom de la classe. Remarquez les exceptions qui sont attrapées. Le plus simple est d'utiliser une interface que tous les plugins devront implémenter. En passant, vous pouvez réviser vos fondamentaux : dans Exemple3, à quoi sert le cast de l'objet o en Plugin (p = (Plugin)o;) ? Ne pourrait-on écrire directement c.newInstance().decrisToi(); ?
###### 3. Mettez Plugin1 dans le paquetage fr.unice.plugin et refaites exécuter Exemple3. Pour que le plugin soit trouvé il faut positionner correctement Plugin car forName délègue la recherche des classes au chargeur de classes des applications. Ce chargeur de classes recherche dans le classpath, en tenant compte du nom du paquetage de la classe.