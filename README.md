# Seguridad
Seguridad Logica

**2) Configuración de Contraseñas Seguras en Windows y Linux**
**Para encontrar las directivas de contraseña en Windows Server nos iremos a:**

Administrador del servidor >> Herramientas >> Directiva de seguridad local >> Directivas de cuenta >> Directiva de contraseña
En la siguiente imagen muestro lo que aparecería una vez hemos accedido a esa ruta:
![image](https://github.com/rcarper023/Seguridad/assets/146191521/821335b5-fe52-4a9b-a4c0-ae30067a0ebb)

A continuación explicaré todas las directivas que nos vamos a encontrar en este apartado.
Almacenar contraseñas con cifrado reversible: Esta directiva proporciona compatibilidad para aplicaciones que usan protocolos que exigen el conocimiento de la contraseña del usuario para fines de autenticación. Esta directiva debe de estar siempre deshabilitada.
Exigir historial de contraseñas: Esta directiva determina el número de nuevas contraseñas únicas que deben de asociarse a una cuenta. Esto permite a los administradores mejorar la seguridad, ya que garantiza que no se reutilicen continuamente contraseñas antiguas.
La contraseña debe cumplir los requisitos de complejidad: Esta directiva de complejidad al habilitarla nos obliga a tener los siguientes requisitos mínimos:
-	Tener una longitud mínima de seis caracteres.
-	Mayúsculas (de la A a la Z)
-	Minúsculas (de la a a la z)
-	Dígitos de base 10 (del 0 al 9)
-	Caracteres no alfanuméricos (por ejemplo, ¡, $, #, %)
Estos requisitos de complejidad se exigen al cambiar o crear contraseñas.
Longitud mínima de la contraseña: Esta directiva determina el número mínimo de caracteres que debe contener la contraseña de un usuario. Valor comprendido entre 1 y 20.
Vigencia máxima de la contraseña: Esta directiva determina el tiempo que un usuario puede usar una contraseña.
Vigencia mínima de la contraseña: Esta directiva determina el periodo de tiempo en que puede usarse una contraseña. Si está en 0 no caducará nunca, pero si establecemos otro número (siempre debe de ser menor que la vigencia máxima) de forma predeterminada se establece en 1, cuando llegue a la vigencia máxima la contraseña expirará y tendremos que crear una nueva contraseña.

Para la configuración de las directivas de complejidad de nuestras contraseñas haremos lo siguiente:
Habilitaremos que la contraseña debe de cumplir los requisitos de complejidad

![image](https://github.com/rcarper023/Seguridad/assets/146191521/d9e71686-5137-49aa-b9e0-5897ba530010)

Tendrá una longitud mínima de 8 caracteres

![image](https://github.com/rcarper023/Seguridad/assets/146191521/39c9f93e-7b88-4133-8d01-f8d9d7b7001d)

Tendrá una vigencia máxima de 90 días

![image](https://github.com/rcarper023/Seguridad/assets/146191521/0203b0c0-340d-4ddb-9896-4a68fa7661a7)

Tendrá una vigencia mínima de 1 día para habilitar la vigencia máxima.
![image](https://github.com/rcarper023/Seguridad/assets/146191521/9fe39d4a-103f-4ad3-8485-89f0e8d6ab76)

Con estas modificaciones ya tendremos nuestra configuración básica para las directivas de complejidad de las contraseñas que vamos a utilizar.

Para Ubuntu haremos lo siguiente:
Accedemos al directorio /etc/ y buscaremos el archivo login.defs

![image](https://github.com/rcarper023/Seguridad/assets/146191521/7fdeaee8-bb79-4e8a-8fe6-f749df1a164f)

Accedemos al fichero para editarlo:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/c614999d-2daa-498c-b538-63da0720fae5)

Buscamos los parámetros PASS_MAX_DAYS Y PASS_MIN_DAYS para modificar los tiempos de utilización de la contraseña y los editamos.
En PASS_MIN_DAYS lo modificaremos a 1 y el PASS_MAX_DAYS lo modifcaremos a 90

![image](https://github.com/rcarper023/Seguridad/assets/146191521/9f1314e7-2b25-42e1-8ea3-8ba74501b299)

Ahora accederemos al directorio /pam.d/ y buscaremos el fichero common-password

![image](https://github.com/rcarper023/Seguridad/assets/146191521/e5a23f90-331e-435d-80fa-1a1e59b42e6d)

Accederemos al fichero para editarlo.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/34f6e5a4-0c8e-41c3-a7f6-28868333a763)

Este fichero habilita “yescrypt” que habilita contraseñas cifradas utilizando el algoritmo yescrypt.

**3) Ataques contra contraseñas en Sistemas Windows -Fichero SAM-**

Descargamos rainbowcrack de la página http://project-rainbowcrack.com/ 
En este caso he descargo el rainbowcrack1.8. Una vez descargado nos aparecerá una carpeta tal que así:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/f8921d0f-0e3f-4239-9ef3-35a8e0cef6b8)

Ahora generaremos la tabla rainbow. Para ello accedemos mediante el CMD al directorio de rainbow y ejecutaremos el siguiente comando:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/187b1083-b80c-4909-8c3f-dbdc3e84937d)

Esto nos generará el siguiente fichero con el conjunto de caracteres numérico.
Los parámetros utilizados son:
-	Lm: Que es el tipo de firma digital (lm, ntlm, md5 y shal)
-	Numeric: conjunto de caracteres definido en el fichero charset.txt
-	1: es la longitud mínima de la contraseña, y 7 es la máxima
-	0: es el índice de la tabla rainbow
-	All: es el sufijo que tendrá el fichero

La probabilidad de obtener la contraseña teniendo una tabla es del 60,05%, por eso hemos usado los siguiente valores para aumentar la probabilidad:
rtgen.exe lm numeric 1 7 0 2100 800000 all
rtgen.exe lm numeric 1 7 1 2100 800000 all
rtgen.exe lm numeric 1 7 2 2100 800000 all
rtgen.exe lm numeric 1 7 3 2100 800000 all
rtgen.exe lm numeric 1 7 4 2100 800000 all
Así generamos 5 tablas y aumentamos la probabilidad.
Tablas creadas

![image](https://github.com/rcarper023/Seguridad/assets/146191521/32f8c710-beec-4d93-836a-e6dbf61b48bd)

Una vez creadas las 5 tablas realizaremos el siguiente paso que es ordenarlas. Para ello usaremos el comando rsort <nombre de la tabla>.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/8f37509f-b884-4770-b29a-79548c0461e4)

Ahora abrimos el rcrack de forma gráfica y nos aparecerá una pantalla tal que así:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/79e22e6f-161c-45fa-ae04-0d9840fd9aab)

Ahora añadiremos las contraseñas hash. Para ello pincharemos en File>Add Hashes… y añadiremos las hash.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/8ddc8b24-51e8-460e-a3a9-def83bee4d71)

Ahora que hemos añadido las hash nos vamos a Rainbow Table>search rainbow table y seleccionaremos una de las tablas creadas anteriormente.
Una vez ejecutada la tabla rainbow si todo va bien debería de aparecer la contraseña en la columna de Plaintext.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/f639a462-ed09-42c1-9abc-ab1e41602a27)

**4) Ataques contra contraseñasen Sistemas Windows.**

