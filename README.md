# examenparcialudayzaid
# Despliegue de IIS en Docker sobre Windows

Este proyecto describe los pasos para crear un servidor web utilizando IIS (Internet Information Services) en un contenedor Docker sobre Windows. La solución incluye la creación de un archivo `Dockerfile` para configurar IIS, la construcción de la imagen y la ejecución del contenedor.

## Requisitos

1. **Docker Desktop** instalado y configurado para usar contenedores de Windows. Si no lo tienes, puedes descargarlo desde [Docker Desktop](https://www.docker.com/products/docker-desktop).
2. **Windows Subsystem for Linux 2 (WSL 2)** habilitado en Docker Desktop (es obligatorio para la compatibilidad con contenedores en Windows).
3. en este caso usaremos compatibilidad para windows
4. ### 1. Crear el directorio del proyecto
5. Crea un directorio para el proyecto. Puedes hacerlo ejecutando:

```bash
mkdir iis-docker
cd iis-docker
o se puede tambien creaando una carpeta en el disco local C o el de preferencia
2. Crear el Dockerfile
En el directorio del proyecto, crea un archivo llamado Dockerfile con el siguiente contenido:
# Usar la imagen oficial de Windows Server Core
FROM mcr.microsoft.com/windows/servercore:ltsc2022

# Instalar IIS (Internet Information Services)
RUN powershell -Command \
    Install-WindowsFeature -Name Web-Server -IncludeManagementTools

# Exponer el puerto 80 para el tráfico HTTP
EXPOSE 80

# Copiar el archivo index.html al contenedor
COPY ./index.html C:/inetpub/wwwroot/index.html

# Configurar IIS para que se inicie automáticamente
CMD ["powershell", "-NoProfile", "-Command", "Start-Service w3svc; Wait-Process"]

3. Crear un archivo index.html
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
4. Construir la imagen Docker
En el directorio iis-docker, abre una terminal y ejecuta el siguiente comando para construir la imagen Docker:

docker build --platform=windows/amd64 -t iis-server .

##conclusiones por motivos de tiempo en la clase sigue descargando la imagen que pesa 1,68 GB
