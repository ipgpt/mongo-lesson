# mongo-lesson

## Накатить бэкап базы
*input*
```
mongodump
```
*output*
```
2018-05-14T22:34:58.970+0300    writing users.system.indexes to
2018-05-14T22:34:59.373+0300    done dumping users.system.indexes (1 document)
2018-05-14T22:34:59.381+0300    writing users.users to
2018-05-14T22:35:00.120+0300    done dumping users.users (844 documents)
```

## Найти средний возраст людей в системе
*input*
```
db.users.aggregate({$group:{_id:null,avgAge:{$avg:"$age"}}})
```
*output*
```
{ "_id" : null, "avgAge" : 30.38862559241706 }
```

## Найти средний возраст в штате Аляска
*input*
```
db.users.aggregate([{$match:{address:{$regex:".*Alaska.*"}}},{$group:{_id:null,alaskaAvgAge:{$avg:"$age"}}}])
```
*output*
```
{ "_id" : null, "alaskaAvgAge" : 31.5 }
```

## Начиная от Math.ceil(avg + avg_alaska) найти первого человека с другом по имени Деннис
*input*
```
db.users.findOne({$and:[{"index":{$gt:Math.ceil(30.38862559241706+31.5)}},{"friends.name":/Dennis/}]},{_id:0,name:"$name",address:"$address"})
```
*output*
```
{
       "name" : "Esperanza Blevins",
       "address" : "659 Oceanic Avenue, Collins, Utah, 6277"
}
```

## Найти людей из того же штата, что и предыдущий человек и посмотреть какой фрукт любят больше всего в этом штате (аггрегация)
