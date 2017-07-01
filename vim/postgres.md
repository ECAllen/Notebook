
### connect
sudo -u postgres psql
\q

### connect to database
\connect <name>

### create new role
createuser --interactive

### create new database
createdb blah

### see tables
\d

### describe table
\d+ <table name>;

### reset password
\password postgres

### list databases
\l

### drop a table 
drop table <table name>;
### set column default value
alter table <table name> alter column <col name> set default false;
### alter column type 
alter table <table name> alter column <col name> type text;
alter table trends alter column publish type text;
