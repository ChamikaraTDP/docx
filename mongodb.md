# Mongodb

## guidlines

## embed or not ?

- Is embeded data needed 80% of the time? then Yes
- Do you need embeded data without containing doc? & how often? If really often then No
- If embeded data grows large without limit, then No
- Embeded data should be small
- How varied the queries? If you need all kinds of different queries it's not good to embed
- Integration db or application db?  NoSql DBs not good for integration dbs



## common

- Document max size 16MB
- MongoDB save users on DB level 
- Relationships enforced in the application
- 




## docker setup

### Install mongo container

```bash
sudo docker run --name mongo-db --network mongo-net -d mongo
```

### start interactive shell

```bash
sudo docker exec -it <cont_id> mongosh <db>

sudo docker exec -it <cont_id> mongosh <db> - u <username> -p <password>


sudo docker exec -it 9c4hasfsh21k2 mongosh test

```




## DB management

### create a new database (or use an existing one)

```
use <db_name>

use test
```

### create new collection

```
db.createCollection("<col_name>")


db.createCollection("school")
```

### create index

```
# sort:  1  ascending
# sort:  -1  descending

db.school.createIndex({ y: 1 })
```



## CURD


### insert doc

```
db.<col_name>.insertOne({ x: 2, y: "a" })

db.school.insertOne({ x: 2, y: "a" })
```

### update doc

```
```


### read doc

```
# find all

db.school.find()


# find one
# sort: ( 1  ascending )
# sort: ( -1  descending )


db.school.find({ x: 2, y: "a" }).sort({ y: 1 }).limit(1)


# operators

db.school.find({ x: { $gt: 1 }})



```

