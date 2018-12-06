Como compilar y ejecutar un codigo Java.
========

- Primero creamos un archivo con código java llamado HolaMundo.java::

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

- Despues ejecutamos el archivo con el parámetro que pide el código y el mismo se ejecuta de manera satisfactoria::

	$ $JAVA_HOME/bin/java HolaMundo jesus
	Hola Mundo !!!jesus




