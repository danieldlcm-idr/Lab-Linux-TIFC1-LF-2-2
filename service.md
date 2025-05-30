# 🛠️ Crear y gestionar un servicio personalizado en Ubuntu (Systemd)

Crear un servicio básico en Ubuntu, que se ejecuta en segundo plano y escribe un mensaje en un archivo de log. 

## 📁 Paso 1: Crear un archivo `.service` en tu computadora

Desde tu terminal local (no dentro del servidor), crea un archivo llamado `hello.service`:

```bash
touch hello.service
```

Usando `vim` edita el archivo y copia el siguiente contenido:

```ini
[Unit]
Description=Servicio de ejemplo - Echo Hello
After=network.target

[Service]
Type=simple
ExecStart=/bin/bash -c 'while true; do echo "Hola desde el servicio" >> /var/log/hello.log; sleep 10; done'
Restart=always

[Install]
WantedBy=multi-user.target
```

Este archivo define un servicio que:
- Se ejecuta después de que la red esté disponible.
- Corre un ciclo infinito que cada 10 segundos escribe un mensaje en `/var/log/hello.log`.
- Se reinicia automáticamente si falla.
- Se inicia automáticamente con el sistema.

## 📤 Paso 2: Subir el archivo al servidor EC2

Desde tu terminal local, utiliza el siguiente comando para subir el archivo:

```bash
scp -i nombre.pem hello.service ubuntu@<IP_PUBLICA>:~
```

Reemplaza `nombre.pem` por el nombre de tu archivo `.pem` y `<IP_PUBLICA>` por la IP de tu instancia.

## 🔐 Paso 3: Conectarse al servidor y mover el archivo

Accede a la instancia EC2:

```bash
ssh -i nombre.pem ubuntu@<IP_PUBLICA>
```

Mueve el archivo al directorio de servicios del sistema:

```bash
sudo mv hello.service /etc/systemd/system/
```

## ⚙️ Paso 4: Habilitar e iniciar el servicio

Primero habilita el servicio para que se inicie con el sistema operativo:

```bash
sudo systemctl enable hello
```

Luego, inicia el servicio:

```bash
sudo systemctl start hello
```

## ✅ Paso 5: Verificar el estado del servicio

Comprueba que el servicio está en ejecución:

```bash
sudo systemctl status hello
```

## 📄 Paso 6: Ver el contenido del log generado

El servicio escribe un mensaje en un archivo de log cada 10 segundos. Puedes ver el contenido con:

```bash
sudo tail -f /var/log/hello.log
```

Presiona `Ctrl + C` para salir del monitoreo en tiempo real.

## 🧪 Bonus: Detener y reiniciar el servicio

Para detener el servicio:

```bash
sudo systemctl stop hello
```

Para reiniciarlo:

```bash
sudo systemctl restart hello
```

