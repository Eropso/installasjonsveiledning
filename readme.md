# Installasjonsveiledning

## Introduksjon
Denne veiledningen skal inneholde hvordan man tilbakestiller Raspberry pi og starter den opp igjen, laster ned ssh og kobler det med egen pc og setter opp mariadb.
 
## Innhold
1. [Introduksjon](#introduksjon)
2. [Raspberry Pi](#raspberry-pi)
3. [Start](#start)
4. [SSH](#ssh)
5. [MariaDB](#mariadb)
6. [Statisk IP-Adresse](#statisk-ip-adresse)
 

 


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

