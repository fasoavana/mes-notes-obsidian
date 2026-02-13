INSTALLATION DE UBUNTU_SERVER

![[Pasted image 20260127102350.png]]

### Installation de la pile LAMP & SNMP

**Serveur web, base de données et de PHP**

```
sudo apt install -y apache2 mariadb-server php php-mysql php-snmp php-xml php-gd php-ldap php-mbstring php-gmp php-zip snmp snmpd rrdtool
```


### **. Installation de RRDTool et SNMPd**

RRDTool va gérer tes graphiques et SNMPd permettra au serveur de s'auto-surveiller.

![[Pasted image 20260204135405.png]]

### **2. Installation et Configuration de Cacti**

Lance l'installation du paquet principal.

```
sudo apt install -y cacti
```

![[Pasted image 20260204135658.png]]

base de donné

![[Pasted image 20260204135934.png]]
