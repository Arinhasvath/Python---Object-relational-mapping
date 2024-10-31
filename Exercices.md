# Exercices

## Exercice 0 : Obtenir tous les états

Écrivez un script qui liste tous les états de la base de données `hbtn_0e_0_usa`.

### Exigences :
- Votre script doit prendre 3 arguments : nom d'utilisateur MySQL, mot de passe MySQL et nom de la base de données (aucune validation des arguments nécessaire).
- Vous devez utiliser le module `MySQLdb` (`import MySQLdb`).
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306.
- Les résultats doivent être triés par ordre croissant de `states.id`.
- Les résultats doivent être affichés comme dans l'exemple ci-dessous.
- Votre code ne doit pas être exécuté lors de l'importation.

### Code :

Fichier : `0-select_states.py`

```python
#!/usr/bin/python3
"""Script that lists all states from the database hbtn_0e_0_usa"""

import MySQLdb
import sys

if __name__ == "__main__":
    # Connexion au serveur MySQL
    db = MySQLdb.connect(
        host="localhost",
        user=sys.argv[1],
        passwd=sys.argv[2],
        db=sys.argv[3],
        port=3306
    )
    
    # Création d'un curseur
    cursor = db.cursor()
    
    # Exécution de la requête pour sélectionner tous les états
    cursor.execute("SELECT * FROM states ORDER BY id ASC")
    
    # Récupération de toutes les lignes
    rows = cursor.fetchall()
    
    # Affichage des résultats
    for row in rows:
        print(row)
    
    # Fermeture du curseur et de la connexion à la base de données
    cursor.close()
    db.close()
```

### Tester le script :
```bash
./0-select_states.py root root hbtn_0e_0_usa
```

### Résultat attendu :
```
(1, 'California')
(2, 'Arizona')
(3, 'Texas')
(4, 'New York')
(5, 'Nevada')
```

---

## Exercice 1 : Filtrer les états

Écrivez un script qui liste tous les états dont le nom commence par "N" (majuscule) dans la base de données `hbtn_0e_0_usa`.

### Exigences :
- Votre script doit prendre 3 arguments : nom d'utilisateur MySQL, mot de passe MySQL et nom de la base de données (aucune validation des arguments nécessaire).
- Vous devez utiliser le module `MySQLdb` (`import MySQLdb`).
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306.
- Les résultats doivent être triés par ordre croissant de `states.id`.
- Les résultats doivent être affichés comme dans l'exemple ci-dessous.
- Votre code ne doit pas être exécuté lors de l'importation.

### Code :

Fichier : `1-filter_states.py`

```python
#!/usr/bin/python3
"""Script that lists all states with a name starting with N from the database hbtn_0e_0_usa"""

import MySQLdb
import sys

if __name__ == "__main__":
    db = MySQLdb.connect(
        host="localhost",
        user=sys.argv[1],
        passwd=sys.argv[2],
        db=sys.argv[3],
        port=3306
    )
    
    cursor = db.cursor()
    
    # Exécution de la requête pour sélectionner les états commençant par 'N'
    cursor.execute("SELECT * FROM states WHERE name LIKE 'N%' ORDER BY id ASC")
    
    rows = cursor.fetchall()
    
    for row in rows:
        print(row)
    
    cursor.close()
    db.close()
```

### Tester le script :
```bash
./1-filter_states.py root root hbtn_0e_0_usa
```

### Résultat attendu :
```
(4, 'New York')
(5, 'Nevada')
```

---

## Exercice 2 : Filtrer les états par saisie utilisateur

Écrivez un script qui prend en argument et affiche toutes les valeurs dans la table `states` de `hbtn_0e_0_usa` où le nom correspond à l'argument.

### Exigences :
- Votre script doit prendre 4 arguments : nom d'utilisateur MySQL, mot de passe MySQL, nom de la base de données et nom d'état recherché (aucune validation des arguments nécessaire).
- Vous devez utiliser le module `MySQLdb` (`import MySQLdb`).
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306.
- Vous devez utiliser `format` pour créer la requête SQL avec l'entrée utilisateur.
- Les résultats doivent être triés par ordre croissant de `states.id`.
- Les résultats doivent être affichés comme dans l'exemple ci-dessous.
- Votre code ne doit pas être exécuté lors de l'importation.

