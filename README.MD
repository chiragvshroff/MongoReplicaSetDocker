# MONGO REPLICA SET WITH AUTHENTICATION USING DOCKER

Mongo Cluster Using 4 Nodes

### Run The Cluster:
```
docker-compose up
```

### Connect To The Primary Node
```
docker-compose exec Mongo-Primary mongo -u "example" -p "password123"
```

### Initiate The Replica Set
```
rs.initiate({"_id" : "Set-Name","members" : [{"_id" : 0,"host" : "Mongo-Primary:27017"},{"_id" : 1,"host" : "Mongo-1:27018"},{"_id" : 2,"host" : "Mongo-2:27019"},{"_id" : 3,"host" : "Mongo-3:27020"}]});
```

### Set The Priority Of The Master Node
```
conf = rs.config();
conf.members[0].priority = 2;
rs.reconfig(conf);
```

### Create a Cluster Admin
```
use admin;
db.createUser({user: "cluster_admin",pwd: "password",roles: [ { role: "userAdminAnyDatabase", db: "admin" },  { "role" : "clusterAdmin", "db" : "admin" } ]});
db.auth("cluster_admin", "password");
```

### Create a Collection On the Database
```
use my_data;
db.createUser({user: "my_user",pwd: "password",roles: [ { role: "readWrite", db: "my_data" } ]});
db.createCollection('my_collection');
```

### Verify Credentials
```
docker-compose exec tut12-mongo-primary mongo -u "my_user" -p "password" --authenticationDatabase "my_data"
```

### Stop The Cluster
```
docker-compose down
```