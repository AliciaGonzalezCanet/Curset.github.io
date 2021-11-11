---
layout: page
title: UD2.INSTAL·LACIÓ I CONFIGURACIÓ D'ODOO.
---

## Preparació del sistema

### 1.Configurar la xarxa en ubuntu server 20.04
Editar el fitxer /etc/netplan/*.yaml

### 2.Actualització del sistema:

```
sudo apt-get update 
sudo apt-get dist-upgrade

```

### 3. Crear l'usuari de linux bàsic per a fer còrrer el Odoo, ja que no es aconsellable executar el Odoo com a root.

```
sudo adduser --system --quiet --shell=/bin/bash --home=/opt/odoo  --group odoo

```

### 4.Instal·lació del gestor de la base de dades i configuració.

```
sudo apt-get install postgresql postgresql-server-dev-12
su postgres 
createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo

```
### 5.Descàrrega d'odoo 
```
git clone https://www.github.com/odoo/odoo --depth 1 --branch 13.0 –single-branch
```

## Instal·lació d'Odoo

### 1.Instal·lació de les llibreries i paquest necessàris
```
sudo apt-get install build-essential python3-pillow python3-lxml 
python3-dev python3-pip python3-setuptools npm nodejs git gdebi 
libldap2-dev libsasl2-dev libxml2-dev libxslt1-dev libjpeg-dev apache2 -y
```

### 2. Instal·lació
```
pip3 install --upgrade pip
sudo pip3 install -r /opt/odoo/odoo/requirements.txt
```

### 3.Comprovació
```
pip3 freeze
```

## Configuració

### 1.Instal·lació de la llibreria pdf per a la generació d’informes:
```
wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.bionic_amd64.deb 
sudo gdebi -n wkhtmltox_0.12.5-1.bionic_amd64.deb 
sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin/ 
sudo ln -s /usr/local/bin/wkhtmltoimage /usr/bin/
```

### 2.Configurar odoo com a servei
#### Arxius logs i configuració de odoo (usuari root  o amb sudo):

```
mkdir /var/log/odoo/
chown odoo:root /var/log/odoo
cp /opt/odoo/odoo/debian/odoo.conf /etc/odoo.conf
chown odoo: /etc/odoo.conf
chmod 640 /etc/odoo.conf
```
#### Editem el fitxer de configuració:
```
nano /etc/odoo.conf
```
Comprovem les dades:
db_user = odoo
db_password = false
addons_path = /opt/odoo/odoo/addons
logfile = /var/log/odoo/odoo-server.log

#### Copiem el fitxer odoo.service al seu lloc:
```
sudo cp /opt/odoo/odoo/debian/odoo.service /etc/systemd/system/odoo.service
```
#### Editem el fitxer:
```
sudo nano /etc/systemd/system/odoo.service
```
#### Posem el següent contingut:
[Service]
Type= simple
User=odoo
Group= odoo
ExecStart=/opt/odoo/odo/odoo-bin --config /etc/odoo.conf

#### Activem el servei:
```
sudo systemctl enable odoo.service
```

#### Arrancar el servei:
```
sudo systemctl start odoo
```

## Accés a odoo
## http://IP_server:8069

![bg](img/odoo.png)

