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

