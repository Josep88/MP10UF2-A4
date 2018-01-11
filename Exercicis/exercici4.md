### Si iniciem una transacció en el master a on hi ha una sèrie d’operacions DML (INSERT, UPDATE o DELETE) . Aquestes es guarden en el binlog?  
Quan es fa el commit de la transacció si. Al binary log es on es guarden tots els esdeveniments.

### Comprova mitjançant SHOW SLAVE STATUS, quins valors et dóna?  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A4/master/img/exercici1/slave2/Captura4.JPG)  

### Quin significat té l’opció MASTER_CONNECT_RETRY en la comanda CHANGE MASTER TO ?   
Indica la quantitat de cops que s'intentarà connectar abans de cancelar-se.  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A4/master/img/exercici1/slave2/Captura3.JPG)  

### Què fa la comanda RESET MASTER en el cas de no utilitzar GTID i utilitzar-lo?  
Eliminia tots els fitxers de logs binaris i en deixa tan sols un de nou i vuit, que torna a començar amb la nomenclatura pel .000001.

### Mira’t alguna de les taules (SHOW TABLES LIKE 'repl%') del PERFORMANCE_SCHEMA;    
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A4/master/img/exercici3/master/Captura5.PNG)  

***
[Torna enrere](https://github.com/Josep88/MP10UF2-A4)
