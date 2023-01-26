#  Docker y Flask

En este ejemplo prático aprenderas a crear un contenedor de Docker a partir de una aplicacion de Flask.

## Requerimientos para este ejemplo

* **Python3**, debes tener Python 3 instalado

[How to install python 3 ubuntu](https://phoenixnap.com/kb/how-to-install-python-3-ubuntu)

```sh
python --version

sudo apt update

sudo apt install software-properties-common

sudo add-apt-repository ppa:deadsnakes/ppa

sudo apt update

sudo apt install python3.8
```




* **Visual Studio Code** (Opcional), en mi caso estaré usando **Visual Studio code** en este ejemplo

    * **Python Extension**. La extension de Python es muy util para poder leer entornos virtuales y poder tener autocompletado dentro del editor, asi como sugerencias


## Instalación del gestor de paquetes **```pip```**.

```sh
sudo apt-get install pip
```





## Creación de un Proyecto Flask

Para este ejemplo crearemos una aplicacion sencilla usando Flask que tendra por nombre **```docker-flask```**

```sh
mkdir docker-flask
cd docker-flask
```

## Creacion de entorno Virtual

Una vez creado nuestro proyecto, vamos a crear un entorno virtual para mantener aislada esta aplicacion de otras posibles aplicaciones que crearas a futuro usando **```Python```**.

Para crear un entorno virtual hay multiples paquetes de python que puedes instalar como:

```sh
venv
virtualenv
pipenv
conda
 ```
En mi caso voy a usar **```virtualenv```**. Puedes instalarlo usando **```pip```**

```sh
pip install virtualenv
# virtualenv venv
python3 -m virtualenv venv
source ./venv/bin/activate
```

## Instala las dependencias
Una vez activado tu entorno virtual, ejecuta lo siguiente:

```sh
pip install flask 
```

## Código Fuente
en un archivo **```app.py```**, agrega el siguiente codigo:


```sh
from flask import Flask, jsonify
from users import users

app = Flask(__name__)

# Routes
@app.route("/", methods=["GET"])
def ping():
    return jsonify({"response": "pong!"})


@app.route("/users")
def usershandler():
    return jsonify({"users": users})

# Start the Server
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=3000, debug=True)
```

luego en un archivo **```users.py```**, agrega lo siguiente:

```sh
users = [{"name": "fazt", "password": "some password"}]
```

finalmente puedes ejecutarlo con:

```sh
python app.py
```

y visitando el **```http://localhost:3000```**

## Dockerizando nuestra aplicacion

Para este ejemplo estaremos usando la [imagen oficial de Python para Docker](https://hub.docker.com/_/python)

Pero antes de colocar nuestra aplicacion en un contenedor.

Primero generemos un archivo que liste todos los paquetes que son necesarios para que funcione nuestra aplicación:

# NO FER AQUESTA COMANDA!!!
```sh
pip freeze > requirements.txt
```

# FER AQUESTA COMANDA!!!
```sh
click==7.1.2 > requirements.txt
Flask==1.1.2 >> requirements.txt
itsdangerous==1.1.0 >> requirements.txt
Jinja2==2.11.3 >> requirements.txt
MarkupSafe==1.1.1 >> requirements.txt
Werkzeug==1.0.1 >> requirements.txt
```

Ahora si, creemos nuestro archivo **```Dockerfile```**.

```sh
FROM python:3.8.10

WORKDIR /app

COPY requirements.txt requirements.txt

RUN pip3 install -r requirements.txt

COPY . .

CMD ["python", "src/app.py"]
```

```sh
docker build -t docker-flask .
docker run --publish 3000:3000 --name docker-flask docker-flask
docker images
```

modo interactivo

```sh
docker run -it docker-flask /bin/sh
```

para ejecutarlo en modo *detach*

```sh
docker run -it -p 4000:4000 --name docker-flask -d docker-flask
docker run -it -p 4000:4000 flaskapp
```

para ver el proceso ejecutando:

```sh
docker ps
docker logs <container_id>
```

para parar la ejecucion del contenedor

```sh
docker stop ID
```# docker-python-flask
