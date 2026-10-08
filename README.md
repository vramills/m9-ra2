# Activitat 2.1 - Instal·lació de WordPress

# Exercici 1 - Llicències i requisits

## 1.1. Gestors de continguts

Un gestor de continguts o **CMS (Content Management System)** és una aplicació que permet crear, modificar, publicar i administrar el contingut d'un lloc web mitjançant una interfície gràfica.

He consultat diferents gestors de continguts i n'he comparat les característiques principals i la llicència.

| CMS | Característiques principals | Llicència | Ús habitual |
|---|---|---|---|
| **WordPress** | Fàcil d'utilitzar, gran quantitat de temes i plugins, gestió de pàgines, entrades, usuaris i contingut multimèdia. | GNU GPL v2 o posterior | Blogs, webs corporatives, portals i botigues en línia |
| **Joomla!** | CMS modular i extensible, amb plantilles, extensions, gestió d'usuaris i contingut multilingüe. | GNU GPL v2 o posterior | Portals, webs corporatives i comunitats |
| **Drupal** | CMS molt flexible, amb gestió avançada d'usuaris, permisos i contingut estructurat. | GNU GPL v2 o posterior | Portals grans, administracions, universitats i webs empresarials |
| **TYPO3** | CMS orientat a projectes empresarials, amb gestió avançada de continguts, idiomes i permisos. | GNU GPL v2 o posterior | Webs empresarials i projectes de grans dimensions |

### WordPress

WordPress és un gestor de continguts de codi obert que permet crear diferents tipus de llocs web.

Algunes de les seves característiques principals són:

- Creació de pàgines i entrades.
- Gestió d'usuaris amb diferents rols.
- Gestió d'imatges i contingut multimèdia.
- Sistema de temes per modificar el disseny.
- Sistema de plugins per ampliar les funcionalitats.
- Gestió de comentaris.
- Configuració d'enllaços permanents.
- Administració mitjançant una interfície web.

WordPress es distribueix sota la llicència **GNU GPL v2 o posterior**.

### Joomla!

Joomla! és un gestor de continguts de codi obert i modular.

Algunes de les seves característiques són:

- Gestió de continguts i articles.
- Sistema de plantilles.
- Instal·lació d'extensions.
- Gestió d'usuaris i permisos.
- Suport per a webs multilingües.
- Arquitectura modular.

Joomla! es distribueix sota la llicència **GNU GPL v2 o posterior**.

### Drupal

Drupal és un gestor de continguts de codi obert especialment orientat a projectes que necessiten flexibilitat i una gestió avançada.

Entre les seves característiques hi ha:

- Gestió avançada d'usuaris i permisos.
- Creació de tipus de contingut personalitzats.
- Suport multilingüe.
- Sistema de mòduls.
- Bona escalabilitat.
- Control d'accés avançat.

Drupal es distribueix sota la llicència **GNU GPL v2 o posterior**.

### TYPO3

TYPO3 és un gestor de continguts de codi obert orientat principalment a entorns empresarials.

Algunes de les seves característiques són:

- Gestió avançada de continguts.
- Gestió de múltiples idiomes.
- Gestió d'usuaris i permisos.
- Administració de diversos llocs web.
- Sistema d'extensions.
- Orientat a projectes de grans dimensions.

TYPO3 utilitza la llicència **GNU GPL v2 o posterior**.

### Comparació

Després de comparar els diferents gestors de continguts, WordPress és una opció adequada per a aquesta pràctica perquè té una instal·lació senzilla, una interfície fàcil d'utilitzar i permet ampliar les seves funcionalitats mitjançant plugins i temes.

Drupal i TYPO3 estan més orientats a projectes grans o que necessiten una configuració més avançada, mentre que Joomla! també ofereix una arquitectura modular basada en extensions.

---

## 1.2. Requisits de funcionament de WordPress

Per instal·lar WordPress és necessari disposar d'un servidor web amb PHP i una base de dades compatible.

Els requisits recomanats són:

- **PHP 8.3 o superior**.
- **MariaDB 10.11 o superior** o **MySQL 8.0 o superior**.
- Suport per a **HTTPS**.
- Servidor web **Apache** o **Nginx**.
- En Apache és recomanable utilitzar el mòdul `mod_rewrite`.

En aquesta pràctica utilitzo:

```text
Sistema operatiu: Ubuntu
Servidor web: Apache
Base de dades: MariaDB
Llenguatge: PHP 8.3
Protocol segur: HTTPS
CMS: WordPress
```

També són necessàries diverses extensions de PHP:

```text
php8.3-mysql
php8.3-curl
php8.3-gd
php8.3-mbstring
php8.3-xml
php8.3-zip
```

Aquestes extensions permeten connectar PHP amb MariaDB, treballar amb imatges, XML, fitxers ZIP i altres funcions necessàries per al correcte funcionament de WordPress.

---

# Exercici 2 - Instal·lació de WordPress

En aquest exercici he instal·lat WordPress sobre el servidor creat anteriorment a la RA1.

El servidor ja disposava d'Apache, MariaDB, PHP i una configuració HTTPS amb el domini:

```text
servidor.thos.local
```

## 2.1. Preparació del servidor

Primer he actualitzat la llista de paquets i els paquets instal·lats:

```bash
sudo apt update && sudo apt upgrade -y
```

Després he instal·lat Apache, MariaDB i PHP:

```bash
sudo apt install apache2
sudo apt install mariadb-server
sudo apt install libapache2-mod-php8.3 php8.3
```

