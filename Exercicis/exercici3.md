### Entorn amb replicació semisíncrona amb master passiu i esclau al master actiu.
  
Utilitzarem la mateixa configuració que a la replicació per GTID. Així la replicació del master actiu amb l'esclau ja esta configurada.  
El que fa de master passiu també necesita d'aquesta replicació per GTID, peró s'han de canviar els fitxers de configuració per de que no sigui esclau i sigui master passiu.  

Primer de tot comprovem quins plugins tenim instal·lats, busquem si tenim el _rpl_semi_sync_master_:  
> mysql> SHOW PLUGINS;  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master/Captura1.PNG)  
  
En cas de que no el tinguem instal·lat haurem d'instal·lar-lo, en funció de quin master sigui.

__CONFIGURACIÓ AL MASTER ACTIU__  
  
> mysql> INSTALL PLUGIN rpl_semi_sync_master SONAME 'semisync_master.so';  
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master/Captura2.PNG)  
Confirmem que s'ha instal·lat:  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master/Captura3.PNG)  

__CONFIGURACIÓ AL MASTER PASSIU__  
  
> mysql> INSTALL PLUGIN rpl_semi_sync_slave SONAME 'semisync_slave.so';  
>  ![1](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master2/Captura9.PNG)  
Confirmem que s'ha instal·lat:  
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master2/Captura10.PNG)  
  
  
Ara tenim que habilitar la replicació semisíncrona:  
  
__CONFIGURACIÓ AL MASTER ACTIU__  
  
> mysql> SET GLOBAL rpl_semi_sync_master_enabled = 1;   
> mysql> SET GLOBAL rpl_semi_sync_master_timeout = 10000;  
O també el podem activar al fitxer _/etc/my.cnf_:  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master/Captura4b.PNG)  
  
I comprovem que estan configurats correctament els paràmetres:  
>  ![3](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master/Captura5.PNG)  

__CONFIGURACIÓ AL MASTER PASSIU__  
  
Habilitem l'esclau, el reiniciem i en comprovem l'estat:
>  ![2](https://raw.githubusercontent.com/Josep88/MP10UF2-A5/master/img/exercici3/master2/Captura11.PNG)  
  
  
Ara ja estan configurats i funciona la replicació semisíncrona entre masters i la replicació master-slave amb l'altre màquina.  
  
Descripció dels paràmetres que ens interesen:  
Rpl_semi_sync_master_status -> Indica si la replicació semisíncrona esta habilitada al master.  
Rpl_semi_sync_master_yes_tx -> La quantitat de commits que s'han enviat correctament al esclau.  
Rpl_semi_sync_master_no_tx  -> La quantitat de commits que no s'han pogut enviar al esclau correctament.  
  
***
[Torna enrere](https://github.com/Josep88/MP10UF2-A5)
