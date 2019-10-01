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
