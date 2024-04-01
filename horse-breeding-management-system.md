# Database Context and Schema Explanation

## Context:
- A small horse breeding operation aims to computerize its system to track the development and career of horses from its stable.
- The objective is to understand the characteristics of the various individuals involved in the breeding, including their name, address, and phone number. Each person is identified by a unique number. These individuals have specific roles (director, secretary, breeder, accountant, owner, maintenance, breeder-owner...).
- Every staff member is personally supervised by a superior (except the director who is at the top of the power hierarchy).
- Information about the name, breed, and color of each horse is known. Each horse is assigned a unique number for identification from birth. Each horse breed has unique characteristics such as its typical weight and size. The growth of each horse is also tracked by recording its weight and size every month. This enables the construction of growth curves specific to each animal and allows for the early detection of any anomalies compared to the typical weight and size of its breed.
- The pedigree of each horse is known, meaning that the characteristics of its parents can be traced. Additionally, each horse has a tattoo number for unique identification, which is assigned to a horse only from the age of 2 months. Each horse is cared for by a specific breeder who trains it for competitions.
- Horses participate in competitions identified by their name, which take place annually. In addition to the number of participants in each event, the placement of each horse from the breeding operation is recorded, enabling statistics on the top winners.
- Horses have an owner whose name, address, and phone number are known. Each owner is also supported by a staff member who coordinates their horses' competitions. Each horse has only one owner at any given time, but it can change owners multiple times, and the database records this evolution along with the price set for each transaction.

## Table: Personne
- **Columns**:
  - `num`: INT (Primary Key)
  - `nom`: VARCHAR(45)
  - `telephonePers`: VARCHAR(45)
  - `adressePers`: VARCHAR(45)
  - `fonctionPers`: VARCHAR(45)
  - `numPersSup`: INT
- **Description**: Represents information about individuals involved in the database.

## Table: Race
- **Columns**:
  - `nom`: VARCHAR(20) (Primary Key)
  - `poidsType`: DOUBLE
  - `tailleType`: DOUBLE
- **Description**: Describes different horse breeds along with their typical weight and height.

## Table: Cheval
- **Columns**:
  - `num`: INT (Primary Key)
  - `nom`: VARCHAR(45)
  - `numTatouage`: VARCHAR(45)
  - `couleurCheval`: VARCHAR(45)
  - `numChevalPere`: INT
  - `numChevalMere`: INT
  - `numPersEleveur`: INT
  - `nomRace`: VARCHAR(20) (Foreign Key referencing Race(nom))
- **Description**: Stores information about horses, including their parentage and breeder.

## Table: proprieter
- **Columns**:
  - `numPers`: INT (Foreign Key referencing Personne(num))
  - `numCheval`: INT (Foreign Key referencing Cheval(num))
  - `DateAchat`: DATE
  - `prixAchat`: DOUBLE
- **Description**: Represents ownership details of horses by individuals.

## Table: Concours
- **Columns**:
  - `libelleConcours`: VARCHAR(40)
  - `AnneeConcours`: INT
  - `NbrParticipants`: INT
  - `numCheval`: INT (Foreign Key referencing Cheval(num))
  - `Place`: VARCHAR(45)
- **Description**: Contains information about horse competitions, including the participating horses and their rankings.

## Table: Croissance
- **Columns**:
  - `id_mois`: INT (Primary Key)
  - `Mois`: INT
  - `tailleMois`: VARCHAR(45)
  - `poidsMois`: VARCHAR(45)
  - `numCheval`: INT (Foreign Key referencing Cheval(num))
- **Description**: Tracks the growth details of horses over different months.

## Table: ParticipationConcours
- **Columns**:
  - `Cheval_num`: INT (Foreign Key referencing Cheval(num))
  - `Concours_libelleConcours`: VARCHAR(40) (Foreign Key referencing Concours(libelleConcours))
  - `Concours_AnneeConcours`: INT
  - `Place`: VARCHAR(45)
- **Description**: Records the participation of horses in various competitions along with their rankings.

## Queries shows how database works

### 1. Black horse count
    SELECT COUNT(*) FROM cheval
    WHERE `couleurCheval`="noir";

### 2. Anglo-Arabian count
    SELECT COUNT(*) FROM cheval
    WHERE `nomRace`="AngloArabe";

### 3. Horse details by tattoo
    SELECT `num`,`nom` FROM cheval
    WHERE `numTatouage`="T0415"

### 4. Dubai competition count
    SELECT COUNT(*) FROM concours
    WHERE `libelleConcours`= "Concours Dubai";

### 5. Total participants in Dubai competition
    SELECT SUM(`NbrParticipants`) FROM concours
    WHERE `libelleConcours`= "Concours Dubai";

### 6. Horses in London competition (2018)
    SELECT `Cheval_num` FROM participationconcours
    WHERE `Concours_AnneeConcours`=2018 AND`Concours_libelleConcours`="londres";

### 7. People with director function
    SELECT `nom` FROM personne
    WHERE `fonctionPers`="directeur";

### 8. Total horse owners
    SELECT COUNT(*) FROM proprieter;

### 9. Horse purchases by person 6 (2020-2021)
    SELECT COUNT(*) FROM proprieter
    WHERE `numPers`=6 AND (`DateAchat` BETWEEN "2020-01-01" AND "2021-01-01");

### 10. Total purchase price by person 6 (2020-2021)
    SELECT SUM(`prixAchat`) FROM proprieter
    WHERE `numPers`=6 AND (`DateAchat` BETWEEN "2020-01-01" AND "2021-01-01");
