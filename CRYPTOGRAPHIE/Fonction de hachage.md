
Une fonction de hachage H calcule un haché de n bits à partir d'un message arbitraire M

H : {0,1}* --+ {0,1} puissance n

pip install hlib
txt = input('Txt 1: ')
hash_object = hashlib.sha256(txt.encode('utf-8)) misafidy an'ilay outls ampiasaina
print (hash_object.hexdigest())

Pour changer un nombre binaire en hexadecimal il suffit de le diviser par 4
exemple : 256 bit en nombre hexadecimal est 64 puisque 256/4  = 64