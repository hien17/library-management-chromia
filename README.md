# Library management in Chromia
## Setup database 
We need to setup PostgresQL before starting our own blockchain
```shell
brew services start postgresql@16
brew services restart postgresql@16
brew services start postgresql
brew services restart postgresql
createuser -s postgres
```
## Create database, create role and grant role 
```shell
sudo locale-gen en_US.UTF-8
psql -U postgres -c "CREATE DATABASE postchain WITH TEMPLATE = template0 LC_COLLATE = 'en_US.UTF-8' LC_CTYPE = 'en_US.UTF-8' ENCODING 'UTF-8';" -c "CREATE ROLE postchain LOGIN ENCRYPTED PASSWORD 'postchain'; GRANT ALL ON DATABASE postchain TO postchain;"
```
```shell
sudo locale-gen C.UTF-8
psql -U postgres -c "DROP DATABASE postchain;"
psql -U postgres -c "DROP ROLE postchain;"
psql -U postgres -c "CREATE DATABASE postchain WITH TEMPLATE = template0 LC_COLLATE = 'C.UTF-8' LC_CTYPE = 'C.UTF-8' ENCODING 'UTF-8';" -c "CREATE ROLE postchain LOGIN ENCRYPTED PASSWORD 'postchain'; GRANT ALL ON DATABASE postchain TO postchain;"
```
## Create a dapp 

```shell
chr --help
chr create-rell-dapp
```
Or we can define the path detailedly. This command line will rename monetization-dapp-chromia instead of default name (my-rell-dapp) with path of directory is ./
```shell
chr create-rell-dapp monetization-dapp-chromia -d ./
```
## Start node to run our own blockchain
We can start a blockchain that has clear data by adding --wipe
```shell
chr node start
chr node start --wipe
```
## Build & test code
```shell
chr build

chr test
``` 
## Do a transaction using Chromia CLI 
```shell
Usage: chr tx [OPTIONS] OPNAME [ARGS]...
Node 
    Target a test node 
    -brid, --blockchain-rid=TEXT  Target Blockchain RID  
    --cid=INT                     Target Blockchain IID   
    --api-url=TEXT                Target api url (default: http://localhost:7740)
Arguments 
    OPNAME  name of the operation to execute.
    ARGS    arguments to pass to the operation. (integer: 123) (big_integer: 1234L) (string: foo,"bar") (bytearray: will be encoded using the rell notation x"<myByteArray>" and will initially be interpreted as a hex-string.) (array: [foo,123]) (dict:["key1":value1,"key2":value2])
```
E.g.
```shell
chr tx --await create_library "Lib1" "Street1" "City1" "PostalCode1"

chr tx --await create_shelf "1" "Lib1"
```
## Query directly in our own blockchain
 We can query directly in the node that we run our own blockchain. \
E.g.
```shell
chr query get_all_library
```
## Query from a blockchain with its rid and method name
```shell
chr query --blockchain-rid <blockchain-rid> <query-name>

E.g.
chr query --blockchain-rid 2E5C798460AE0709930D95375F80049B22141E6C9E685DD49972AED01C3916F8 hello_world
```
## Query by curl
```shell
curl -X GET 'localhost:7740/query/2E5C798460AE0709930D95375F80049B22141E6C9E685DD49972AED01C3916F8?type=hello_world'
```
## Create key pair by Chromia CLI
```shell
chr keygen
```
## Chromia core
 Sometime, we need to update core of chromia or switch version to get rid of errors
```shell
brew update
brew upgrade
brew install chromia/core/chr@<version>
brew unlink chr
brew link chr@<version>
```

# Common Errors

When you see 400 error with bad request , it means that your rell code has problem so you can't make a operation or query.

Remember rell entities, operations and queries must be put in module of main.rell or if you put them in another folder, you must import that module into main module