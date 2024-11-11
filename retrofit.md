# Retrofit
Retrofit es una biblioteca disponible en Kotlin, que nos facilita la utilizacion de APIs REST, al ayudarnos con la creacion de solicitudes HTTP y brindando un manejo mas sencillo de las respuestas.

## Agregar Dependencias
Primero debemos agregar las dependencias necesarias al proyecto, para ellos debemos modificar libs.versions.toml y build.gradle.kts.
```plaintext
├── myapplication
│   ├── .gradle
│   ├── .idea
│   ├── .kotlin
│   ├── app
│   │   ├── src
│   │       ├── main
│   │       │   ├── java
│   │       │   │   └── com
│   │       │   │       └── example
│   │       │   │           └── myapplication
│   │       │   │               └── MainActivity.kt
│   │       └── res
│   │           └── layout
│   │               └── activity_main.xml
│   ├── gradle
│   │   └── wrapper
│   │   └── libs.versions.toml
│   └── build.gradle.kts
└── settings.gradle
```
En libs.version.kts

```kotlin
// Example of a Retrofit interface in Kotlin
interface ApiService {
    @GET("users/{user}/repos")
    fun listRepos(@Path("user") user: String): Call<List<Repo>>
}
```