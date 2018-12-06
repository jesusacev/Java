Como compilar y ejecutar un codigo Java.
========

Javac
+++++++

- Primero creamos una carpeta llamada sources y dentro de ella creamos un archivo con código java llamado HolaMundo.java::

	$ mkdir sources
	$ cd sources
	$ vim HolaMundo.java
	
	/*
 	* * Hola Mund..... Ejemplo
 	* */

	public class HolaMundo {

	public static void main (String args[]){

	System.out.println("Hola Mundo !!!" + args[0]);
					        }
	}

  En la parte del código que dice "args[0]" se está solicitando un parámetro cuando se ejecuta el código java, que trabaja de forma parecida a las variables posicionales en unix.

- Definimos la variable JAVA_HOME y luego procedemos a compilarlo dandole la ruta absoluta del java con el binario encargado de la compilación que es el javac::

		
	$ export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_161
	$ echo $JAVA_HOME
	/usr/lib/jvm/jdk1.8.0_161
	$JAVA_HOME/bin/javac HolaMundo.java

- Y luego que compilamos el archivo de codigo Java, podemos ver que se genera otro archivo .class que es el compilado::

	$ ls
	HolaMundo.class  HolaMundo.java

- Despues ejecutamos el archivo a través del binario java con el parámetro que pide el código y el mismo se ejecuta de manera satisfactoria::

	$ $JAVA_HOME/bin/java HolaMundo jesus
	Hola Mundo !!!jesus

ANT
+++++++

- Vamos a volver a compilar el .java pero esta vez con el software ANT que es un compilador, y es una manera automatizada de hacer la compilación. Para ello procedemos a instalar ANT::

	# apt-get install ant

- Borramos el .class porque es el compilado y vamos a volver a compilar pero esta vez a través de ANT::

	$ rm -f HolaMundo.class

- Y creamos otro archivo .java para ver como el utilitario ANT compila los 2 archivos al mismo tiempo::

	$ vi BuenasMundo.java

	/*
	* Hola Mund..... Ejemplo
	*/

	public class BuenasMundo {

        public static void main (String args[]){

                System.out.println("Buenas Mundo !!!");
        }
	}

- Si listamos la carpeta sources tenemos los 2 .java::

	$ ls
	BuenasMundo.java  HolaMundo.java

- Ahora procedemos a crear el build.xml que es el archivo por defecto que lee el ANT cuando es ejecutado. Cabe destacar que se debe ejecutar en la misma ruta a donde fue creado el build::

	$ cd ..
	$ vi build.xml
	<project name="mi proyecto" default="all">
  	<target name="all" depends="build,pack" />

  	<target name="build">
    	<mkdir dir="bin" />
	<mkdir dir="lib" />
    	<!-- esto indica que compile todo lo que haya bajo sources y ponga las clases resultantes 		en bin -->
    	<javac srcdir="sources" destdir="bin" includes="**/*.java">
      	<!-- este es un classpath de ejemplo: un directorio lib al mismo nivel que sources, 		incluimos todos los jars que contenga -->
      	<classpath>
        <fileset dir="lib" includes="*.jar" />
      	</classpath>
    	</javac>
  	</target>

  	<target name="pack">
    	<jar file="SaludandoMundo.jar">
      	<!-- incluimos todas las clases bajo bin -->
      	<fileset dir="bin" includes="**/*.class" />
      	<!-- incluimos tambien los properties que estan directamente bajo sources (sin recursion) 		-->
      	<fileset dir="sources" includes="*.properties" />
      	<fileset dir="lib" includes="*.jar" />
      	<manifest>
        <attribute name="Main-Class" value="HolaMundo" />
      	</manifest>
    	</jar>
  	</target>

	</project>

- En el build.xml le decimos que vamos a compilar todo lo que esté en el directorio sources y que a su vez sea .java, y que el compilado será enviado al directorio bin como se expresa en esta línea "<javac srcdir="sources" destdir="bin" includes="**/*.java">". Luego en la carpeta lib se cargan las clases que son requeridas. Finalmente se crea un .jar que en este caso es SaludandoMundo.jar, que empaqueta los .class que están en bin y las librerias que están en lib.

- Ejecutamos ANT en el mismo directorio a donde tenemos el build.xml::

	$ ant
	Buildfile: /tmp/build.xml

	build:
    	[mkdir] Created dir: /tmp/bin
    	[mkdir] Created dir: /tmp/lib
    	[javac] /tmp/build.xml:8: warning: 'includeantruntime' was not set, defaulting to 		build.sysclasspath=last; set to false for repeatable builds
    	[javac] Compiling 2 source files to /tmp/bin

	pack:
      	[jar] Building jar: /tmp/SaludandoMundo.jar

	all:

	BUILD SUCCESSFUL
	Total time: 0 seconds

- luego verificamos el directorio bin que es donde definimos que se iban a colocar los archivos .class que fueron compilados::

	$ ls
	BuenasMundo.class  HolaMundo.class

- y si los ejecutamos con el binario de java tenemos el siguiente resultado::

	$ $JAVA_HOME/bin/java HolaMundo jesus
	Hola Mundo !!!jesus
	
	$ $JAVA_HOME/bin/java BuenasMundo
	Buenas Mundo !!!
	
- Por último ejecutamos el jar que dijimos que tenía el empaquetado de los .class de bin y las librerias de lib::
	
	$ cd ..
	$ java -jar SaludandoMundo.jar jesus
	Hola Mundo !!!jesus

- Como podemos ver sólo nos ejecuta el HolaMundo.class que tiene empaquetado, ya que en el build.xml le definimos que ese sería la clase principal en esta linea "<attribute name="Main-Class" value="HolaMundo" />", y por ende será el primero que se ejecute, y en este caso ese compilado no invoca a otro.
	
	




