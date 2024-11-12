# SQLite
SQLite es una biblioteca de software que implementa un sistema de gestión de bases de datos relacional, ligera y de código abierto. No requiere un servidor separado para funcionar, ya que almacena toda la base de datos en un solo archivo local.

## Uso de SQLite en Kotlin Android
Para usar SQLite en una aplicación Android con Kotlin, sigue estos pasos básicos:

### Crear una clase que extienda `SQLiteOpenHelper`:
```kotlin
class DBHelper(context: Context) : SQLiteOpenHelper(context, DATABASE_NAME, null, DATABASE_VERSION) {

    override fun onCreate(db: SQLiteDatabase) {
        // Crear tabla
        val createTable = ("CREATE TABLE $TABLE_NAME ("
                + "$ID_COL INTEGER PRIMARY KEY AUTOINCREMENT, "
                + "$NAME_COL TEXT)")
        db.execSQL(createTable)
    }

    override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) {
        // Actualizar la base de datos si es necesario
        db.execSQL("DROP TABLE IF EXISTS $TABLE_NAME")
        onCreate(db)
    }

    // Método para agregar una persona
    fun agregarPersona(nombre: String) {
        val values = ContentValues().apply {
            put(NAME_COL, nombre)
        }
        val db = this.writableDatabase
        db.insert(TABLE_NAME, null, values)
        db.close()
    }

    // Método para leer todas las personas
    fun leerPersonas(): List<String> {
        val listaNombres = mutableListOf<String>()
        val db = this.readableDatabase
        val cursor = db.query(TABLE_NAME, arrayOf(NAME_COL), null, null, null, null, null)

        with(cursor) {
            while (moveToNext()) {
                val nombre = getString(getColumnIndexOrThrow(NAME_COL))
                listaNombres.add(nombre)
            }
            close()
        }
        db.close()
        return listaNombres
    }

    companion object {
        private const val DATABASE_NAME = "personas.db"
        private const val DATABASE_VERSION = 1

        const val TABLE_NAME = "persona"
        const val ID_COL = "id"
        const val NAME_COL = "nombre"
    }
}
```

### Insertar y leer datos en la base de datos:

```kotlin
val dbHelper = DBHelper(context)

// Agregar una persona
dbHelper.agregarPersona("Juan Pérez")

// Leer personas
val personas = dbHelper.leerPersonas()
for (nombre in personas) {
    // Usar el nombre de la persona
    println(nombre)
}
```

Estos pasos te permiten crear y gestionar una base de datos SQLite en una aplicación Android usando Kotlin, mostrando cómo crear la base de datos, insertar y leer datos básicos sin funciones adicionales.