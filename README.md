# Practica-Tomcat-JavierGarcia

## 1. Configuraciones Previas de la Máquina

Creamos el repositorio de Github y lo clonamos mediante **git clone**

Una vez hemos clonado el repositorio, entramos dentro de lo que sería el proyecto y hacemos un **vagrant init ubuntu/jammy64**, con esto se nos creará de manera automática el **Vagrantfile**, pero nosotros lo vamos a editar de la siguiente manera:

![Vagrantfile](/capturas/vagrantfile.png)

Con esa edición ya si podemos hacer el **vagrant up** y el **vagrant ssh** correspondientes.

![Vagranssh](/capturas/vagrantssh.png)

## 2. Instalación de Java

Ya una vez dentro de la máquina virtual lo primero que vamos a hacer es instalar Java, primeramente actualizaremos la máquina mediante un **sudo apt update** y posteriormente haremos un **sudo apt install -y openjdk-11-jdk** para poder instalarlo, comprobaremos que se ha instalado correctamente mediante un **java -version**:

![JavaVersion](/capturas/javaversion.png)

## 3. Instalación del servidor Apache Tomcat

Una vez comprobado queya tenemos instalado Java pasaremos a instalar Tomcat. Para elló, usaremos el comando **sudo apt install -y tomcat9 tomcat9-admin** y para comprobar que se ha instalado correctamente usaremos un **systemctl status tomcat9**:

![systemctl](/capturas/systemctl.png)

Ahora solo nos faltaría irnos a nuestro navegador y comprobar que todo esta funcionando poniendo en la URL **http://localhost:8080**:

![localhost](/capturas/localhost.png)

## 4. Configuración del usuario administrador de Tomcat

Para configurar el usuario administrador de Tomcat, debemos meternos en el siguiente fichero **/etc/tomcat9/tomcat-users.xml**, y dentro de él, antes de **</tomcat-users>** añadimos lo siguiente, que básicamente lo que estaríamos haciendo es darle un rol al admin, un nombre de usuario y una contraseña:

![tomcatxml](/capturas/tomcatxml.png)

Y después de haber editado eso, lo guardamos y reiniciamos el sistema mediante un **systemctl restart tomcat9**

## 5. Revisamos el archivo context.xml

Para continuar tendriamos que comprobar que el acceso remoto al Manager de Tomcat está permitido, para ello, nos metemos en el siguiente fichero **/etc/tomcat9/context.xml** y comprobamos que no haya ninguna regla que contenga esta etiqueta **<Valve>**, si estuviera la tendriamos que comentar( en mi caso no me aparecía eso, me aparecía directamente lo que ves en la imagen, yo de ese fichero no he tenido que tocar nada y me ha funcionado todo correctamente ):

![tomcatxml](/capturas/contentxml.png)

## 6. Despliegue de la aplicación desde el Manager

Una vez creado el admin accedemos a **http://localhost:8080/manager**, donde nos pedirá las credenciales del usuario que hemos añadido anteriormente:

![manager](/capturas/manager.png)

Y ahora donde pone **Archivo War a desplegar**, seleccionamos el archivo que nos has dejado en la práctica y le damos a **Desplegar**:

![desplegar](/capturas/desplegar.png)

Y para comprobar que todo funciona, ponemos en la URL **http://localhost:8080/tomcat1** y nos debería de aparecer lo siguiente:

![tomcat1](/capturas/tomcat1.png)

## 7. Instalación de Maven

Si todo va correctamente hasta este punto, pasamos a instalar Maven mediante un **sudo apt install -y maven** y para comprobar que lo hemos instalado haremos un **mvn -version**:

![mvnversion](/capturas/mvnversion.png)

## 8. Desplegar la aplicación de piedra, papel y tijeras con Maven

Una vez instalado Maven, lo primero que vamos a hacer es clonar el repositorio que nos has dejado en la guía mediante un **git clone https://github.com/cameronmcnz/rock-paper-scissors.git**, nos metemos dentro de él con un **cd rock-paper-scissors** y cambiamos de rama con un **git checkout patch-1**:

![rock](/capturas/rock.png)

Ahora para compilar el proyecto se ejecuta el siguiente comando **mvn clean package** dentro del directorio del proyecto, y Maven le llamaba roshambo.war de forma automática, ya que lo ha cogido del pom.xml y lo genera en el directorio target:

![compilar](/capturas/compilar.png)

Y ahora desplegamos la aplicación copiando el WAR al directorio de Tomcat mediante un **sudo cp target/roshambo.war /var/lib/tomcat9/webapps/**:

![copiar](/capturas/copiar.png)

Y ya para comprobar que todo ha ido perfectamente, pondremos en la URL **http://localhost:8080/roshambo**, y nos debería de aparecer el juego:

![juego](/capturas/juego.png)

Y con esto habríamos terminado esta práctica de Aplicaciones de Java con Maven y Tomcat

### JAVIER GARCIA SANTIAGO






