### Es vol muntar un entorn SGBD MySQL Percona amb rèplica similar a l’anterior, però aquesta vegada es vol realitzar mitjançant GTID.

Primer de tot em de fer igual que al primer exercici. Hi ha que fer el mysqldump al master i passar-li els fitxers als dos slaves per tenir la mateixa base de dades.  
  
__CONFIGURACIÓ AL MASTER__  
  
Fitxer de configuració _/etc/my.conf_:  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/master/1.PNG)  

Creem l'usuari que utilitzaran els slaves per rebre la replicació:  
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/master/2.PNG)  

__CONFIGURACIÓ ALS SLAVES__  
Configuració de l’arxiu _/etc/my.cnf_:  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/slave1/Captura1.PNG)  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/slave2/Captura1.JPG)  

Configuració del master al mysql slave:  
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/slave1/Captura2.PNG)  
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/slave2/Captura2.JPG)  

I comprovació del funcionament del slave:    
> SHOW SLAVE STATUS\G  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/slave1/Captura3.PNG)  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici2/slave2/Captura3.JPG)  

***
[Torna enrere](https://github.com/Josep88/MP10UF2-A5)
