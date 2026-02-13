
Protéger l'accès physique pour le premier switch
```

Enable
Configure terminal
line console 0
password ccna
login
exec-timeout 3 30
exit
```

Pour gérer le switch à distance , il lui faut une interface virtuelle (SVI)

```
interface vlan 1
ip address 192.168.1.253 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.254
```

Activation du SSH

```
hostname SW1
ip domain-name insi.mg
crypto key generate rsa

```
Activer sur les lignes virtuelles

```
line vty 0 15
login local
transport input ssh
enable secret class
exit
```

Configurer le Routeur R1

```
Router>enable
Router#configure terminal
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.1.254 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
```

Puis pour le pc en teste l'accés ssh

```
`ssh -l admin 192.168.1.253`
```