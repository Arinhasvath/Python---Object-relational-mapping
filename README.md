# Python---Object-relational-mapping

Voici une synthèse détaillée du projet Python - Object-relational mapping, basée sur les informations fournies :

1. Objectif du projet
Le projet vise à lier les bases de données à Python en utilisant deux approches :
a) Utilisation du module MySQLdb pour exécuter des requêtes SQL directement.
b) Utilisation de SQLAlchemy comme ORM (Object Relational Mapper) pour abstraire les opérations de base de données.

2. Contexte
- Environnement : Ubuntu 20.04 LTS
- Version Python : 3.8.5
- Base de données : MySQL 8.0
- Modules principaux : MySQLdb (version 2.0.x), SQLAlchemy (version 1.4.x)

3. Objectifs d'apprentissage
- Connecter un script Python à une base de données MySQL
- Exécuter des requêtes SELECT et INSERT via Python
- Comprendre le concept d'ORM
- Mapper une classe Python à une table MySQL

4. Configuration de l'environnement
a) Installation de MySQL 8.0 :
```bash
sudo apt update
sudo apt install mysql-server
```

b) Installation de MySQLdb :
```bash
sudo apt-get install python3-dev libmysqlclient-dev zlib1g-dev
sudo pip3 install mysqlclient
```

c) Installation de SQLAlchemy :
```bash
sudo pip3 install SQLAlchemy
```

5. Structure du projet
Le projet est divisé en plusieurs exercices, chacun se concentrant sur un aspect spécifique de l'interaction entre Python et MySQL.

6. Exercices détaillés

6.1 Exercice 0 : Get all states
- Fichier : `0-select_states.py`
- Objectif : Lister tous les états de la base de données
- Fonctions principales :
  ```python
  import MySQLdb
  conn = MySQLdb.connect(host="localhost", port=3306, user=sys.argv[1], passwd=sys.argv[2], db=sys.argv[3])
  cur = conn.cursor()
  cur.execute("SELECT * FROM states ORDER BY id ASC")
  ```

6.2 Exercice 1 : Filter states
- Fichier : `1-filter_states.py`
- Objectif : Lister les états commençant par 'N'
- Modification clé :
  ```python
  cur.execute("SELECT * FROM states WHERE name LIKE 'N%' ORDER BY id ASC")
  ```

6.3 Exercice 2 : Filter states by user input
- Fichier : `2-my_filter_states.py`
- Objectif : Filtrer les états selon l'entrée utilisateur
- Point clé :
  ```python
  query = "SELECT * FROM states WHERE name = '{}' ORDER BY id ASC".format(sys.argv[4])
  cur.execute(query)
  ```

6.4 Exercice 3 : SQL Injection...
- Fichier : `3-my_safe_filter_states.py`
- Objectif : Prévenir les injections SQL
- Modification importante :
  ```python
  cur.execute("SELECT * FROM states WHERE name = %s ORDER BY id ASC", (sys.argv[4],))
  ```

6.5 Exercice 4 : Cities by states
- Fichier : `4-cities_by_state.py`
- Objectif : Lister toutes les villes avec leurs états
- Requête clé :
  ```python
  cur.execute("""
      SELECT cities.id, cities.name, states.name 
      FROM cities 
      JOIN states ON cities.state_id = states.id 
      ORDER BY cities.id ASC
  """)
  ```

6.6 Exercice 5 : All cities by state
- Fichier : `5-filter_cities.py`
- Objectif : Lister les villes d'un état spécifique
- Requête principale :
  ```python
  cur.execute("""
      SELECT cities.name 
      FROM cities 
      JOIN states ON cities.state_id = states.id 
      WHERE states.name = %s 
      ORDER BY cities.id ASC
  """, (sys.argv[4],))
  ```

7. Transition vers SQLAlchemy (ORM)

7.1 Définition du modèle State
- Fichier : `model_state.py`
- Structure :
  ```python
  from sqlalchemy import Column, Integer, String
  from sqlalchemy.ext.declarative import declarative_base

  Base = declarative_base()

  class State(Base):
      __tablename__ = 'states'
      id = Column(Integer, primary_key=True)
      name = Column(String(128), nullable=False)
  ```

