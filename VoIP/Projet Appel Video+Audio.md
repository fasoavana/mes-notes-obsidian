
On va utliser **Jitsi Meet sur Ubuntu**

## Mettre à jour le système

```
sudo apt update
sudo apt upgrade -y
```

## Ajouter le depôt jitsi

**Ajouter la clé GPG**
```
curl -sSL https://download.jitsi.org/jitsi-key.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/jitsi-keyring.gpg

```
**Ajouter le dépôt**
```
echo 'deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/' | sudo tee /etc/apt/sources.list.d/jitsi-stable.list > /dev/null

```
**Mettre à jour**
```
sudo apt update
```

## **Installer Jitsi Meet**

**Installation complète**
```
sudo apt install -y jitsi-meet
```