### Code :

Fichier : `2-my_filter_states.py`

```python
#!/usr/bin/python3
"""Script that takes in an argument and displays all values in the states table where name matches the argument."""

import MySQLdb
import sys

if __name__ == "__main__":
    db = MySQLdb.connect(
        host="localhost",
        user=sys.argv[1],
        passwd=sys.argv[2],
        db=sys.argv[3],
        port=3306
    )
    
    cursor = db.cursor()
    
    state_name = sys.argv[4]
    
    # Utilisation de format pour créer la requête SQL avec l'entrée utilisateur
    cursor.execute("SELECT * FROM states WHERE name = %s ORDER BY id ASC", (state_name,))
    
    rows = cursor.fetchall()
    
    for row in rows:
        print(row)
    
    cursor.close()
    db.close()
```

### Tester le script :
```bash
./2-my_filter_states.py root root hbtn_0e_0_usa 'Arizona'
```

### Résultat attendu :
```
(2, 'Arizona')
```

---

## Exercice 3 : Sécurité contre les injections SQL

Écrivez un script qui prend en arguments et affiche toutes les valeurs dans la table `states` de `hbtn_0e_0_usa` où le nom correspond à l'argument. Mais cette fois, écrivez un script qui est sécurisé contre les injections SQL !

### Exigences :
- Votre script doit prendre 4 arguments : nom d'utilisateur MySQL, mot de passe MySQL, nom de la base de données et nom d'état recherché (sécurisé contre les injections SQL).
- Vous devez utiliser le module `MySQLdb` (`import MySQLdb`).
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306.
- Les résultats doivent être triés par ordre croissant de `states.id`.
- Les résultats doivent être affichés comme dans l'exemple ci-dessous.
- Votre code ne doit pas être exécuté lors de l'importation.

### Code :

Fichier : `3-my_safe_filter_states.py`

```python
#!/usr/bin/python3
"""Script that displays all values in the states table where name matches the argument, safe from SQL injection."""

import MySQLdb
import sys

if __name__ == "__main__":
    db = MySQLdb.connect(
        host="localhost",
        user=sys.argv[1],
        passwd=sys.argv[2],
        db=sys.argv[3],
        port=3306
    )
    
    cursor = db.cursor()
    
    state_name = sys.argv[4]
    
   # Utilisation d'une requête paramétrée pour prévenir les injections SQL 
   cursor.execute("SELECT * FROM states WHERE name = %s ORDER BY id ASC", (state_name,))
   
   rows = cursor.fetchall()

   for row in rows:
       print(row)

   cursor.close()
   db.close()
```

### Tester le script :
```bash
./3-my_safe_filter_states.py root root hbtn_0e_0_usa 'Arizona'
```

### Résultat attendu :
```
(2, 'Arizona')
```

---

## Exercice 4 : Villes par état

Écrivez un script qui liste toutes les villes de la base de données `hbtn_0e_4_usa`.

### Exigences :
- Votre script doit prendre 3 arguments : nom d'utilisateur MySQL, mot de passe MySQL et nom de la base de données.
- Vous devez utiliser le module `MySQLdb` (`import MySQLdb`).
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306.
- Les résultats doivent être triés par ordre croissant de `cities.id`.
- Vous ne pouvez utiliser `execute()` qu'une seule fois.
- Les résultats doivent être affichés comme dans l'exemple ci-dessous.
- Votre code ne doit pas être exécuté lors de l'importation.

### Code :

Fichier : `4-cities_by_state.py`

```python
#!/usr/bin/python3
"""Script that lists all cities from the database hbtn_0e_4_usa"""

import MySQLdb
import sys

if __name__ == "__main__":
    db = MySQLdb.connect(
        host="localhost",
        user=sys.argv[1],
        passwd=sys.argv[2],
        db=sys.argv[3],
        port=3306
    )
    
    cursor = db.cursor()
    
    # Exécution d'une requête pour sélectionner toutes les villes avec leurs noms d'état correspondants 
    cursor.execute("""
        SELECT cities.id, cities.name, states.name 
        FROM cities 
        JOIN states ON cities.state_id = states.id 
        ORDER BY cities.id ASC""")
    
    rows = cursor.fetchall()
    
    for row in rows:
        print(row)
    
    cursor.close()
    db.close()
```