A continuació he instal·lat les extensions de PHP necessàries per a WordPress:

```bash
sudo apt install php8.3-mysql php8.3-curl php8.3-gd php8.3-mbstring php8.3-xml php8.3-zip
```

Finalment he reiniciat Apache:

```bash
sudo systemctl restart apache2
```

---

## 2.2. Creació de la base de dades

He accedit a MariaDB amb l'usuari `root`:

```bash
sudo mysql -u root -p
```

Dins de MariaDB he creat la base de dades per a WordPress:

```sql
CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

He creat un usuari específic per a WordPress:

```sql
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY '1234';
```

He assignat tots els permisos de la base de dades a aquest usuari:

```sql
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
```

He aplicat els canvis:

```sql
FLUSH PRIVILEGES;
```

Finalment he sortit de MariaDB:

```sql
EXIT;
```

Les dades utilitzades són:

```text
Base de dades: wordpress
Usuari de la base de dades: wordpress
Contrasenya de la base de dades: 1234
Servidor: localhost
```

---

## 2.3. Configuració del domini i certificat autosignat

La configuració del domini i del certificat SSL ja s'havia realitzat durant la RA1.

El domini utilitzat és:

```text
servidor.thos.local
```

He creat un certificat autosignat amb:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/servidor.key -out /etc/ssl/certs/servidor.crt -subj "/C=ES/ST=Barcelona/L=Mataro/O=IES Thos i Codina/OU=ASIX/CN=servidor.thos.local/emailAddress=victor@iesthosicodina.cat"
```

El certificat s'ha guardat a:

```text
/etc/ssl/certs/servidor.crt
```

I la clau privada a:

```text
/etc/ssl/private/servidor.key
```

---

## 2.4. Configuració d'Apache

He editat el fitxer de configuració HTTP:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

He afegit el domini i la redirecció cap a HTTPS:

```apache
<VirtualHost *:80>

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    ServerName servidor.thos.local
    Redirect permanent / https://servidor.thos.local

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

A continuació he editat la configuració HTTPS:

```bash
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

He configurat:

```apache
<VirtualHost *:443>

    ServerAdmin webmaster@localhost
    ServerName servidor.thos.local
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    SSLEngine on

    SSLCertificateFile /etc/ssl/certs/servidor.crt
    SSLCertificateKeyFile /etc/ssl/private/servidor.key

</VirtualHost>
```

He habilitat el lloc SSL:

```bash
sudo a2ensite default-ssl.conf
```

He habilitat el mòdul SSL:

```bash
sudo a2enmod ssl
```

També he habilitat el mòdul `rewrite`:

```bash
sudo a2enmod rewrite
```

Finalment he reiniciat Apache:

```bash
sudo systemctl restart apache2
```

---

## 2.5. Configuració del domini al client

Al client he editat el fitxer:

```bash
sudo nano /etc/hosts
```

He associat la IP del servidor amb el domini:

```text
<IP_DEL_SERVIDOR> servidor.thos.local
```

Per exemple, durant la configuració inicial de la pràctica:

```text
192.168.56.101 servidor.thos.local
```

D'aquesta manera es pot accedir al servidor utilitzant:

```text
https://servidor.thos.local
```

---

## 2.6. Descàrrega de WordPress

M'he desplaçat al directori temporal:

```bash
cd /tmp
```

He descarregat WordPress:

```bash
wget https://wordpress.org/latest.tar.gz
```

He descomprimit el fitxer:

```bash
tar -xzf latest.tar.gz
```

---

## 2.7. Instal·lació de WordPress

He creat el directori de WordPress dins del directori web d'Apache:

```bash
sudo mkdir /var/www/html/wordpress
```

He copiat els fitxers:

```bash
sudo cp -a /tmp/wordpress/. /var/www/html/wordpress/
```

He assignat els fitxers a l'usuari i grup d'Apache:

```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
```

He configurat els permisos dels directoris:

```bash
sudo find /var/www/html/wordpress -type d -exec chmod 755 {} \;
```

He configurat els permisos dels fitxers:

```bash
sudo find /var/www/html/wordpress -type f -exec chmod 644 {} \;
```

---

## 2.8. Configuració de WordPress

He accedit al directori de WordPress:

```bash
cd /var/www/html/wordpress
```

He creat el fitxer de configuració a partir del fitxer d'exemple:

```bash
sudo cp wp-config-sample.php wp-config.php
```

He editat el fitxer:

```bash
sudo nano wp-config.php
```

He configurat la connexió amb la base de dades:

```php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpress' );
define( 'DB_PASSWORD', '1234' );
define( 'DB_HOST', 'localhost' );
```

Finalment he reiniciat Apache:

```bash
sudo systemctl restart apache2
```

---

## 2.9. Accés a WordPress

Com que WordPress està instal·lat a:

```text
/var/www/html/wordpress
```

s'hi accedeix des del navegador mitjançant:

```text
https://servidor.thos.local/wordpress/
```

El panell d'administració està disponible a:

```text
https://servidor.thos.local/wordpress/wp-admin/
```

Les credencials d'accés a WordPress són:

```text
Usuari: wordpress
Contrasenya: 1234
```

Amb aquestes credencials es pot accedir al panell d'administració de WordPress i continuar amb la configuració del lloc web.

Des de l'assistent web de WordPress he completat la configuració inicial del lloc indicant:

- Títol del lloc.
- Usuari administrador: `wordpress`.
- Contrasenya: `1234`.
- Correu electrònic.

Amb aquests passos WordPress queda instal·lat i preparat per començar el projecte web.