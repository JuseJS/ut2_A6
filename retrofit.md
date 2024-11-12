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
retrofit = "2.9.0"
gson = "2.10.1"

[libraries]
retrofit = { group = "com.squareup.retrofit2", name = "retrofit", version.ref = "retrofit" }
converter-gson = { group = "com.squareup.retrofit2", name = "converter-gson", version.ref = "retrofit" }


// build.gradle.kts
dependencies {
    implementation(libs.retrofit)
    implementation(libs.converter.gson)
}

```

## Ejemplo de Uso
### Crear la Interfaz del API

Primero, debemos definir una interfaz que describa los endpoints de la API.

```kotlin
// ApiService.kt
import retrofit2.Call
import retrofit2.http.GET

interface ApiService {
    @GET("posts")
    fun getPosts(): Call<List<Post>>
}
```

### Crear el Modelo de Datos

Luego debemos crear un data class para manejar correctamente los datos obtenidos.
```kotlin
// Post.kt
data class Post(
    val userId: Int,
    val id: Int,
    val title: String,
    val body: String
)
```

### Configurar Retrofit

Configuramos Retrofit en una clase singleton para que pueda ser reutilizado en toda la aplicación.

```kotlin
// RetrofitInstance.kt
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object RetrofitInstance {
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"

    val api: ApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(ApiService::class.java)
    }
}
```

### Realizar una Solicitud

Finalmente, probamos a realizar una solicitud a la API.

```kotlin
// MainActivity.kt
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val call = RetrofitInstance.api.getPosts()
        call.enqueue(object : Callback<List<Post>> {
            override fun onResponse(call: Call<List<Post>>, response: Response<List<Post>>) {
                if (response.isSuccessful) {
                    response.body()?.let { posts ->
                        for (post in posts) {
                            Log.d("MainActivity", "Post: ${post.title}")
                        }
                    }
                }
            }

            override fun onFailure(call: Call<List<Post>>, t: Throwable) {
                Log.e("MainActivity", "Error: ${t.message}")
            }
        })
    }
}
```