### Tester le script :
```bash
./4-cities_by_state.py root root hbtn_0e_4_usa
```

### Résultat attendu :
```
(1, 'San Francisco', 'California')
(2, 'San Jose', 'California')
(3, 'Los Angeles', 'California')
(4, 'Fremont', 'California')
(5, 'Livermore', 'California')
(6, 'Page', 'Arizona')
(7, 'Phoenix', 'Arizona')
(8, 'Dallas', 'Texas')
(9, 'Houston', 'Texas')
(10, 'Austin', 'Texas')
(11, 'New York', 'New York')
(12, 'Las Vegas', 'Nevada')
(13, 'Reno', 'Nevada')
(14, 'Henderson', 'Nevada')
(15, 'Carson City', 'Nevada')
```

---

Si vous avez besoin d'autres exercices ou d'informations supplémentaires sur ceux-ci, faites-le moi savoir !

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt

## Exercice 5 : Toutes les villes par état

Écrivez un script qui prend le nom d'un état comme argument et liste toutes les villes de cet état, en utilisant la base de données `hbtn_0e_4_usa`.

- Votre script doit prendre 4 arguments : nom d'utilisateur MySQL, mot de passe MySQL, nom de la base de données et nom de l'état (sans risque d'injection SQL !)
- Vous devez utiliser le module `MySQLdb` (`import MySQLdb`)
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306
- Les résultats doivent être triés par ordre croissant de `cities.id`
- Vous ne pouvez utiliser `execute()` qu'une seule fois
- Les résultats doivent être affichés comme dans l'exemple ci-dessous
- Votre code ne doit pas être exécuté lors de l'importation

```bash
guillaume@ubuntu:~/$ ./5-filter_cities.py root root hbtn_0e_4_usa Texas
Dallas, Houston, Austin
guillaume@ubuntu:~/$ ./5-filter_cities.py root root hbtn_0e_4_usa Hawaii

guillaume@ubuntu:~/$
```

Aucun cas de test nécessaire

## Exercice 6 : Premier état modèle d'état

Écrivez un script Python qui contient la classe définition d'un `State` et une instance `Base = declarative_base()` :

- `State` classe :
    - hérite de `Base` (importez `declarative_base()` de `sqlalchemy`)
    - liens à la table MySQL `states`
    - attributs de classe :
        - `id` qui représente une colonne d'un entier auto-incrémenté, ne peut pas être nul et est une clé primaire
        - `name` qui représente une colonne d'une chaîne de caractères avec un maximum de 128 caractères et ne peut pas être nulle
- Vous devez utiliser le module `SQLAlchemy`
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306
- **ATTENTION** : Il est important que toutes vos classes qui héritent de `Base` soient importées avant d'appeler `Base.metadata.create_all(engine)`

```python
guillaume@ubuntu:~/$ cat 6-model_state.sql
-- Create database hbtn_0e_6_usa
CREATE DATABASE IF NOT EXISTS hbtn_0e_6_usa;
USE hbtn_0e_6_usa;
SHOW CREATE TABLE states;

guillaume@ubuntu:~/$ cat 6-model_state.sql | mysql -uroot -p
Enter password: 
ERROR 1146 (42S02) at line 4: Table 'hbtn_0e_6_usa.states' doesn't exist
guillaume@ubuntu:~/$ cat 6-model_state.py
#!/usr/bin/python3
"""Start link class to table in database 
"""
import sys
from model_state import Base, State

from sqlalchemy import (create_engine)

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost/{}'.format(sys.argv[1], sys.argv[2], sys.argv[3]), pool_pre_ping=True)
    Base.metadata.create_all(engine)

guillaume@ubuntu:~/$ ./6-model_state.py root root hbtn_0e_6_usa
guillaume@ubuntu:~/$ cat 6-model_state.sql | mysql -uroot -p
Enter password: 
Table   Create Table
states  CREATE TABLE `states` (\n  `id` int(11) NOT NULL AUTO_INCREMENT,\n  `name` varchar(128) NOT NULL,\n  PRIMARY KEY (`id`)\n) ENGINE=InnoDB DEFAULT CHARSET=latin1
guillaume@ubuntu:~/$ 
```

Aucun cas de test nécessaire

## Exercice 7 : Tous les états via SQLAlchemy

Écrivez un script qui liste tous les objets `State` de la base de données `hbtn_0e_6_usa`

- Votre script doit prendre 3 arguments : nom d'utilisateur MySQL, mot de passe MySQL et nom de la base de données
- Vous devez utiliser le module `SQLAlchemy`
- Vous devez importer `State` et `Base` de `model_state` - `from model_state import Base, State`
- Votre script doit se connecter à un serveur MySQL fonctionnant sur `localhost` au port 3306
- Les résultats doivent être triés par ordre croissant de `states.id`
- Les résultats doivent être affichés comme dans l'exemple ci-dessous
- Votre code ne doit pas être exécuté lors de l'importation

```bash
guillaume@ubuntu:~/$ cat 7-model_state_fetch_all.sql
-- Insert states
INSERT INTO states (name) VALUES ("California"), ("Arizona"), ("Texas"), ("New York"), ("Nevada");

guillaume@ubuntu:~/$ cat 7-model_state_fetch_all.sql | mysql -uroot -p hbtn_0e_6_usa
Enter password: 
guillaume@ubuntu:~/$ ./7-model_state_fetch_all.py root root hbtn_0e_6_usa
1: California
2: Arizona
3: Texas
4: New York
5: Nevada
guillaume@ubuntu:~/$ 
```

Aucun cas de test nécessaire

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt

Voici la suite des exercices avec leurs explications en français :

## Exercice 8 : Premier état

Écrivez un script qui affiche le premier objet `State` de la base de données `hbtn_0e_6_usa`.

### Code :

Fichier : `8-model_state_fetch_first.py`

```python
#!/usr/bin/python3
"""Script qui affiche le premier objet State de la base de données"""

import sys
from model_state import Base, State
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    first_state = session.query(State).order_by(State.id).first()
    if first_state:
        print("{}: {}".format(first_state.id, first_state.name))
    else:
        print("Nothing")

    session.close()
```

### Explication :
- Ce script récupère le premier état de la base de données, trié par ID.
- Si aucun état n'est trouvé, il affiche "Nothing".

### Pour tester :
```bash
./8-model_state_fetch_first.py root root hbtn_0e_6_usa
```

## Exercice 9 : Contient 'a'

Écrivez un script qui liste tous les objets `State` qui contiennent la lettre 'a' de la base de données `hbtn_0e_6_usa`.

### Code :

Fichier : `9-model_state_filter_a.py`

```python
#!/usr/bin/python3
"""Script qui liste tous les objets State contenant la lettre 'a'"""

import sys
from model_state import Base, State
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    states = session.query(State).filter(State.name.like('%a%')).order_by(State.id)
    
    for state in states:
        print("{}: {}".format(state.id, state.name))

    session.close()
```

### Explication :
- Ce script utilise le filtre `like` pour trouver tous les états dont le nom contient la lettre 'a'.
- Les résultats sont triés par ID et affichés.

### Pour tester :
```bash
./9-model_state_filter_a.py root root hbtn_0e_6_usa
```

## Exercice 10 : Obtenir un état

Écrivez un script qui affiche l'objet `State` avec le nom passé en argument de la base de données `hbtn_0e_6_usa`.

### Code :

Fichier : `10-model_state_my_get.py`

```python
#!/usr/bin/python3
"""Script qui affiche l'objet State avec le nom passé en argument"""

import sys
from model_state import Base, State
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    state_name = sys.argv[4]
    state = session.query(State).filter(State.name == state_name).first()
    
    if state:
        print(state.id)
    else:
        print("Not found")

    session.close()
```

### Explication :
- Ce script prend un nom d'état en argument et recherche cet état dans la base de données.
- Si l'état est trouvé, il affiche son ID. Sinon, il affiche "Not found".

### Pour tester :
```bash
./10-model_state_my_get.py root root hbtn_0e_6_usa Texas
```

Ces exercices vous permettent de pratiquer des requêtes plus complexes avec SQLAlchemy, y compris le filtrage et la recherche d'objets spécifiques dans la base de données.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt

Voici la suite des exercices avec leurs explications en français :

## Exercice 11 : Ajouter un nouvel état

Écrivez un script qui ajoute l'objet `State` "Louisiana" à la base de données `hbtn_0e_6_usa`.

### Code :

Fichier : `11-model_state_insert.py`

```python
#!/usr/bin/python3
"""Script qui ajoute l'objet State 'Louisiana' à la base de données"""

import sys
from model_state import Base, State
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    new_state = State(name="Louisiana")
    session.add(new_state)
    session.commit()

    print(new_state.id)

    session.close()
```

### Explication :
- Ce script crée un nouvel objet `State` avec le nom "Louisiana".
- Il ajoute cet objet à la session et le sauvegarde dans la base de données.
- Après avoir commité la transaction, il affiche l'ID du nouvel état.

### Pour tester :
```bash
./11-model_state_insert.py root root hbtn_0e_6_usa
```

## Exercice 12 : Mettre à jour un état

Écrivez un script qui change le nom d'un objet `State` de la base de données `hbtn_0e_6_usa`.

### Code :

Fichier : `12-model_state_update_id_2.py`

```python
#!/usr/bin/python3
"""Script qui change le nom d'un objet State"""

import sys
from model_state import Base, State
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    state = session.query(State).filter_by(id=2).first()
    if state:
        state.name = "New Mexico"
        session.commit()

    session.close()
```

### Explication :
- Ce script recherche l'état avec l'ID 2.
- S'il le trouve, il change son nom en "New Mexico".
- Les changements sont ensuite sauvegardés dans la base de données.

### Pour tester :
```bash
./12-model_state_update_id_2.py root root hbtn_0e_6_usa
```

## Exercice 13 : Supprimer des états

Écrivez un script qui supprime tous les objets `State` contenant la lettre 'a' de la base de données `hbtn_0e_6_usa`.

### Code :

Fichier : `13-model_state_delete_a.py`

```python
#!/usr/bin/python3
"""Script qui supprime tous les objets State contenant la lettre 'a'"""

import sys
from model_state import Base, State
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    states_to_delete = session.query(State).filter(State.name.like('%a%')).all()
    for state in states_to_delete:
        session.delete(state)
    session.commit()

    session.close()
```

### Explication :
- Ce script recherche tous les états dont le nom contient la lettre 'a'.
- Il supprime ensuite chacun de ces états de la base de données.
- Les changements sont commités à la fin.

### Pour tester :
```bash
./13-model_state_delete_a.py root root hbtn_0e_6_usa
```

Ces exercices vous permettent de pratiquer les opérations CRUD (Create, Read, Update, Delete) avec SQLAlchemy, en manipulant des objets Python qui représentent des enregistrements dans votre base de données.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/de2c6f8a-5372-4950-98e9-36f0c61974b9/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/09c7772c-1f1d-4805-8038-d0d82f43fb6a/paste-2.txt

Voici la suite des exercices avec leurs explications en français :

## Exercice 14 : Villes dans l'état

Écrivez un script Python qui affiche toutes les villes de l'état `State` passé en argument de la base de données `hbtn_0e_14_usa`.

### Code :

Fichier : `14-model_city_fetch_by_state.py`

```python
#!/usr/bin/python3
"""Script qui affiche toutes les villes d'un état donné"""

import sys
from model_state import Base, State
from model_city import City
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

if __name__ == "__main__":
    engine = create_engine('mysql+mysqldb://{}:{}@localhost:3306/{}'
                           .format(sys.argv[1], sys.argv[2], sys.argv[3]),
                           pool_pre_ping=True)
    Session = sessionmaker(bind=engine)
    session = Session()

    results = session.query(State, City).filter(State.id == City.state_id).order_by(City.id).all()

    for state, city in results:
        print("{}: ({}) {}".format(state.name, city.id, city.name))

    session.close()
```

### Explication :
- Ce script utilise une jointure entre les tables `State` et `City`.
- Il affiche toutes les villes avec leur état correspondant, triées par l'ID de la ville.
- Le format d'affichage est "Nom de l'État: (ID de la ville) Nom de la ville".

### Pour tester :
```bash
./14-model_city_fetch_by_state.py root root hbtn_0e_14_usa
```
