sudo ip link set dev wlp3s0 up
[sudo] Mot de passe de faso : 
Current MAC:   18:cf:5e:c5:ce:00 (unknown)
Permanent MAC: 18:cf:5e:c5:ce:00 (unknown)
[ERROR] Could not change MAC: interface up or insufficient permissions: Too many open files in system
❯ sudo systemctl restart NetworkManager
❯ sleep 5
❯ ip a show wlp3s0
3: wlp3s0: <NO-CARRIER,BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state DORMANT group default qlen 1000
    link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff permaddr 18:cf:5e:c5:ce:00
❯ # Alternative plus radicale
sudo nmcli networking off
sudo nmcli networking on
❯ sudo nmcli device set wlp3s0 managed yes
❯ ping google.com
ping: google.com: Échec temporaire dans la résolution du nom
❯ sudo systemctl restart NetworkManager
❯ nmcli device wifi connect "L3L3" password "1234512345" ifname wlp3s0
^CErreur : nmcli terminé par le signal Interrompre (2)
Erreur : échec de l'activation de la connexion : (0) Aucun raison donnée.
❯ journalctl -fu NetworkManager
Feb 18 08:16:20 anonymas NetworkManager[8441]: <info>  [1771391780.9735] Config: added 'ssid' value 'L3L3'
Feb 18 08:16:20 anonymas NetworkManager[8441]: <info>  [1771391780.9736] Config: added 'scan_ssid' value '1'
Feb 18 08:16:20 anonymas NetworkManager[8441]: <info>  [1771391780.9737] Config: added 'bgscan' value 'simple:30:-70:86400'
Feb 18 08:16:20 anonymas NetworkManager[8441]: <info>  [1771391780.9737] Config: added 'key_mgmt' value 'WPA-PSK WPA-PSK-SHA256'
Feb 18 08:16:20 anonymas NetworkManager[8441]: <info>  [1771391780.9738] Config: added 'auth_alg' value 'OPEN'
Feb 18 08:16:20 anonymas NetworkManager[8441]: <info>  [1771391780.9738] Config: added 'psk' value '<hidden>'
Feb 18 08:16:21 anonymas NetworkManager[8441]: <info>  [1771391781.0126] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:16:22 anonymas NetworkManager[8441]: <info>  [1771391782.0036] device (wlp3s0): supplicant interface state: scanning -> associating
Feb 18 08:16:22 anonymas NetworkManager[8441]: <info>  [1771391782.4243] device (wlp3s0): supplicant interface state: associating -> disconnected
Feb 18 08:16:22 anonymas NetworkManager[8441]: <info>  [1771391782.5274] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:16:25 anonymas NetworkManager[8441]: <info>  [1771391785.8182] device (wlp3s0): supplicant interface state: scanning -> associated
Feb 18 08:16:25 anonymas NetworkManager[8441]: <info>  [1771391785.8234] device (wlp3s0): supplicant interface state: associated -> disconnected
Feb 18 08:16:25 anonymas NetworkManager[8441]: <info>  [1771391785.9233] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:16:33 anonymas NetworkManager[8441]: <info>  [1771391793.9139] device (wlp3s0): supplicant interface state: scanning -> associating
Feb 18 08:16:33 anonymas NetworkManager[8441]: <info>  [1771391793.9408] device (wlp3s0): supplicant interface state: associating -> disconnected
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.8969] audit: op="connection-update" uuid="903489b2-1962-451f-b177-f8f22364833e" name="L3L3" pid=8543 uid=1000 result="success"
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9029] device (wlp3s0): state change: config -> deactivating (reason 'new-activation', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9031] manager: NetworkManager state is now DISCONNECTING
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9041] device (wlp3s0): disconnecting for new activation request.
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9042] audit: op="connection-activate" uuid="903489b2-1962-451f-b177-f8f22364833e" name="L3L3" pid=8543 uid=1000 result="success"
Feb 18 08:16:36 anonymas NetworkManager[8441]: <warn>  [1771391796.9301] device (wlp3s0): Deactivation failed: GDBus.Error:fi.w1.wpa_supplicant1.NotConnected: This interface is not connected
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9302] device (wlp3s0): state change: deactivating -> disconnected (reason 'new-activation', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9311] manager: NetworkManager state is now DISCONNECTED
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9313] device (wlp3s0): Activation: starting connection 'L3L3' (903489b2-1962-451f-b177-f8f22364833e)
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9329] device (wlp3s0): state change: disconnected -> prepare (reason 'none', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9333] manager: NetworkManager state is now CONNECTING
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9336] device (wlp3s0): state change: prepare -> config (reason 'none', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9341] device (wlp3s0): Activation: (wifi) access point 'L3L3' has security, but secrets are required.
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9341] device (wlp3s0): state change: config -> need-auth (reason 'none', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9355] device (wlp3s0): state change: need-auth -> prepare (reason 'none', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9358] device (wlp3s0): state change: prepare -> config (reason 'none', sys-iface-state: 'managed')
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9362] device (wlp3s0): Activation: (wifi) connection 'L3L3' has security, and secrets exist.  No new secrets needed.
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9362] Config: added 'ssid' value 'L3L3'
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9362] Config: added 'scan_ssid' value '1'
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9362] Config: added 'bgscan' value 'simple:30:-70:86400'
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9363] Config: added 'key_mgmt' value 'WPA-PSK WPA-PSK-SHA256'
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9363] Config: added 'auth_alg' value 'OPEN'
Feb 18 08:16:36 anonymas NetworkManager[8441]: <info>  [1771391796.9363] Config: added 'psk' value '<hidden>'
Feb 18 08:16:37 anonymas NetworkManager[8441]: <info>  [1771391797.9323] device (wlp3s0): supplicant interface state: disconnected -> associating
Feb 18 08:16:38 anonymas NetworkManager[8441]: <info>  [1771391798.0577] device (wlp3s0): supplicant interface state: associating -> associated
Feb 18 08:16:45 anonymas NetworkManager[8441]: <info>  [1771391805.0061] device (wlp3s0): supplicant interface state: associated -> disconnected
Feb 18 08:16:45 anonymas NetworkManager[8441]: <info>  [1771391805.1091] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:16:56 anonymas NetworkManager[8441]: <info>  [1771391816.0944] device (wlp3s0): supplicant interface state: scanning -> associating
Feb 18 08:16:56 anonymas NetworkManager[8441]: <info>  [1771391816.1176] device (wlp3s0): supplicant interface state: associating -> disconnected
Feb 18 08:17:01 anonymas NetworkManager[8441]: <warn>  [1771391821.9659] device (wlp3s0): Activation: (wifi) association took too long
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9660] device (wlp3s0): state change: config -> need-auth (reason 'none', sys-iface-state: 'managed')
Feb 18 08:17:01 anonymas NetworkManager[8441]: <warn>  [1771391821.9664] device (wlp3s0): Activation: (wifi) asking for new secrets
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9676] device (wlp3s0): state change: need-auth -> prepare (reason 'none', sys-iface-state: 'managed')
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9681] device (wlp3s0): state change: prepare -> config (reason 'none', sys-iface-state: 'managed')
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9685] device (wlp3s0): Activation: (wifi) connection 'L3L3' has security, and secrets exist.  No new secrets needed.
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9686] Config: added 'ssid' value 'L3L3'
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9686] Config: added 'scan_ssid' value '1'
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9686] Config: added 'bgscan' value 'simple:30:-70:86400'
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9687] Config: added 'key_mgmt' value 'WPA-PSK WPA-PSK-SHA256'
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9687] Config: added 'auth_alg' value 'OPEN'
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9687] Config: added 'psk' value '<hidden>'
Feb 18 08:17:01 anonymas NetworkManager[8441]: <info>  [1771391821.9883] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:17:02 anonymas NetworkManager[8441]: <info>  [1771391822.9786] device (wlp3s0): supplicant interface state: scanning -> associating
Feb 18 08:17:03 anonymas NetworkManager[8441]: <info>  [1771391823.2840] device (wlp3s0): supplicant interface state: associating -> disconnected
Feb 18 08:17:03 anonymas NetworkManager[8441]: <info>  [1771391823.3881] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:17:06 anonymas NetworkManager[8441]: <info>  [1771391826.7792] device (wlp3s0): supplicant interface state: scanning -> associated
Feb 18 08:17:06 anonymas NetworkManager[8441]: <info>  [1771391826.7840] device (wlp3s0): supplicant interface state: associated -> disconnected
Feb 18 08:17:06 anonymas NetworkManager[8441]: <info>  [1771391826.8840] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:17:14 anonymas NetworkManager[8441]: <info>  [1771391834.8916] device (wlp3s0): supplicant interface state: scanning -> associating
Feb 18 08:17:22 anonymas NetworkManager[8441]: <info>  [1771391842.3813] device (wlp3s0): supplicant interface state: associating -> disconnected
Feb 18 08:17:22 anonymas NetworkManager[8441]: <info>  [1771391842.4834] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.2329] device (wlp3s0): supplicant interface state: scanning -> associated
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.2380] device (wlp3s0): supplicant interface state: associated -> disconnected
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.3380] device (wlp3s0): supplicant interface state: disconnected -> scanning
Feb 18 08:17:26 anonymas NetworkManager[8441]: <warn>  [1771391846.9663] device (wlp3s0): Activation: (wifi) association took too long
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9663] device (wlp3s0): state change: config -> need-auth (reason 'none', sys-iface-state: 'managed')
Feb 18 08:17:26 anonymas NetworkManager[8441]: <warn>  [1771391846.9677] device (wlp3s0): Activation: (wifi) asking for new secrets
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9718] device (wlp3s0): state change: need-auth -> prepare (reason 'none', sys-iface-state: 'managed')
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9733] device (wlp3s0): state change: prepare -> config (reason 'none', sys-iface-state: 'managed')
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9744] device (wlp3s0): Activation: (wifi) connection 'L3L3' has security, and secrets exist.  No new secrets needed.
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9746] Config: added 'ssid' value 'L3L3'
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9746] Config: added 'scan_ssid' value '1'
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9747] Config: added 'bgscan' value 'simple:30:-70:86400'
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9756] Config: added 'key_mgmt' value 'WPA-PSK WPA-PSK-SHA256'
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9757] Config: added 'auth_alg' value 'OPEN'
Feb 18 08:17:26 anonymas NetworkManager[8441]: <info>  [1771391846.9758] Config: added 'psk' value '<hidden>'
^C

╭─    ~ ·································· INT ✘  1m 5s   08:17:27  ─╮
╰─ 

❯ nmcli device wifi connect "B1_Salle6" password "1NS1_B1_2o25.6" ifname wlp3s0
Erreur : échec de l'activation de la connexion : (7) Des secrets étaient requis, mais aucun n'a été fourni.
❯ nmcli device wifi connect "B1_Salle6" password "1NS1_B1_2o25.6" ifname wlp3s0
❯ nmcli device wifi connect "L3L3" password "1234512345" ifname wlp3s0
Erreur : échec de l'activation de la connexion : (7) Des secrets étaient requis, mais aucun n'a été fourni.

╭─    ~ ····································· ✔  1m 15s   08:17:51  ─╮
╰─    