**- Caso 1: Utilizar alguna de las herramientas indicadas en el tema para obtener las claves del fichero SAM de los usuarios de un S.O. Windows.**

Descargamos OPHCRACK del siguiente enlace:
https://ophcrack.sourceforge.io/download.php
Y también descargaremos en tablas la XP free small
https://ophcrack.sourceforge.io/tables.php
Una vez tengamos todo ejecutamos el programa y aparecerá una ventana tal que así:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/820ea862-982a-4865-aa25-0ff2465e92ac)

Ahora añadiremos un hash en load>single hash y añadimos el hash en la parte de abajo y pinchamos en OK.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/0caa8127-e0f7-4777-a3f0-b4a631c93937)

Ahora iremos a tables y pincharemos en install para agregar la tabla que hemos descargado anteriormente y nos saldrá algo tal que así:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/f5c2fb13-05c5-4654-8d2b-f962a43140c1)

Por último pinchamos en crack y nos aparecerá la contraseñas desencriptada:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/ad69787f-f763-431a-9b34-b046b685aa3b)

**- Caso 2: Utilizar Kali (Exploits Herramienta MetaSploit en línea de comandos y Armitage GUI) para acceder al fichero SAM de un Windows. A continuación utilizando alguna de las aplicaciones que proporciona esta distribución para craquearlas.**

**-Caso 3: Resetea la clave de administración utilizando alguna de las distribuciones LIVE indicadas en el tema**

