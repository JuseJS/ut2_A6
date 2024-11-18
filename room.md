# Persistencia de Datos con Room en Android Kotlin

En este codelab, nos enseñan como implementar la persistencia de datos en una App de Android, utilizando Room y Jetpack Compose. Room es una libreria que nos facilita el trabajar con SQLite, faciliantando el acceso a los datos.

## Pasos principales
### 1. Configuración Inicial
Primero debemos agregar las dependencias necesarias al proyecto, para ellos debemos modificar `libs.versions.toml` y `build.gradle.kts`.

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

```kotlin
// libs.versions.toml
[versions]
roomRuntime = "2.7.0-alpha11" // Debemos utilizar la version alpha, debido a que la version estable no funciona correctaemnte en Kotlin 2.0.0
ksp = "2.0.0-1.0.21"

[libraries]
androidx-room-runtime = { module = "androidx.room:room-runtime", version.ref = "roomRuntime" }
androidx-room-ktx = { module = "androidx.room:room-ktx", version.ref = "roomRuntime" }
androidx-room-compiler = { module = "androidx.room:room-compiler", version.ref = "roomRuntime" }


// build.gradle.kts
dependencies {
    implementation(libs.androidx.room.runtime)
    implementation(libs.androidx.room.ktx)
    ksp(libs.androidx.room.compiler)
}
```

### 2. Definición del Modelo de Datos
Creamos una clase de datos con anotaciones, para definir una entidiad de Room:

```kotlin
@Entity(tableName = "item")
data class Item(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val name: String,
    val price: Double,
    val quantity: Int
)
```

### 3. Creación del DAO
Definimos un Data Acces Object (DAO) con las consultas necesarias usando anotaciones:

```kotlin
@Dao
interface ItemDao {
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(item: Item)

    @Update
    suspend fun update(item: Item)

    @Delete
    suspend fun delete(item: Item)

    @Query("SELECT * from items WHERE id = :id")
    fun getItem(id: Int): Flow<Item>

    @Query("SELECT * from items ORDER BY name ASC")
    fun getAllItems(): Flow<List<Item>>
}
```

### 4. Configurar la Base de Datos
Configuramos una clase que representa la BBDD:

```kotlin
@Database(entities = [Item::class], version = 1, exportSchema = false)
abstract class ItemRoomDatabase : RoomDatabase() {
    abstract fun itemDao(): ItemDao

    companion object {
        @Volatile
        private var INSTANCE: ItemRoomDatabase? = null

        fun getDatabase(context: Context): ItemRoomDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    ItemRoomDatabase::class.java,
                    "item_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

### 5. Integración con Jetpack Compose
Mostrar los datos guardados en la pantalla, usando `collectAsState` para actualizar los datos en tiempo real.

```kotlin
@Composable
fun ItemListScreen(itemViewModel: ItemViewModel = viewModel()) {
    val items by itemViewModel.allItems.collectAsState(initial = emptyList())
    LazyColumn {
        items(items) { item ->
            Text(text = item.name)
        }
    }
}
```

## Conclusión
El codelab nos guia paso a paso en la creacion de una app de ejemplo que lo que pretende es enseñarnos de forma basica y sencilla a crear una App de android que implemente de forma correcta Room.