7.2 Utilisation de SQLAlchemy
- Exemple de requête :
  ```python
  from sqlalchemy import create_engine
  from sqlalchemy.orm import sessionmaker
  from model_state import Base, State

  engine = create_engine('mysql+mysqldb://{}:{}@localhost/{}'.format(sys.argv[1], sys.argv[2], sys.argv[3]))
  Session = sessionmaker(bind=engine)
  session = Session()

  for state in session.query(State).order_by(State.id):
      print("{}: {}".format(state.id, state.name))
  ```

8. Diagramme simplifié de l'architecture

```
[Python Script] <-> [ORM (SQLAlchemy)] <-> [MySQL Database]
       ^
       |
[MySQLdb pour requêtes directes]
```

9. Bonnes pratiques
- Utiliser des requêtes paramétrées pour éviter les injections SQL
- Fermer les connexions et les curseurs après utilisation
- Utiliser des sessions SQLAlchemy dans un contexte de gestion (with statement)
- Suivre le style de code PEP 8 (vérifié par pycodestyle)

10. Ressources supplémentaires
- Documentation MySQLdb : https://mysqlclient.readthedocs.io/
- Tutoriel SQLAlchemy : https://docs.sqlalchemy.org/en/14/orm/tutorial.html
- Cheatsheet SQLAlchemy : https://www.pythonsheets.com/notes/python-sqlalchemy.html

Cette synthèse couvre les aspects principaux du projet, fournissant une base solide pour comprendre et implémenter les interactions entre Python et MySQL, tant avec des requêtes SQL directes qu'avec un ORM.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt

## Les principales fonctionnalités et concepts abordés dans ce projet, classés par catégories. Voici le tableau :

| Catégorie | Fonctionnalité/Concept | Description | Exemple |
|-----------|------------------------|-------------|---------|
| **Base de données MySQL** |
| | Connexion | Établir une connexion à la base de données MySQL | `MySQLdb.connect(host, user, passwd, db)` |
| | Exécution de requêtes | Exécuter des requêtes SQL directes | `cursor.execute("SELECT * FROM states")` |
| | Récupération de résultats | Récupérer les résultats d'une requête | `cursor.fetchall()` |
| | Fermeture de connexion | Fermer proprement la connexion à la base de données | `cursor.close()`, `db.close()` |
| **MySQLdb** |
| | Importation | Importer le module MySQLdb | `import MySQLdb` |
| | Création de curseur | Créer un curseur pour exécuter des requêtes | `cursor = db.cursor()` |
| | Requêtes paramétrées | Utiliser des requêtes paramétrées pour éviter les injections SQL | `cursor.execute("SELECT * FROM states WHERE name = %s", (state_name,))` |
| **SQLAlchemy (ORM)** |
| | Définition de modèle | Définir une classe Python qui représente une table | `class State(Base):` |
| | Mappage de colonnes | Mapper les colonnes de la table aux attributs de la classe | `name = Column(String(128), nullable=False)` |
| | Création de session | Créer une session SQLAlchemy pour interagir avec la base de données | `Session = sessionmaker(bind=engine)` |
| | Requêtes ORM | Effectuer des requêtes en utilisant l'API de SQLAlchemy | `session.query(State).all()` |
| | Filtrage | Filtrer les résultats d'une requête | `session.query(State).filter(State.name.like('N%'))` |
| | Tri | Trier les résultats d'une requête | `session.query(State).order_by(State.id)` |
| **Sécurité** |
| | Protection contre les injections SQL | Utiliser des requêtes paramétrées ou l'ORM pour éviter les injections SQL | Voir exemples MySQLdb et SQLAlchemy |
| **Fonctionnalités avancées** |
| | Jointures | Effectuer des jointures entre tables | `session.query(City).join(State)` |
| | Relation entre modèles | Définir des relations entre les modèles SQLAlchemy | `cities = relationship("City", backref="state")` |
| **Bonnes pratiques** |
| | Gestion des erreurs | Utiliser des blocs try/except pour gérer les erreurs de base de données | `try: ... except MySQLdb.Error as e: ...` |
| | Utilisation de with | Utiliser le contexte 'with' pour la gestion automatique des ressources | `with engine.connect() as conn: ...` |
| | Documentation | Documenter les classes, méthodes et modules | Utilisation de docstrings |
| **Outils et configuration** |
| | Installation de paquets | Installer les paquets nécessaires (MySQLdb, SQLAlchemy) | `pip install mysqlclient SQLAlchemy` |
| | Configuration de la base de données | Configurer et initialiser la base de données MySQL | Utilisation de scripts SQL fournis |

