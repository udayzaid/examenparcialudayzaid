# examenparcialudayzaid
# Despliegue de IIS en Docker sobre Windows

Este proyecto describe los pasos para crear un servidor web utilizando IIS (Internet Information Services) en un contenedor Docker sobre Windows. La solución incluye la creación de un archivo `Dockerfile` para configurar IIS, la construcción de la imagen y la ejecución del contenedor.

#Requisitos previos

Antes de comenzar, asegúrate de tener los siguientes requisitos instalados:

    Docker Desktop: Instalado y configurado para ejecutar contenedores de Windows.
    Windows 10/11 o una versión compatible con contenedores Docker para Windows.
    PowerShell: Para ejecutar los comandos necesarios en la terminal de Windows.
4. ### 1. Crear el directorio del proyecto
5. Crea un directorio para el proyecto. Puedes hacerlo ejecutando:

```bash
mkdir iis-docker
cd iis-docker
o se puede tambien creaando una carpeta en el disco local C o el de preferencia
#2. Crear el Dockerfile
En el directorio del proyecto, crea un archivo llamado Dockerfile con el siguiente contenido:
//Usar la imagen base de Windows Server Core con soporte LTSC 2019
FROM mcr.microsoft.com/windows/servercore:ltsc2019

 //Instalar IIS y herramientas de administración
RUN powershell -Command \
    Install-WindowsFeature -Name Web-Server -IncludeManagementTools

// Exponer el puerto 80 para tráfico HTTP
EXPOSE 80

 //Copiar el archivo index.html al contenedor
COPY ./index.html C:/inetpub/wwwroot/index.html

//Iniciar el servicio IIS y mantener el contenedor activo
CMD ["powershell", "-NoProfile", "-Command", "Start-Service w3svc; ping -t localhost"]


#3. Crear un archivo index.html
En el mismo directorio iis-docker, crea un archivo index.html con el siguiente contenido:
<!DOCTYPE html>
<html>
<head>
    <title>Bienvenido a IIS en Docker</title>
</head>
<body>
    <h1>¡Hola! Este es un servidor IIS corriendo dentro de un contenedor Docker sobre Windows.</h1>
</body>
</html>
#4. Construir la imagen Docker
En el directorio iis-docker, abre una terminal y ejecuta el siguiente comando para construir la imagen Docker:

docker build -t iis-server .
Este comando leerá el Dockerfile y creará la imagen llamada iis-server.
#5. Ejecutar el contenedor

Para ejecutar el contenedor en segundo plano y exponer el puerto 8080 del contenedor al puerto 80 de tu máquina local, usa el siguiente comando:

docker run -d -p 8080:80 --name iis-container iis-server
# 6. Acceder al servidor IIS
Una vez que el contenedor esté en ejecución, puedes acceder a IIS desde tu navegador utilizando la siguiente URL:

http://localhost:8080

# Conclusión

Con estos pasos, hemos configurado un servidor IIS en un contenedor Docker utilizando Windows Server Core. Ahora puedes personalizar el servidor, agregar más configuraciones y servir tus aplicaciones web dentro de un contenedor Windows. Docker ofrece una forma eficiente de desplegar aplicaciones en entornos controlados y reproducibles.
