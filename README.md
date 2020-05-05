# Tutorial: GnuPG
**GNU Privacy Guard** (GnuPG o GPG) es una herramienta de cifrado y firmas digitales desarrollado por Werner Koch, que viene a ser un reemplazo del **PGP** (Pretty Good Privacy) pero con la principal diferencia que es Software Libre licenciado bajo licencia GPLv3. **GPG** utiliza el estándar del IETF denominado **OpenPGP**.

Para seguir los pasos de esta guía primero debes tener instalado en tu sistema **GnuPG**.
```bash
~$ sudo apt install gnupg
```

## Cifrado simétrico de ficheros
A continuación explico de forma muy breve cómo se cifra un archivo cualquiera empleando cifrado simétrico.

1. Para este ejemplo primero crearemos un archivo `prueba.txt` que contendrá la cadena de texto "*Hola*".
    ```bash
    ~$ echo "Hola" > prueba.txt
    ~$ cat prueba.txt
    Hola
    ```

2. Si queremos cifrar el archivo `prueba.txt` con un *passphrase* ejecutamos el siguiente comando sobre el archivo:
    ```bash
    ~$ gpg -o prueba.gpg -c prueba.txt
    ```
Nos pedirá insertar un *passphrase* para su cifrado.

3. Debemos recordar este *passphrase* para luego descifrar nuestro archivo. La opción `-o` es para indicar el archivo de salida ya cifrado, y la opción `-c` es para indicar que se va a realizar un cifrado simétrico (por defecto AES128). Si se quisiera cambiar el tipo de cifrado se puede sustituir la opción `-c` por `--cipher-algo` y a continuación especificar el tipo de cifrado, por ejemplo:
    ```bash
    ~$ gpg -o prueba.gpg --cipher-algo AES256 prueba.txt
    ```
Los algoritmos de cifrado simétrico disponibles son: `IDEA`, `3DES`, `CAST5`, `BLOWFISH`, `AES`, `AES192`, `AES256`, `TWOFISH`, `CAMELLIA128`, `CAMELLIA192` y `CAMELLIA256`.

4. Una vez hecho esto, se puede listar el contenido del directorio actual para ver lo que se ha generado.
    ```bash
    ~$ ls -lrt
    -rw-rw-r-- 1 jdg jdg     5 may  5 17:53 prueba.txt
    -rw-rw-r-- 1 jdg jdg    85 may  5 17:53 prueba.gpg
    ```

5. Si queremos ver qué contiene el archivo `prueba.gpg` generado veremos que ya no tiene el texto en claro y está cifrado.
    ```bash
    ~$ cat prueba.gpg
    ??K0pF?%<??Z?8??>??Tgh???_u???O?
    ????8a?
    ```

6. Ahora ya podemos guardar para nosotros mismos o hacer llegar el archivo a una persona que conozca la *passphrase* para descifrarlo, de un modo seguro y fiable. Para descifrar el archivo bastaría con ejecutar el siguiente comando:
    ```bash
    ~$ gpg -d prueba.gpg
    gpg: datos cifrados AES
    gpg: cifrado con 1 frase contraseña
    Hola
    ```
Nos pedirá insertar un *passphrase* para descifrarlo, y si lo introducimos correctamente aparecerá el mensaje en claro que está en el archivo original.

Si lo que se quiere cifrar es un conjunto de archivos y directorios bastaría con empaquetarlos y/o comprimirlos en un archivo, por ejemplo `.tar`, `.zip` `.gz`, etc, y repetir el proceso de esta guía.

## Cifrado de ficheros con funciones hash


## Cifrado asimétrico de ficheros con clave pública
1. Para cifrar un archivo con clave pública se ha de ejecutar el siguiente comando:
    ```bash
    ~$ gpg -e -u ​"mi identificador"​ -r ​ "el del destinatario"​ prueba.txt
    ```