Ce tableau offre une vue d'ensemble structurée des principales fonctionnalités et concepts abordés dans le projet, organisés par catégories pour une meilleure compréhension et référence rapide.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt

## Tableau détaillé des fonctions et mots-clés techniques utilisés dans les exercices, en expliquant leur utilité et leur fonction. Voici le tableau :

| Mot-clé/Fonction | Utilisation | Description |
|------------------|-------------|-------------|
| `SELECT` | SQL | Permet de récupérer des données d'une ou plusieurs tables |
| `WHERE` | SQL | Filtre les résultats d'une requête selon une condition spécifique |
| `ORDER BY` | SQL | Trie les résultats d'une requête |
| `JOIN` | SQL | Combine des lignes de deux ou plusieurs tables |
| `LIKE` | SQL | Recherche un motif spécifique dans une colonne |
| `%` | SQL | Caractère joker dans une clause LIKE, représente zéro ou plusieurs caractères |
| `connect()` | MySQLdb | Établit une connexion à la base de données MySQL |
| `cursor()` | MySQLdb | Crée un objet curseur pour exécuter des requêtes SQL |
| `execute()` | MySQLdb | Exécute une requête SQL |
| `fetchall()` | MySQLdb | Récupère tous les résultats d'une requête |
| `close()` | MySQLdb | Ferme la connexion à la base de données |
| `format()` | Python | Formate une chaîne de caractères en insérant des valeurs |
| `sys.argv` | Python | Accède aux arguments de ligne de commande |
| `import` | Python | Importe un module dans le script |
| `if __name__ == "__main__":` | Python | Vérifie si le script est exécuté directement |
| `create_engine()` | SQLAlchemy | Crée une connexion à la base de données |
| `declarative_base()` | SQLAlchemy | Crée une classe de base pour les modèles déclaratifs |
| `Column()` | SQLAlchemy | Définit une colonne dans un modèle SQLAlchemy |
| `String()` | SQLAlchemy | Définit un type de colonne pour les chaînes de caractères |
| `Integer()` | SQLAlchemy | Définit un type de colonne pour les entiers |
| `relationship()` | SQLAlchemy | Définit une relation entre deux modèles |
| `sessionmaker()` | SQLAlchemy | Crée une fabrique de sessions pour interagir avec la base de données |
| `query()` | SQLAlchemy | Commence une requête sur un modèle |
| `filter()` | SQLAlchemy | Filtre les résultats d'une requête SQLAlchemy |
| `all()` | SQLAlchemy | Récupère tous les résultats d'une requête SQLAlchemy |
| `first()` | SQLAlchemy | Récupère le premier résultat d'une requête SQLAlchemy |
| `add()` | SQLAlchemy | Ajoute un nouvel objet à la session |
| `commit()` | SQLAlchemy | Valide les changements dans la base de données |
| `delete()` | SQLAlchemy | Supprime un objet de la base de données |
| `__tablename__` | SQLAlchemy | Spécifie le nom de la table pour un modèle |
| `ForeignKey()` | SQLAlchemy | Définit une clé étrangère dans un modèle |
| `backref` | SQLAlchemy | Crée une référence bidirectionnelle entre les modèles |

Ce tableau couvre la plupart des fonctions et mots-clés techniques utilisés dans les exercices, fournissant une référence rapide pour comprendre leur utilité dans le contexte de l'interaction entre Python et MySQL, que ce soit via MySQLdb ou SQLAlchemy.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt
