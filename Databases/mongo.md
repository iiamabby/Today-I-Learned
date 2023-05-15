# Mongo cli commands

> Return nested documents that match either query
```db.collection.find({$or: [{"depart.country:"US"}, {"depart.country":"BR"}]})```
> Returns all records appart from ones departing from the US
```db.collection.find({"depart.country": {$ne: "US"}}) ```