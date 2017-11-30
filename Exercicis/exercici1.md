### Es vol muntar entorn SGBD MySQL Percona amb rèplica. Es vol tenir un MySQL master a on s'aniran enviant totes les instruccions SQL d'inserció, modificació i esborrat. Es vol tenir dos MySQL  esclau del master anteriorment esmentat.  
### Cal que que al realitzar un INSERT en el master veiem les dades a l'esclau al cap d'un instant de temps.  

__CONFIGURACIÓ AL MASTER__

Realitza una còpia del fitxer de configuració del MySQL /etc/my.conf --> /etc/my.conf.bkp  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura1.PNG)  
  
Modifica el fitxer /etc/my.conf i activa el paràmetre log-bin.   
Verifica que el paràmetre server-id té un valor numèric (per defecte és 1).  
Verifica que tots els paràmetres de InnoDB estiguin descomentats.  
Canvia el paràmetre innodb_log_buffer a 10M.  
Canvia o afegeix el paràmetre innodb_log_files_in_group a 2.  
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura2.PNG)  
  
Para el servei de MySQL.  
> systemctl stop mysqld  
  
Borra tots els fitxers de log InnoDB del directory /var/lib/mysql  
  
Engega el servei de MySQL.  
> systemctl start mysqld  
  
Quants fitxers comencen amb el nom <PRIMER LLETRA DEL NOM + 1r COGNOM>rep dins el directori /var/lib/mysql? Digues quins són.  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura3.PNG)  
  
Realitza un instrucció DML, per exemple INSERT,UPDATE o DELETE.  
>  ![4](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura4.PNG)  
  
Obre un altre terminal i utilitzant l'eina mysqlbinlog mira el contingut del fitxer del binlog creat nom.000001  
> mysqlbinlog nom.000001  
  
Quin és el seu contingut?  
>  ![5](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura5.PNG)  

Fes un FLUSH DELS LOGS utilitzant la comanda FLUSH LOGS dins del MySQL.  
> mysql> FLUSH LOGS;  
>  ![6](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura6.PNG)  

Realitza una comprovació dels logs com a master mitjançant SHOW MASTER LOGS  
> mysql> SHOW MASTER LOGS  
>  ![7](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura7.PNG)  

__CONFIGURACIÓ AL MASTER__
Realitza una còpia de la màquina virtual a on tinguis SGBD MySQL. Aquesta nova màquina serà que farà d'esclau.  
Esbrina quina IP tenen cadascuna de les màquines (master, slave).  
Crea un backup de la BD a la màquina master utilitzant:  
> mysqldump --user=root --password=vostrepwd --master-data=2 sakila > /tmp/master_backup.sql
>  ![8](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura8.PNG)  

Edita el fitxer master_backup.sql i busca la línia que comenci per --CHANGE MASTER TO.... i busca els valors MASTER_LOG_FILE i MASTER_LOG_POS.
>  ![9](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura9.PNG)  
  
També podriem veure aquests valors des del MySQL:
> mysql> SHOW MASTER STATUS;  
>  ![9b](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura9b.PNG)  

__CONFIGURACIÓ ALS SLAVES__  
Tenim que carregar el fitxer del master que hem tret amb el mysqldump.  
> mysql -u root -ppatata < master_backup.dump  

Para el servei de MySQL.  
> systemctl stop mysqld  
                                     
Modifica el fitxer de configuració /etc/my.conf  
Comenta els paràmetres log-bin i binlog_format. D'aquesta manera desactivarem el sistema de log-bin.  
Assigna un valor al paràmetre  server-id (diferent que el del Master).  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave1/Captura1.PNG)  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave2/Captura1.JPG)  

Torna engegar el servei MySQL.
> systemctl start mysqld
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave1/Captura2.PNG)  

__CONFIGURACIÓ AL MASTER__  
Afegeix l'usuari slave amb la IP de la màquina slave.  
> mysql> CREATE USER 'slave'@'IP-SERVIDOR-SLAVE' IDENTIFIED BY 'patata';  
Afegix el permís de REPLICATION SLAVE a l'usuari que acabes de crear.
> mysql> GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%';  
> mysql> FLUSH PRIVILEGES;  
>  ![10](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/master/Captura10.PNG)  

__CONFIGURACIÓ ALS SLAVES__  
A la màquina SLAVE executa la següent comanda ajudant-te de les dades del pas 3 i 4:  
> mysql> CHANGE MASTER TO  
> -> MASTER_HOST = '<ip-servidor-master>',  
> -> MASTER_USER = 'slave',  
> -> MASTER_PASSWORD = 'patata',  
> -> MASTER_PORT = '3306',   
> -> MASTER_LOG_FILE = 'nom.000002',  
> -> MASTER_LOG_POS = <valor trobat en el pas 4>,  
> -> MASTER_CONNECT_RETRY = 10;  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave1/Captura3.PNG)  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave2/Captura3.JPG)  
  
I podem comprovar que esta a la espera d'esdeveniments des del master:  
> mysql> SHOW SLAVE STATUS\G  
>  ![4](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave1/Captura4.PNG)  
>  ![4](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici1/slave2/Captura4.JPG)  

***
[Torna enrere](https://github.com/Josep88/MP10UF2-A5)
