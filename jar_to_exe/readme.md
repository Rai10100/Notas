# CREACIÓN DE .EXE CON INSTALADOR DESDE .JAR CON JRE INCLUIDO
[launch]: http://launch4j.sourceforge.net/
[inno]: https://jrsoftware.org/isdl.php
## **Antecedentes**
* Quería envíar una aplicación sencilla de escritorio a otra persona. 
* Cuando se construye una aplicación sencilla con java, al final se tendrá por ejemplo un _calculadora.jar_ o _TablaRegistrosMySQL.jar_ que para ser ejecutado requiere tener instalado JRE (Java Runtime Environment).
* La otra persona no tiene instalado JRE.

## **Generando ejecutable**
A este punto ya se debe contar con el .jar funcionando.  
Te recomiendo copiar la carpeta que contenga este archivo y mandarla al escritorio para trabajar.  
Cuando es un proyecto en Ant por ejemplo se genera una carpeta llamada _dist_ y dentro viene el archivo y las librerías que ocupamos para dicho proyecto, en la imagen el proyecto es simple y no tiene ningún extra.  
También te recomiendo tener dos íconos preparados para agregarlos a nuestro .exe e instalador.

![jar](imgGit/jar.png)  

Ahora descargamos, instalamos y ejecutamos  [**Launch4j**][launch].

![launch](imgGit/launch4j.png) 

### **Configuramos:**  
**En la pestaña _Basic_ :**

1. Output file --> Es el archivo .exe ejecutable que se generará. Debemos proporcionar una ruta donde se guardará y un nombre que termine con la extensión .exe.  
Ejemplo: _C:\Users\Rai\Desktop\Games.exe_
2. Jar --> Es el archivo .jar que vamos a convertir en .exe. Proporcionamos la ruta del archivo.  
Ejemplo: _C:\Users\Rai\Desktop\dist\Games.exe_
3. Icon --> Podemos (o no) seleccionar un ícono que sea la presentación de nuestro ejecutable. Brindamos la ruta.  
Ejemplo: _C:\Users\Rai\Desktop\iconoEjecutable.ico_

**En la pestaña _JRE_ :** 

Hay dos caminos:  

* Descargar e instalar JRE en el Windows donde se ejecutará.
* Posteriormente incluir el JRE en el instalador para que vaya junto a la aplicación (Va a pesar mucho más).

Si no se elije alguno de estos dos caminos podemos obtener un error del tipo _this application requires a java runtime environment 1.X.X_.

Optaremos por la segunda opción, ya que aunque lo mejor es instalar JRE en la pc destino para que pueda interpretar este .exe el objetivo es hacer un instalador que genere un .exe que no dependa de tener instalado el JRE. Así que...

1. Bundled JRE path --> Es la ruta donde buscará el JRE (en cualquier pc donde se ejecute) que nosotros colocaremos con el instalador en la PC destino.  
Ejemplo: _C:\Program Files (x86)\Games\miJRE_

Nota 1: Si se opta por instalar directamente JRE en el Windows destino no es necesario colocar la ruta del JRE, sino únicamente llenar el campo de _Min JRE version_ específicando la versión de java que utilizamos para crear el proyecto, de esta manera el .exe sabe la versión mínima con la que se puede ejecutar correctamente.  
Nota 2: Con excepción de la selección del ícono, esta es la *configuración básica* y lo que Launch4j exige para poder generar un .exe, se pueden realizar más cambios como seleccionar una clase principal, ajustar la versión máxima soportada, entre otros. En este caso todos los demás ajustes se consideran innecesarios.
### **Generamos el ejecutable:**  
Súper fácil. Solo buscar el ícono que tiene un engranaje y que trae el tooltip _Build wrapper_.  
Al pulsarlo nos pedirá la ubicación para guardar un archivo de configuración .xml. Da igual donde lo guardemos.  
Cuando termine de construirlo tendremos nuestro archivo .exe, en este caso en el escritorio.  
## **Generando el instalador**
Descargamos, instalamos y ejecutamos [**Inno Setup**][inno].
Nos abrirá una ventana donde podemos seleccionar la ayuda de un asistente (script wizard). 
![inno](imgGit/inno.png)  
Debemos ir llenando los campos solicitados _requeridos_. Aquí dejó la configuración en la versión 6.1.2:  
1. En la primer ventana se seleccionan las especificaciones de la aplicación generada por el instalador, nombre y versión.
Ejemplo: _Games_ y _1.0.0_
2. En la siguiente ventana se configura el folder base donde se instalará que por defecto es (como la mayoría de los programas) en _Program Files folder_.  
**Lo importante** es cambiar el nombre del folder generado por el de la ruta que le dimos en el Launch4j para buscar el JRE y desmarcar el checkbutton que permite al usuario modificar esta ruta.  
Ejemplo: _Games_.   (Así crea la carpeta "Games" en ProgramFiles (x86) en cualquier pc donde se instale).
3. En la siguiente ventana se especifíca la ruta del .exe que nos generó Launch4j y que se generará cada vez que ejecutemos en algún equipo este instalador.  
Y otra **configuracion importante** es seleccionar en otros archivos de aplicación la ruta de nuestro folder que contiene el jre.  
Ejemplo: _C:\Program Files\Java\jre1.8.0_271_  
Este lo incluirá en el instalador, así viajará con nuestra aplicación.  
*Importante* editar el _destination subfolder_ para que el instalador lo genere en la carpeta que seleccionamos en el Launch4j, en este ejemplo _miJRE_.
4. En la siguiente ventana desmarcamos la opción que habilita la vinculación con extensiones para nuestra aplicación (a menos que la programación de nuestra app lo requiera).
5. La siguiente ventana permite elegir que el instalador genere o no accesos directos de escritorio. Marcar o desmarcar al gusto.
6. La siguiente ventana es para  introducir la licencia del archivo y archivos de información antes y después de la instalación. Si los tenemos proporcionamos la ruta. En este ejemplo se dejan vacíos.
7. En esta ventana se selecciona un modo de instalación. En este ejemplo se deja el valor por default.
8. En esta ventana se seleccionan los idiomas disponibles para nuestra instalación.
9. En esta ventana seleccionamos ña ruta donde nos dejará este instalador (en nuestra pc) y el nombre del instalador  
Ejemplo: _C:\Users\Rai\Desktop\_ y como nombre _instaladorGames_  
También podemos seleccionar un ícono para nuestro instalador.
10. Esta ventana se queda con el valor por defecto y damos click en _finalizar_.
11. Ejecutamos nuestro script.



