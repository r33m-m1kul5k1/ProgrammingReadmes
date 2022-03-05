# coding-self-learing
# Structured Query Language (SQL)
Tables with rows and colums that can be access with queries.
To actully run SQL server on windows you need to install two tools:
1. SQL server
2. SQL Server Management<br>
[Here is a guid on how to download these](https://www.youtube.com/watch?v=RSlqWnP-Dy8&t=28s)<br>
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
