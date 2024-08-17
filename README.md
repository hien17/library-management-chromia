```shell
brew services start postgresql@16
createuser -s postgres
```

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


```shell
chr --help
chr create-rell-dapp
```

```shell
chr node start
chr node start --wipe
```

```shell
chr tx --await create_library "Lib1" "Street1" "City1" "PostalCode1"
```

```shell
chr query --blockchain-rid <blockchain-rid> <query-name>

E.g.
chr query --blockchain-rid 2E5C798460AE0709930D95375F80049B22141E6C9E685DD49972AED01C3916F8 hello_world
```
```shell

curl -X GET 'localhost:7740/query/2E5C798460AE0709930D95375F80049B22141E6C9E685DD49972AED01C3916F8?type=hello_world'
```

```shell
chr keygen
brew update
brew upgrade
brew install chromia/core/chr@<version>
brew unlink chr
brew link chr@<version>
```

# Common Errors

When you see 400 error with bad request , it means that your rell code has problem so you can't make a operation or query.

Remember rell entities, operations and queries must be put in module of main.rell or if you put them in another folder, you must import that module into main module