**5) Ataques contra contraseñas en Sistemas Linux**
**Utiliza Backtrack y John The Ripper para descrubir las contraseñas encriptadas de un equipo Ubuntu.**

Para conseguir la contraseña de mi usuario en Kali Linux vamos a hacer lo siguiente:
Iniciaremos con el siguiente comando:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/5c6676af-d4bf-451e-b0a4-9cd12e720f47)

Este comando lo que hace es combinar dos ficheros en uno con “sudo unshadow”
Accedemos al directorio donde se encuentra el fichero donde hemos combinado passwd y shadow y lanzamos el siguiente comando.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/4b6c233a-c84a-4df6-81f6-9af67828c505)

Con este comando lo que hacemos para obtener la clave del usuario
Por último, haremos un john –show y el fichero que hemos creado anteriormente para mostrar la contraseña:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/4a0fd823-366f-462c-9d18-619f834df20f)

**6) Realiza un listado de este tipo de herramientas y analiza la instalación y configuración de 2 congeladores.**

-	Deep Freeze: es un administrador del núcleo que protege la integridad del disco duro redirigiendo la información que se va a escribir en el disco duro o partición protegida, dejando la información original intacta. El funcionamiento es muy sencillo, protege las unidades del ordenador donde se encuentre instalado, impidiendo que se pueda guardar ningún tipo de archivo, y por tanto se presenta como una barrera clara para que los virus u otros archivos malignos queden instalados en el ordenador.

-	Toolwiz Time Freeze es una herramienta de seguridad que te permitirá probar programas y realizar experimentos en Windows sin miedo a perder datos o la estabilidad del sistema. Esta herramienta también permite bloquear archivos y carpetas con contraseña para que no puedas borrarlos accidentalmente o por culpa de una aplicación o programa dañino.

-	Reboot Restore Rx es la herramienta perfecta a la hora de usar ordenadores que sean de libre acceso al público como en bibliotecas, aulas de informática, etc. Ya que esta utilidad nos permite deshacer todos los cambios que se hayan realizado durante su uso al reiniciar Windows.

-	SmartShield crea un espacio virtual en el que todos los cambios no deseados, incluida las infecciones por malware y ransomware, se eliminarán al reiniciar, liberando al usuario de peligros complejos al realizar sus habituales tareas diarias sin comprometer la integridad del sistema

Podemos encontrar también en window:
•	Clean Slate de Fortres Grand
•	SafeShield de Filestream
•	Rextore de Crucial Solutions
•	Shadow Defender de SD
En Mac OS
•	Deep Freeze
•	SmartShield
En Linux
•	Dafturn Ofris de Deffa
•	FSProtect de V13
•	Lethe de Lihuen
En esta práctica vamos a mostrar la instalación de las siguientes herramientas:
Reboot Restore RX3
Iniciamos la instalación:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/ef37512a-0d8b-458a-a631-e2eead8e40fd)

Seleccionamos la partición o disco que queremos proteger:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/03af22b3-c3bd-4b10-9b7c-ca7c5c3dd5cf)

Reiniciamos para hacer efectiva la instalación:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/fc3b6fd6-7164-4aaf-8c35-3854cd9a0af0)

Si tenemos la herramienta activada, a partir de ahora cada vez que reiniciemos el sistema volverá al estado en el que estaba justo cuando se había instalado la herramienta.

Toolwiz Time Freeze
Iniciamos la instalación:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/ee56a45a-f3f6-4127-837d-a1624782af31)

Dejamos los siguientes ajustes y pinchamos en next e install.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/91705267-1418-45d6-bf15-4771bad72df0)

Reiniciamos para hacer efectiva la instalación.

![image](https://github.com/rcarper023/Seguridad/assets/146191521/86e672fd-5126-46a5-8820-e557c0fc23b5)

Configuraciones importante:

![image](https://github.com/rcarper023/Seguridad/assets/146191521/c137cc32-22ff-42c3-94b7-5184b326be9f)

Si pinchamos en start time Freeze, iniciaremos la herramienta

![image](https://github.com/rcarper023/Seguridad/assets/146191521/8a77d02e-d212-42ff-9f91-b76a2eb18813)

En esta imagen muestro que ya está la herramienta iniciada y lo siguiente es si queremos que se inicie la herramienta cada vez que se inicia el sistema.

La última parte es si queremos excluir algún directorio o partición cuando la herramienta esté iniciada.
