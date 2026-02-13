L'appliction DIAL est la plus inmportantes d'Asterik
ON utlise la protocole SIP , LAX, PSUBR,
nous utilisions generalement SIP
Destination : il est possbile de passager le fop ou directement

On va creer Sop.conf

/etc/asterisk
[1001]
username=1001
secret = 1001
acountcode = 1001
callerid  = 1001
mailbox = 1001
context = defautlt
type = friend
host = dynamic

extention.conf
exten =-+ 124.1 Dal/SIP/1001.20
asterisk -vvr
diaplan reload
