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
En `libs.versions.toml` debemos agregar las versiones de los paquetes necesarios y nombrar las librerías. Luego, añadimos las dependencias en `build.gradle.kts`.

```kotlin
// libs.versions.toml
[versions]
activity-compose = "1.7.2"
lifecycle = "2.6.2"
retrofit = "2.9.0"
gson = "2.10.1"

[libraries]
androidx-activity-compose = { group = "androidx.activity", name = "activity-compose", version.ref = "activity-compose" }
androidx-lifecycle-viewmodel-ktx = { group = "androidx.lifecycle", name = "lifecycle-viewmodel-ktx", version.ref = "lifecycle" }
retrofit = { group = "com.squareup.retrofit2", name = "retrofit", version.ref = "retrofit" }
converter-gson = { group = "com.squareup.retrofit2", name = "converter-gson", version.ref = "retrofit" }


// build.gradle.kts
dependencies {
    implementation(libs.androidx.activity.compose)
    implementation(libs.androidx.lifecycle.viewmodel.ktx)
    implementation(libs.retrofit)
    implementation(libs.converter.gson)
}

```