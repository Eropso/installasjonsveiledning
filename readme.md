# Installasjonsveiledning

## Introduksjon
Denne veiledningen skal inneholde hvordan man tilbakestiller Raspberry pi og starter den opp igjen, laster ned ssh og kobler det med egen pc, setter opp mariadb og sette opp telefonkatalogen.
 
## Innhold
1. [Introduksjon](#introduksjon)
2. [Raspberry Pi](#raspberry-pi)
3. [Start](#start)
4. [SSH](#ssh)
5. [MariaDB](#mariadb)
6. [Statisk IP-Adresse](#statisk-ip-adresse)
7. [Telefonkatalog](#telefonkatalog)
 

 


## Raspberry Pi
Last ned exe fil fra https://www.raspberrypi.com/software/

Run exe filen 

Ta SD kortet i Pc-en og slett alt innhold 

Når den er ferdig innstalert får du opp alternativ for Device, Os og Storage.

* Velg din Pi model
* Velg Ubuntu desktop
* Velg din storage
* Trykk next

Når installasjonen er ferdig, sett SD kortet i Pi-en og slå den på

Følg instruksjonene


## Start

Det er viktig å først oppdatere alle programvarene


```bash
sudo apt update
```
Finner oppdateringer
```bash
sudo apt upgrade
```

Det er noen viktige installasjoner man kan få bruk for
```bash
sudo apt install python3-pip
```

```bash
sudo apt install git
```




## SSH

### Brannmur

Sett opp brannmur for SSH tilkboling

```bash 
sudo apt install ufw
```
Installerer UFW


```bash 
sudo ufw enable
```
Aktiverer brannmur ved oppstart

```bash 
sudo ufw allow ssh
```
Tillater SSH gjennom brannmur

### Skru på SSH
```bash
sudo apt install openssh-server
```
Installerer SSH-server

```bash
sudo systemctl enable ssh 
```
SSH skrur ved oppstart

```bash
sudo systemctl start ssh
```
Starter SSH

<br>


For å finne IP-en din skriv følgende
```bash
ip a
```

Når du har funnet IP-en din gå til pc-en du har lyst til å styre Pi-en din med SSH og åpne terminalen
```cmd
ssh brukernavn@ip
```


Bytt ut brukernavn og ip med dine


## MariaDB

```bash
sudo apt install mariadb-server
```

```bash
sudo mysql_secure_installation
```

```bash
sudo mariadb –u root
```
Logg inn i MariaDB


```bash
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```
Lag ny bruker

```bash
GRANT ALL PRIVILEGES ON *.* TO 'username'@’localhost’ IDENTIFIED BY 'password';
```
Gir rettigheter

```bash
FLUSH PRIVILEGES;
```
Oppdater rettigheter


## Statisk IP-Adresse
```bash
sudo nmtui
```
1. Gå inn på edit connection

2. Velg ethernet eller wifi

3. Bytt IPv4 til manual

4. Legg til ønsket adresse

5. Sett gateway til 10.0.0.1

6. Sett DNS til 10.0.0.10 og 1.1.1.1 (cloudfare) som sekundær

7. Sett search domain som 255.0.0.0

8. Press ok

## Telefonkatalog

```
mkdir kode
```
Lager en mappe som heter kode


```
cd kode
```
Går inn i mappen

Finn github lenken og kopier den
https://github.com/Eropso/installasjonsveiledning

```
git clone https://github.com/Eropso/installasjonsveiledning
```
Kloner innholdet i github repo

Logg inn i MariaDB og skriv/lim inn koden fra de fire første sql-filene

```
CREATE DATABASE telefonkatalog;
```

```
USE telefonkatalog;

CREATE TABLE person (
    id int NOT NULL AUTO_INCREMENT,
    fornavn VARCHAR(255) NOT NULL,
    etternavn VARCHAR(255) NOT NULL,
    telefonnummer CHAR(8),
    PRIMARY KEY (id)
);
```

```
INSERT INTO person (fornavn, etternavn, telefonnummer)
VALUES ('Erik', 'Perik', '12345678');
INSERT INTO person (fornavn, etternavn, telefonnummer)
VALUES ('Lise', 'Pise', '22334455');
INSERT INTO person (fornavn, etternavn, telefonnummer)
VALUES ('Testus', 'Jensen', '11114444');
INSERT INTO person (fornavn, etternavn, telefonnummer)
VALUES ('Knut', 'Donald', '31415926');
```


```
SELECT * FROM person;
SELECT fornavn, telefonnummer FROM person;
SELECT * FROM person WHERE fornavn = "Erik";
SELECT telefonnummer FROM person WHERE fornavn = "Lise" AND etternavn = "Pise";
```

I GitHub-repositoriet ligger det en mappe for Python. Last ned telefonkatalog_oppdater_sql.py og lagre den på laptopen din (ikke Raspberry Pi).

Installer mysql-connector-python


```
pip install mysql-connector-python
```

```
import mysql.connector

mydb = mysql.connector.connect(
host="ip_adressen_til_PI",
user="username",
password="password",
database="telefonkatalog")

cursor = mydb.cursor()
cursor.execute("SELECT * FROM person")
resultater = cursor.fetchall()

for dings in resultater:
    print(dings)
```

Endre IP-adresse, username og passord til din.

Kjør python koden på laptopen din.