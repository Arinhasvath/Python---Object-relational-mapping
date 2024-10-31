# Python - Mappage objet-relationnel

## Contexte

Dans ce projet, vous allez relier deux mondes fascinants : les bases de données et Python !

Dans la première partie, vous utiliserez le module `MySQLdb` pour vous connecter à une base de données MySQL et exécuter vos requêtes SQL.

Dans la seconde partie, vous utiliserez le module `SQLAlchemy` (ne me demandez pas comment le prononcer...), un Mappeur Objet-Relationnel (ORM).

La plus grande différence est : plus de requêtes SQL ! En effet, le but d'un ORM est d'abstraire le stockage pour l'utilisation. Avec un ORM, votre principale préoccupation sera "Que puis-je faire avec mes objets" et non "Comment cet objet est-il stocké ? où ? quand ?". Vous n'écrirez aucune requête SQL, seulement du code Python. Dernier point, votre code ne sera pas dépendant du type de stockage. Vous pourrez changer votre stockage facilement sans réécrire l'intégralité de votre projet.

Sans ORM :

```python
conn = MySQLdb.connect(host="localhost", port=3306, user="root", passwd="root", db="ma_db", charset="utf8")
cur = conn.cursor()
cur.execute("SELECT * FROM etats ORDER BY id ASC") # ICI je dois connaître SQL pour récupérer tous les états dans ma base de données
lignes_requete = cur.fetchall()
for ligne in lignes_requete:
    print(ligne)
cur.close()
conn.close()
```

Avec un ORM :

```python
moteur = create_engine('mysql+mysqldb://{}:{}@localhost/{}'.format("root", "root", "ma_db"), pool_pre_ping=True)
Base.metadata.create_all(moteur)

session = Session(moteur)
for etat in session.query(Etat).order_by(Etat.id).all(): # ICI : pas de requête SQL, seulement des objets !
    print("{}: {}".format(etat.id, etat.nom))
session.close()
```

Vous voyez la différence ? Cool, non ?

La plus grande difficulté avec les ORM est : La syntaxe !

En effet, ils ont tous le même type de syntaxe, mais pas toujours. Veuillez lire les tutoriels et ne lisez pas toute la documentation avant de commencer, sautez-y simplement si vous ne comprenez pas quelque chose.

## Ressources

Lisez ou regardez :

- [Mappeurs objet-relationnel](lien_vers_ressource)
- [Documentation mysqlclient/MySQLdb](lien_vers_ressource) (ne faites pas attention à _mysql)
- [Tutoriel MySQLdb](lien_vers_ressource)
- [Tutoriel SQLAlchemy](lien_vers_ressource)
- [SQLAlchemy](lien_vers_ressource)
- [mysqlclient/MySQLdb](lien_vers_ressource)
- [Introduction à SQLAlchemy](lien_vers_ressource)
- [Flask SQLAlchemy](lien_vers_ressource)
- [10 obstacles courants pour les débutants en SQLAlchemy](lien_vers_ressource)
- [Aide-mémoire Python SQLAlchemy](lien_vers_ressource)
- [Tutoriel ORM SQLAlchemy pour les développeurs Python](lien_vers_ressource) (Attention : Ce tutoriel utilise PostgreSQL, mais le concept de SQLAlchemy est le même avec MySQL)
- [Tutoriel SQLAlchemy](lien_vers_ressource)

## Objectifs d'apprentissage

À la fin de ce projet, vous devriez être capable d'expliquer à n'importe qui, sans l'aide de Google :

### Général
- Comment se connecter à une base de données MySQL depuis un script Python
- Comment effectuer des opérations SELECT sur une table MySQL depuis un script Python
- Comment effectuer des opérations INSERT sur une table MySQL depuis un script Python
- Ce que signifie ORM
- Comment mapper une classe Python à une table MySQL

## Exigences

### Général
- Éditeurs autorisés : vi, vim, emacs
- Tous vos fichiers seront interprétés/compilés sur Ubuntu 20.04 LTS en utilisant python3 (version 3.8.5)
- Vos fichiers seront exécutés avec MySQLdb version 2.0.x
- Vos fichiers seront exécutés avec SQLAlchemy version 1.4.x
- Tous vos fichiers doivent se terminer par une nouvelle ligne
- La première ligne de tous vos fichiers doit être exactement `#!/usr/bin/python3`
- Un fichier README.md, à la racine du dossier du projet, est obligatoire
- Votre code doit utiliser le style pycodestyle (version 2.7.*)
- Tous vos fichiers doivent être exécutables
- La longueur de vos fichiers sera testée en utilisant `wc`
- Tous vos modules doivent avoir une documentation (`python3 -c 'print(__import__("mon_module").__doc__)'`)
- Toutes vos classes doivent avoir une documentation (`python3 -c 'print(__import__("mon_module").MaClasse.__doc__)'`)
- Toutes vos fonctions (à l'intérieur et à l'extérieur d'une classe) doivent avoir une documentation (`python3 -c 'print(__import__("mon_module").ma_fonction.__doc__)'` et `python3 -c 'print(__import__("mon_module").MaClasse.ma_fonction.__doc__)'`)
- Une documentation n'est pas un simple mot, c'est une vraie phrase expliquant le but du module, de la classe ou de la méthode (la longueur sera vérifiée)
- Vous n'êtes pas autorisé à utiliser `execute` avec sqlalchemy

## Plus d'informations

### Installation de MySQL 8.0 sur Ubuntu 20.04 LTS

```bash
$ sudo apt update
$ sudo apt install mysql-server
...
$ mysql --version
mysql  Ver 8.0.25-0ubuntu0.20.04.1 for Linux on x86_64 ((Ubuntu))
$
```

Connectez-vous à votre serveur MySQL :

```bash
$ sudo mysql
```

### Installation du module MySQLdb version 2.0.x

Pour installer MySQLdb, vous devez avoir MySQL installé.

```bash
$ sudo apt-get install python3-dev
$ sudo apt-get install libmysqlclient-dev
$ sudo apt-get install zlib1g-dev
$ sudo pip3 install mysqlclient
...
$ python3
>>> import MySQLdb
>>> MySQLdb.version_info 
(2, 0, 3, 'final', 0)
```

### Installation du module SQLAlchemy version 1.4.x

```bash
$ sudo pip3 install SQLAlchemy
...
$ python3
>>> import sqlalchemy
>>> sqlalchemy.__version__ 
'1.4.22'
```

Vous pouvez également avoir ce message d'avertissement :

```
/usr/local/lib/python3.4/dist-packages/sqlalchemy/engine/default.py:552: Warning: (1681, "'@@SESSION.GTID_EXECUTED' is deprecated and will be re
moved in a future release.")                                                                                                                    
  cursor.execute(statement, parameters)  
```

Vous pouvez l'ignorer.

## Tâches

[Les tâches spécifiques seraient listées ici, avec leurs descriptions détaillées et les exigences pour chaque script à écrire.]

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt
[3] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/c5d7607f-ffa8-4e57-81cc-cafd67c04850/paste.txt
