# 🖥️ Laboratorio: Conexión a Servidor Ubuntu EC2 y Configuración de Apache

En esta guía aprenderás a conectarte a una instancia EC2 de Ubuntu en AWS utilizando un archivo de clave privada (.pem). También realizarás la instalación de Apache para verificar que el servidor web está funcionando correctamente y te introducirás a comandos de verificación, red y archivos.

## 🔧 Requisitos previos

- Tener un archivo `.pem` (clave privada) descargado desde AWS.
- Tener instalada una terminal compatible: Windows Terminal, Git Bash, PowerShell, o Terminal en macOS/Linux.
- Tener acceso a internet y la IP pública de tu instancia EC2.
- Tu instancia debe tener habilitado el puerto 22 (SSH) y 80 (HTTP) en su grupo de seguridad.

## 🔐 Conexión SSH a tu instancia Ubuntu

1. Ubica tu archivo `.pem` en una carpeta segura.
2. Abre la terminal y navega a esa carpeta.
3. Establece permisos seguros con:

```bash
chmod 400 nombre.pem
```

4. Conéctate usando:

```bash
ssh -i nombre.pem ubuntu@<IP_PUBLICA>
```

Reemplaza `nombre.pem` por el nombre real de tu archivo y `<IP_PUBLICA>` por la IP de tu instancia EC2.

## 🌐 Verificar conexión a Internet (Importante)

Antes de hacer actualizaciones, asegúrate de que la instancia tiene conexión:

```bash
ping -c 3 google.com
```

## 🔄 Actualización del sistema Ubuntu

Una vez conectado, ejecuta lo siguiente para actualizar el índice local de paquetes y descarga una lista actualizada de los paquetes disponibles y sus versiones:

```bash
sudo apt update
```

Instala las actualizaciones disponibles de los paquetes que ya tienes instalados:

```bash
sudo apt upgrade
```

## 🌐 Instalación del servidor web Apache

Para instalar Apache en el servidor Ubuntu, ejecuta:

```bash
sudo apt install apache2
```

Consulta la versión instalada:

```bash
sudo apache2 -v
```

Inicia el servicio apache2 usando systemctl, el comando para controlar servicios en sistema:

```bash
sudo systemctl start apache2     # Inicia el servicio
```

Consulta el estado del servicio apache2:

```bash
sudo systemctl status apache2    # Muestra si está activo
```

Configura Apache para que se inicie automáticamente al arrancar el sistema.

```bash
sudo systemctl enable apache2    # Lo activa al arrancar 
```

## 🔎 Verifica el puerto 80 y respuesta de Apache

Comprueba que el servidor esté escuchando en el puerto 80:

```bash
sudo ss -tuln | grep :80
```

Alternativa para probar desde la misma instancia:

```bash
curl -I http://localhost
```

## ✅ Verificación del servidor web

Edita el contenido visible desde el navegador con:

```bash
echo '¡Hola desde mi servidor Ubuntu EC2!' | sudo tee /var/www/html/index.html
```

> `tee` permite redirigir con permisos elevados

Luego abre tu navegador y accede a:

```
http://<IP_PUBLICA>
```

## 🚀 Nueva Página Personalizada

Crea un archivo nuevo y accede a él desde el navegador:

```bash
echo 'Página personalizada de <TuNombre>' | sudo tee /var/www/html/info.html
```

Accede a:

```
http://<IP_PUBLICA>/info.html
```

## 🛫 Parte 2

Continúa a la segunda parte del Laboratorio consultando el archivo [Service](./service.md) para seguir trabajando con tu instancia.

## 📔 Glosario

Puedes consultar el glosario de términos en [Glosario](./glosario.md)
