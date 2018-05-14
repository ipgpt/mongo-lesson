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
db.users.findOne({$and:[{"index":{$gt:Math.ceil(30.38862559241706+31.5)}},{"friends.name":/Dennis/}]},{_id:1,name:"$name",address:"$address"})
```
*output*
```
{
        "_id" : ObjectId("5adf3c1544abaca147cdd47c"),
        "name" : "Esperanza Blevins",
        "address" : "659 Oceanic Avenue, Collins, Utah, 6277"
}
```
## Найти людей из того же штата, что и предыдущий человек и посмотреть какой фрукт любят больше всего в этом штате
*input*
```
db.users.aggregate({$match:{"address":{$regex:"Utah"}}},{$group:{_id:"$favoriteFruit",amount:{$sum:1}}})
```
*output*
```
{ "_id" : "strawberry", "amount" : 3 }
{ "_id" : "apple", "amount" : 6 }
{ "_id" : "banana", "amount" : 6 }
```
## Найти саммого раннего зарегистрировавшегося пользователя с таким любимым фруктом
*input*
```
db.users.findOne({"favoriteFruit": "apple"},{name:"$name"})
```
*output*
```
{ "_id" : ObjectId("5adf3c1544abaca147cdd34b"), "name" : "Macias Shannon" }
```
## Добавить этому пользователю свойство: { features: 'first apple eater' }
*input*
```
db.users.update({"_id":ObjectId("5adf3c1544abaca147cdd34b")},{$set:{"features":"first apple eater"}}
```
*output*
```
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
## Удалить всех любителей клубники
