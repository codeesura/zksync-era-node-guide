# Guía de Configuración del Nodo zkSync Era

Esta guía explica paso a paso cómo configurar un nodo `zkSync Era` en Ubuntu Linux.

## Requisitos del Sistema

- CPU: Se recomienda una CPU relativamente moderna.
- RAM: 32 GB
- Almacenamiento: 300 GB, con un crecimiento del estado de aproximadamente 1 TB por mes.
- Red: Conexión de 100 Mbps (se recomienda 1 Gbps+)

### Alquiler de Servidor

Si necesita un servidor, puede alquilar uno de Hetzner. Además, si se registra utilizando este enlace de referencia, recibirá un crédito de 20 EUR.

[Hetzner](https://hetzner.cloud/?ref=fu2umOyLCWhh)

## Paso 1: Instalación de Visual Studio Code (En el Computador Local)

1. **Descargar e Instalar Visual Studio Code:**
   [Página de Descarga de Visual Studio Code](https://code.visualstudio.com/)

   - Una vez descargado, siga el asistente de instalación para instalar Visual Studio Code.

2. **Instalar la Extensión de SSH:**
   - Abra Visual Studio Code.
   - Haga clic en el icono de `Extensiones` en la barra lateral izquierda.
   - Escriba `Remote - SSH` en la barra de búsqueda e instale la extensión desarrollada por Microsoft.

## Paso 2: Conectarse al Servidor vía SSH (Desde el Computador Local)

1. **Conectarse a SSH en Visual Studio Code:**

   - Presione `Ctrl+Shift+P` en Visual Studio Code y seleccione `Remote-SSH: Connect to Host...`.
   - Ingrese la dirección de su servidor en el formato `ssh root@ip_address`.
   - Ingrese su contraseña o clave SSH para completar la conexión.

   **Nota:** Reemplace `ip_address` con la dirección IP real de su servidor.

2. **Verificar la Conexión:**

   - Si la conexión es exitosa, el nombre del servidor conectado aparecerá en la esquina inferior izquierda de la ventana de Visual Studio Code.

3. **Abrir el Terminal:**
   - Después de conectarse al servidor en Visual Studio Code, puede abrir el terminal haciendo clic en `Terminal` > `Nuevo Terminal` en el menú superior.
   - Alternativamente, puede abrir el terminal presionando `Ctrl+` (tecla Control y la tecla de comillas invertidas, ubicada debajo de la tecla ESC).

## Paso 3: Instalación de Docker y Docker Compose (En el Servidor)

Después de conectarse con éxito al servidor, ejecute estos comandos en el servidor.

### Instalación de Docker

1. **Actualizar los Paquetes del Sistema:**

   ```sh
   sudo apt update
   ```

2. **Instalar Docker:**

   ```sh
   sudo apt install docker.io
   ```

3. **Verificar que el Servicio de Docker está en Ejecución:**

   ```sh
   sudo systemctl status docker
   ```

4. **Verificar la Instalación de Docker:**
   ```sh
   docker --version
   ```

### Instalación de Docker Compose

1. **Descargar Docker Compose:**

   ```sh
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Conceder Permiso de Ejecución a Docker Compose:**

   ```sh
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Verificar la Instalación:**
   ```sh
   docker-compose --version
   ```

## Paso 4: Iniciar el Nodo zkSync Era (En el Servidor)

1. **Instalar Git (si no está instalado):**

   ```sh
   sudo apt install git
   ```

2. **Clonar el Repositorio:**

   ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **Navegar a los Directorios Clonados:**

   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **Iniciar el Nodo Usando Docker Compose:**

   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d
   ```

5. **Verificar los Contenedores en Ejecución:**

   ```sh
   docker ps
   ```

   Este comando lista los contenedores Docker que están en ejecución. Debería ver los contenedores relacionados con el nodo `zkSync Era`.

6. **Ver y Seguir los Últimos 100 Logs:**
   ```sh
   docker logs -f --tail 100 docker-compose-examples-external-node-1
   ```

Presione `Ctrl+C` para salir de la vista de logs. Esto no detendrá su nodo; continuará ejecutándose en segundo plano.

## Paso 5: Acceder al Dashboard mediante Reenvío de Puertos (En el Computador Local)

Después de ejecutar Docker, puede acceder a un dashboard en el puerto 3000. Para ver este dashboard desde su computador local, debe configurar el reenvío de puertos en Visual Studio Code.

1. **Configurar el Reenvío de Puertos:**

   - Presione `Ctrl+Shift+P` para abrir la paleta de comandos.
   - Escriba `Forward a Port` y seleccione la opción.
   - Ingrese `3000` como número de puerto y presione Enter.

   Alternativamente:

   - Haga clic en la sección `PORTS` en la esquina inferior derecha de Visual Studio Code.
   - Seleccione `Forward Port...` en el menú.
   - Ingrese `3000` como número de puerto y presione Enter.

2. **Acceder al Dashboard:**

   - Abra su navegador web y vaya a `http://localhost:3000/d/0/external-node`.
   - Ahora debería tener acceso al dashboard.

Este paso le permite ver fácilmente el dashboard de la aplicación que se ejecuta en su servidor remoto desde su computador local.

## Información Adicional

- **Ver Logs de los Contenedores:**

  ```sh
  docker logs <container_id>
  ```

- **Detener el Nodo:**
  ```sh
  docker-compose -f mainnet-external-node-docker-compose.yml down
  ```
