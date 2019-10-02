# NSUrlSession_playground
Изучаем NSUrlSession

### URLSessionConfiguration 
Может быть трех типов: 

* default
* ephemeral (не хранит куки, credentials, кэш)
* background

[Документация](https://developer.apple.com/documentation/foundation/nsurlsessionconfiguration)

Попытка изменить уже существующую сессию: 

```swift
let sharedSession = URLSession.shared // Singleton session obj
sharedSession.configuration.allowCellularAccess = false 
// ничего не изменит, т.к. нужно сконфигурировать конфигурацию до
// добавления в сессию.
```

Пример создания различных типов конфигураций сессии: 

```swift
let myDefaultConfiguration = URLSessionConfiguration.default
let myEConfiguration = URLSessionConfiguration.ephemeral
let myBConfiguration = URLSessionConfiguration.background(withIdetifier: "com.denis.nefedov")

myDefaultConfiguration.allowCellularAccess = false // теперь изменения будут приняты
myDefaultConfiguration.waitsForConnectivity = true // Apple recomendation

// создаем новую сессию со своими настройками
let myDefaultSession = URLSession(configuration: myDefaultConfiguration)

// если не хотим ничего менять в конфигурации, то можно создать дефолтную сессию
let defaultSession = URLSession(configuration: .default)
```

### URLSessionTasks

* URLSessionDataTask - response in memory, not supported in background sessions
* URLSessionUploadTask - easier to provide request body
* URLSessionDownloadTask - response written to file on disk


### Взаимодействие с REST API

[Тестовый REST API Server](http://jsonplaceholder.typicode.com)

Начнем с нашей конфигурации:

```swift
let dummyConfiguration = URLSessionConfiguration.default
dummyConfiguration.waitsForConnectivity = true
let mySession = URLSession(configuration: dummyConfiguration)
```

Дальше создадим таск:

```swift

let url = URL(string: "http://jsonplaceholder.typicode.com/users")!
let dataTask = mySession.dataTask(with: url) {
    (data, response, error) in
    guard let httpResponse = response as? HTTPURLResponse, httpResponse.statusCode == 200 else {return}
    
    guard let data = data else {
        print(error.debugDescription)
        return
    }
    
    if let result = NSString(data: data, encoding: String.Encoding.utf8.rawValue) {
        print(result)
    }
        
}

dataTask.resume() // по-умолчанию таски находятся в suspended состоянии. 
```

Получаем следующий вывод:

```bash
[
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "suite": "Apt. 556",
    "city": "Gwenborough",
    "zipcode": "92998-3874",
    "geo": {
      "lat": "-37.3159",
      "lng": "81.1496"
    }
  },
  "phone": "1-770-736-8031 x56442",
  "website": "hildegard.org",
  "company": {
    "name": "Romaguera-Crona",
    ...
```
