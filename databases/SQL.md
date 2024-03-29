# coding-self-learing
# Structured Query Language (SQL)
Tables with rows and columns that can be access with queries.<br>
records = rows.<br>
fields = cloumns.<br>
DML = data related statements.<br> 
DDL = databse related statements<br>
### RDB 
databases with connections between tables.
#### Primary Key
is a filed with unique record in each row. for example ID.
#### Links
using the `primary key` and `forgein key`, different tables can be "joined".<br>
##### Types
1. one to many:
2. many to many: 
3. one to one:


## Basic DML statements 
```SQL
SELECT <cloumn,> FROM <table>
WHERE condition AND/OR condition...;
```
### conditions
```SQL
column =/!= 1
column LIKE "ABC" / "A%" / "A_"
(_ meaning one random char, % sequence of random chars)
column IN - exsits inside a list
column BETWEEN 1 AND 2
```

#### To Group & SQL functions
```
AVG
COUNT
FIRST
LAST
MIN
MAX
ROUND
SUM
```
group duplicate rows then you can do a function on those groups.
```SQL
select count(user_id) from albums group by user_id;
```
count number of `user_id` groupped row in the row. in the same way you could count(name).<br>
it doesn't matter, the concept is that the functions operate on a "new" table, with the identical by column rows.

### Filtering and Sorting
```SQL
SELECT DISTINCT ...; - without duplicate records
SELECT FROM ORDER BY column ASC/DESC LIMIT max records OFFSET lookup offset.
```
### Join
```SQL
SELECT column, another_table_column
FROM mytable
INNER/LEFT/RIGH/FULL JOIN another_table 
    ON mytable.id = another_table.id
```
**NOTE**: left right and full are if some of the rows does not exsits in the one of the tables.<br>
ON without on join will connect stuff that have no connection.

### Insertion
```SQL
insert into table_name values (), ();
```
### update rows

```SQL
UPDATE mytable
SET column = value_or_expr, 
    other_column = another_value_or_expr, 
    …
WHERE condition;
```
```SQL
delete from table where...;
```

### Transactions
to execute multiple commands at a sequence but if one command isn't successfull then undo all changes.
```SQL
begin;
commands;
commit;
```
alternatives
```SQL
rollback - undo changes
end transaction; = commit
```

## Basic DDL statements
```SQL
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```
### constrains
specify a role about the row or the column. For example not null, or unique.

Link between tables using foreign key
```
FOREIGN KEY (PersonID)  REFERENCES Persons(PersonID)
```

## Setup
Requiremnets:
1. MySQL installer
    1. Workbench
    2. Server
    3. command line (to test it, and Debug)<br>

These [docs](https://dev.mysql.com/doc/mysql-getting-started/en/#mysql-getting-started-connecting)
shows how to use the CLI.
```powershell
cd C:\Program Files\MySQL\MySQL Server 8.0\bin
```
```SQL
# see all users
SELECT USER FROM MySQL.user;
CREATE DATABASE DB
CREATE USER 'reem'@'localhost' IDENTIFIED BY '1234';
GRANT ALL PRIVILEGES ON DB.* TO 'reem'@'localhost';
FLUSH PRIVILEGES;
DROP USER 'reem'@'localhost';
```

```SQL
# delete a table
DROP TABLE tablename
# generate commands to delete all tables
SELECT concat('DROP TABLE IF EXISTS `', table_name, '`;')
FROM information_schema.tables
WHERE table_schema = 'MyDatabaseName';
```
Then start a database server using the workbench.
Enter the sql server
```Python
def create_db_connection(host_name, user_name, user_password, db_name):
    connection = None
    try:
        connection = mysql.connector.connect(
            host=host_name,
            user=user_name,
            passwd=user_password,
            database=db_name
        )
        print("MySQL Database connection successful")
    except Error as err:
        print(f"Error: '{err}'")

    return connection
```

execute a query 
```Python
def execute_query(connection, query):
    cursor = connection.cursor()
    try:
        cursor.execute(query)
        connection.commit()
        print("Query successful")
    except Error as err:
        print(f"Error: '{err}'")
```

add a table, a string in a specific structure that is send to the `execute_query` function.
```Python
create_teacher_table = """
CREATE TABLE teacher (
  teacher_id INT PRIMARY KEY,
  first_name VARCHAR(40) NOT NULL,
  last_name VARCHAR(40) NOT NULL,
  language_1 VARCHAR(3) NOT NULL,
  language_2 VARCHAR(3),
  dob DATE,
  tax_id INT UNIQUE,
  phone_no VARCHAR(20)
  );
 """
```
Main
```Python
connection = create_db_connection("localhost", "reem", 12, "my_db")
execute_query(connection, create_client_table)
```
# SQL alchemy 
Objects with realations, and queries to get those objects. This supposed to be more efficient

#### Object-Relational-Mapper ORM
translates Python classes to tables on relational databases
and automatically converts function calls to SQL statements.

### Structure
SQLalchemy consist from the core and the ORM. <br>
- core: a basic toolkit for SQL
- ORM: can be build on the core.

### [Code Examples](https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91)
Note: you can use `panda` library to print the output
```Python
import sqlalchemy as db
engine = db.create_engine('dialect+driver://user:pass@host:port/db')
connection = engine.connect()
metadata = db.MetaData()
emp = db.Table('emp', metadata,
              db.Column('Id', db.Integer()),
              db.Column('name', db.String(255), nullable=False),
              db.Column('salary', db.Float(), default=100.0),
              db.Column('active', db.Boolean(), default=True)
              )

metadata.create_all(engine) #Creates the table
```
Quering
```Python
# Equivalent to `SELECT * FROM my_table`
query =sqlalchemy.select([my_table])  
# a list of tuples of 
answer = connection.execute(query).fetchall()
```
Fetching a long object
```Python
ResultProxy = connection.execute(query)
while flag:
    partial_results = ResultProxy.fetchmany(50)
    if(partial_results == []): 
	flag = False
    //
	code
   //
ResultProxy.close()
```
Insert data
```Python
query = db.insert(emp).values(Id=1, name='naveen', salary=60000.00, active=True)
ResultProxy = connection.execute(query)
```

Update table values
```Python
db.update(table_name).values(attribute = new_value).where(condition)
```

Delete a table (cleas the table)
```Python
db.delete(table_name).where(condition)
```

Drop a table (remove the table completely)

```Python
table_name.drop(engine) #drops a single table
metadata.drop_all(engine) #drops all the tables in the database
```

```Python
ssl_args = {'ssl_ca': ca_path}
engine = create_engine("mysql+mysqlconnector://<user>:<pass>@<addr>/<schema>",
                        connect_args=ssl_args)
```


# SQL in C++

