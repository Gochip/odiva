# odiva
Other Damn insecure and vulnerable App

This project is a Android Studio Project, so it's ready for open it and run it.

Contains 6 challenges:

## Insecure Data Storage I: Shared Preferences ##
Android ofrece un objeto llamado `SharedPreferences` para almacenar datos en formato `clave-valor`. Sin embargo, almacenar datos sensibles en este Storage no es recomendable porque en general es de fácil acceso.
Como aprenderemos en este challenge, las SharedPreferences son almacenadas en un archivo xml (sin cifrar):

/data/data/[paquete_app]/shared_prefs/[archivo_preferences].xml

Si necesitás usar sí o sí las Shared Preferences (normalmente el motivo principal es por su simplicidad de uso), podés utilizar una clase que ofrece el paquete `androidx.security.crypto` llamada `EncryptedSharedPreferences` y que básicamente usarla es tan simple como la clase `SharedPreferences`.

Referencia oficial de EncryptedSharedPreferences: https://developer.android.com/reference/androidx/security/crypto/EncryptedSharedPreferences.

Código de ejemplo de Shared Preferences:

```
SharedPreferences spref = PreferenceManager.getDefaultSharedPreferences(this);
SharedPreferences.Editor spedit = spref.edit();
EditText usr = (EditText) findViewById(R.id.edit_txt_user);
EditText pwd = (EditText) findViewById(R.id.edit_txt_password);

spedit.putString("user", usr.getText().toString());
spedit.putString("password", pwd.getText().toString());
spedit.commit();
```

### Pasos para superar el desafío ###
1. Ejecutar la app en algún emulador usando: `./emulator -avd Nexus_S_API_25`. Tener en cuenta que emulator es un script se encuentra en [HOME]/Android/Sdk/tools.
2. Abrir la app ODIVA y presionar el botón “Insecure Data Storage I”.
3. Ingresar un usuario y un password, y presionar en el botón “Login”. Esto hizo que tanto el usuario y el password se hayan guardado en Shared Preferences.
4. Ahora para visualizar estos datos sensibles, se debe ejecutar:

`./adb shell "run-as com.gochip.odiva cat /data/data/com.gochip.odiva/shared_prefs/com.gochip.odiva_preferences.xml" > ~/archivo_sp.xml`

5. Finalmente, vemos los datos con cat ~/archivo_sp.xml.



## Insecure Data Storage II: SQLite ##
SQLite es un Storage que se puede usar en cualquier aplicación Android para almacenar datos como tablas SQL. Hay que tener cuidado con almacenar datos sensibles sin cifrar en SQLite porque es de fácil acceso.


Ejemplo de código:

```
try {
   mDB = openOrCreateDatabase("ids2", MODE_PRIVATE, null);
   mDB.execSQL("CREATE TABLE IF NOT EXISTS myuser(user VARCHAR, password VARCHAR);");
}
catch(Exception e) {
   Log.d("Diva", "Error occurred while creating database: " + e.getMessage());
}


EditText usr = (EditText) findViewById(R.id.edit_txt_user);
EditText pwd = (EditText) findViewById(R.id.edit_txt_password);
try {
   mDB.execSQL("INSERT INTO myuser VALUES ('"+ usr.getText().toString() +"', '"+ pwd.getText().toString() +"');");
   mDB.close();
}
catch(Exception e) {
   Log.d("Diva", "Error occurred while inserting into database: " + e.getMessage());
}
```

### Pasos para superar el desafío ###
1. Ejecutar la app en algún emulador usando: `./emulator -avd Nexus_S_API_25`. Tener en cuenta que emulator es un script se encuentra en [HOME]/Android/Sdk/tools.
2. Abrir la app ODIVA y presionar el botón “Insecure Data Storage II”.
3. Ingresar un usuario y un password, y presionar en el botón “Login”. Esto hizo que tanto el usuario y el password se hayan guardado en SQLite.
4. Ahora para visualizar estos datos sensibles, se debe ejecutar:

```
$ ./adb root
$ ./adb shell
$ cd /data/data/com.gochip.odiva/databases
$ sqlite3 ids2
sqlite> select * from myuser
```
