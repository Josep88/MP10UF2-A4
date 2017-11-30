### Es vol muntar entorn SGBD MySQL Percona amb rèplica. Es vol tenir un MySQL master a on s'aniran enviant totes les instruccions SQL d'inserció, modificació i esborrat. Es vol tenir dos MySQL  esclau del master anteriorment esmentat.  
### Cal que que al realitzar un INSERT en el master veiem les dades a l'esclau al cap d'un instant de temps.  

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



***
[Torna enrere](https://github.com/Josep88/MP10UF2-A5)
