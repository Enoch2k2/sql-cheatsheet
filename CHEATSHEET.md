# SQL CheatSheet
- SQLITE3 is a type of database we use in order to store data
  - to get into sqlite3 you will need in the terminal to type sqlite3 database_name (you can choose what database_name you would want to give it)
- Creating Files to do sql
  - you can create new files to do the queries in, they need to have the .sql extension to that file. You might have a create_pets.sql file that has the sql for creating your table and another file insert_pets.sql for inserting new pets to the database. To run the files and update your database simple type sqlite3 < file_name.sql
- CREATE TABLE table_name (values);
- ALTER TABLE table_name ADD column datatype;
- INSERT INTO table_name (columns) VALUES (values);
- UPDATE table_name SET column = value WHERE (condition); Updated data in our database.
- DELETE table_name WHERE condition (removes data from a database);
- SELECT (colums) FROM table_name;
- WHERE (condition)
- WHERE (column) BETWEEN value AND value;
- GROUP BY column (condenses query to one selection by the group)
- ORDER BY column ASC / DESC (orders either alphabetical or numeric)
- SELECT column AS alias (will alias the column by any alias you choose)
- MIN(column) will select minimum that is grouped
- SUM(column) will sum the total that is grouped
- MAX(column) will select the max that is grouped
- HAVING (condition) uses aggregate methods for conditional purposes (must have a group by)
- LIMIT(number) will limit the query based on what number you give
- SELECT (column) FROM table_name INNER JOINS other_table_name ON other_table_name.id = table.other_table_name_id (will associate the queries based on forgeign keys)

# Useful Sqlite3 Commands
- All of these commands inside the database prompt (after typing sqlite3 database_name to go into your database)
- `.mode column` sets the queries into columns
- `.headers on` displays the headers of each column
- `.width auto` automatically adjust the width of each column to display correctly (and not look gross)

# Examples:
## Tables
### Creating a table:
```
CREATE TABLE pets (
  id INTEGER PRIMARY KEY, // INTEGER is long for int in ruby
  name TEXT, // TEXT is kind of like string in ruby
  age INTEGER,
  species TEXT,
  owner_id INTEGER
);
```
Creates a table for pets with columns id, name, age, species, and owner_id.

### Add a column
```
ALTER TABLE pets ADD mood TEXT;
```
Will Add a mood as a column to the pet

### Change a datatype of a column
```
ALTER TABLE pets ALTER COLUMN mood INTEGER;
```
Mood is now an integer

### Delete a column
```
ALTER TABLE pets DROP COLUMN mood;
```
Mood is now gone from the pets schema.

## Add / update / remove data
### Inserting Data:
```
INSERT INTO pets (name, age, species, owner_id) VALUES ("Maru", 2, "Cat", 1);
```
Creates a cat who's name is Maru, that is 2 years older, and is a cat. Maru belongs to the owner who's id is 1.

### Editing Data:
```
UPDATE pets SET name = "Garfield" WHERE name = "Maru";
```
Will change the name of Maru to Garfield.

### Deleting Data:
```
DELETE pets WHERE name = "Garifled";
```
Garfield would be deleted from the database.

## Querying
### Selecting your queries
```
SELECT name FROM pets;
```
Gives us back all the names from all the pets

```
SELECT * FROM pets;
```
Gives us back all the information about all of the pets;

### Where clause
```
SELECT * FROM pets WHERE species = "Dog";
```
Will select all of the dogs and return all of the information about the dogs

```
SELECT * FROM pets WHERE age > 2;
```
Will return all of the pets that are 3 years or older.

### MIN / SUM / MAX
```
SELECT species, MIN(net_worth) FROM pets GROUP BY species;
```
Returns the species and the value of the min net_worth of that species. Since each species is group, it will select the minimum value of the net_worth in that group

```
SELECT species, SUM(net_worth) FROM pets GROUP BY species;
```
Returns the species and the total net_worth for each group.

```
SELECT species, MAX(net_worth) FROM pets GROUP BY species;
```

Returns the maximum net_worth for each species.

### ORDER BY ASC / DESC
```
SELECT * FROM pets ORDER BY name ASC;
```
Orders your query in ascending order

```
SELECT * FROM pets ORDER BY net_worth DESC;
```
Orders your query in descending order

### LIMIT
```
SELECT species, SUM(net_worth) FROM pets GROUP BY species ORDER BY SUM(net_worth) DESC LIMIT 1;
```
Returns the species and sum net worth from the pets in descending order by the sum of the net_worth and limits to 1. Which means we queried the species with the maximum total net_worth.

### HAVING
```
SELECT name, species, net_worth FROM pets GROUP BY name HAVING net_worth > 500;
```
Returns the name, species, and net_worth from the pets that have a net_worth greater than 500.

### ALIAS
```
SELECT species, SUM(net_worth) AS total_net_worth FROM pets GROUP BY species;
```
Aliases SUM(net_worth) that we select as total_net_worth in our query.

### INNER JOINS
```
SELECT owners.name, COUNT(pets.owner_id) AS number_of_pets FROM pets
INNER JOIN owners
ON owners.id = pets.owner_id
GROUP BY owners.id;
```
Returns the number of pets each owner has.
