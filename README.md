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
