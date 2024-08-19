# Setup de versiones

Odoo tiene distintas versiones, por lo tanto tener separadas las versiones es necesario.

La estructura recomendada es la siguiente:

```
.
└── v17/
    ├── compose.yaml
    ├── odoo.conf
    ├── odoo
    ├── extra-addons
    ├── .venv
    └── .python-version
```

Compose.yaml es el archivo de docker-compose que se encarga de levantar los contenedores necesarios para el entorno de desarrollo.

Odoo.conf es el archivo de configuración de Odoo.

Odoo es el directorio donde se clona el repositorio de Odoo.

Extra-addons es el directorio donde se clonan los módulos de Odoo personalizados.

.venv es el directorio donde se crea el entorno virtual de Python.

.python-version es el archivo que contiene la versión de Python que se utilizará en el entorno virtual.

## Crear entorno virtual

### Instalar pyenv

```bash
curl https://pyenv.run | bash
```

Se debe agregar la siguiente línea al archivo .bashrc o .zshrc.
Esto se encuentra en el directorio home del usuario o ~/.bashrc o ~/.zshrc.

```bash
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
```

### Ejecutar pyenv

Dentro del directorio del proyecto se debe ejecutar el siguiente comando para crear el entorno virtual.

```bash
pyenv install 3.10
pyenv local 3.10
python -m venv .venv
```

### Activar entorno virtual

```bash
source .venv/bin/activate
```

### Descargar Odoo

```bash
git clone https://github.com/odoo/odoo odoo
```

### Instalar dependencias para Ubuntu

```bash
sudo apt install nodejs npm
sudo npm install -g less less-plugin-clean-css
```

### Instalar dependencias de Odoo

Está dentro del directorio odoo clonado de github.

```bash
sudo ./odoo/setup/debinstall.sh
```

### Editar el archivo odoo.conf

```bash
[options]
addons_path = ./odoo/addons,./extra-addons/modulo1,./extra-addons/modulo2
db_host = localhost
db_port = 5432
db_user = odoo
db_password = odoo
```

#### Odoo para multiples clientes

Supongamos que tienes n clientes, cada cliente tiene su propia instancia de base de datos, por lo tanto, el odoo conf debe tener la siguiente estructura.

```bash
[options]
addons_path = ./odoo/addons,./extra-addons/modulo1,./extra-addons/modulo2
db_host = localhost
db_port = 5432
db_user = odoo
db_password = odoo
dbfilter = ^%d$
```

Donde %d es el nombre de la base de datos. Esto reflejado en la web seria como cliente1.midominio.com, cliente2.midominio.com, etc. donde cliente1, cliente2, etc. son los nombres de las bases de datos.

### Levantar Docker compose

```bash
docker compose up -d
```

### Iniciar Odoo

```bash
./odoo/odoo-bin -c odoo.conf
```

### Crear módulo

La estructura del comando es la siguiente:

**./odoo/odoo-bin scaffold nombre_modulo ruta_modulo**

```bash
./odoo/odoo-bin scaffold modulo1 ./extra-addons/modulo1
```

## Produccion

### Nginx

```bash
sudo apt install nginx
```

### Configurar ufw

El cometido con UFW es permitir el tráfico de Nginx por medio de un dominio, mas no por la IP y el puerto, por defecto se usa el puerto 8069 y 8071 en odoo, estos son visibles y accesibles por cualquier persona, por lo tanto, se debe restringir el acceso a estos puertos.

Deshabilitar los puertos 8069 y 8071 de que sean publicos y solo sean accesibles por el servidor.

```bash
sudo ufw deny 8069
sudo ufw deny 8071
```

Habilitar nginx en ufw
```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 'Nginx HTTPS'
```

### Build de Odoo

```bash
docker compose up -d --build
```