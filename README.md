# odiva
Other Damn insecure and vulnerable App

This project is a Android Studio Project, so it's ready for open it and run it.

Contains 6 challenges:

## Insecure Data Storage I ##
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
