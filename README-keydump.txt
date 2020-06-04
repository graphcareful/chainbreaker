Here is info related to a quick hack of chainbreaker to dump non-exportable keys on High Sierra 10.13.
See README.md original README for chainbreaker.
Credits for coding go to Anton Brazovski.

=== === === === === === === === === === === === === === === === === === === === === === === === 

Prerequisites: sudo pip install hexdump pycrypto

$ ./chainbreaker.py -f ~/Library/Keychains/login.keychain-db -p "ooUlah..#"
 [-] DB Key
[+] Symmetric Key Table:
[+] Generic Password Record
...
[+] Certificate
...
[+] Public Key Record
...
[+] Private Key Record
...

All extracted data are saved under extracted/, in certs/ and keys/.
The tool also tries to match keys and certs and saves them under associated/.
There is a simple shell script which can be used to double-check matching - match.sh.

$ ls -lR exported
total 0
drwxr-xr-x   8 abb  staff  256 Jun 11 12:57 associated
drwxr-xr-x  12 abb  staff  384 Jun 11 12:57 certs
drwxr-xr-x  11 abb  staff  352 Jun 11 12:57 keys

exported/associated:
total 0
drwxr-xr-x  4 abb  staff  128 Jun 11 12:57 1
drwxr-xr-x  4 abb  staff  128 Jun 11 12:57 2
drwxr-xr-x  4 abb  staff  128 Jun 11 12:57 3
drwxr-xr-x  4 abb  staff  128 Jun 11 12:57 4
drwxr-xr-x  4 abb  staff  128 Jun 11 12:57 5
drwxr-xr-x  4 abb  staff  128 Jun 11 12:57 6

exported/associated/1:
total 16
-rw-r--r--  1 abb  staff  1384 Jun 11 12:57 1.crt
-rw-r--r--  1 abb  staff  1217 Jun 11 12:57 4.key

exported/associated/2:
total 16
-rw-r--r--  1 abb  staff  1384 Jun 11 12:57 3.crt
-rw-r--r--  1 abb  staff  1216 Jun 11 12:57 3.key

exported/associated/3:
total 16
-rw-r--r--  1 abb  staff  1219 Jun 11 12:57 5.key
-rw-r--r--  1 abb  staff  1114 Jun 11 12:57 6.crt

exported/associated/4:
total 16
-rw-r--r--  1 abb  staff  1218 Jun 11 12:57 6.key
-rw-r--r--  1 abb  staff  1382 Jun 11 12:57 7.crt

exported/associated/5:
total 16
-rw-r--r--  1 abb  staff  1218 Jun 11 12:57 2.key
-rw-r--r--  1 abb  staff  1553 Jun 11 12:57 8.crt

exported/associated/6:
total 16
-rw-r--r--  1 abb  staff  1216 Jun 11 12:57 1.key
-rw-r--r--  1 abb  staff  1336 Jun 11 12:57 10.crt

exported/certs:
total 80
-rw-r--r--  1 abb  staff  1384 Jun 11 12:57 1.crt
-rw-r--r--  1 abb  staff  1336 Jun 11 12:57 10.crt
-rw-r--r--  1 abb  staff  1058 Jun 11 12:57 2.crt
-rw-r--r--  1 abb  staff  1384 Jun 11 12:57 3.crt
-rw-r--r--  1 abb  staff  1114 Jun 11 12:57 4.crt
-rw-r--r--  1 abb  staff  1382 Jun 11 12:57 5.crt
-rw-r--r--  1 abb  staff  1514 Jun 11 12:57 6.crt
-rw-r--r--  1 abb  staff  1500 Jun 11 12:57 7.crt
-rw-r--r--  1 abb  staff  1553 Jun 11 12:57 8.crt
-rw-r--r--  1 abb  staff  1340 Jun 11 12:57 9.crt

exported/keys:
total 72
-rw-r--r--  1 abb  staff  1217 Jun 11 12:57 1.key
-rw-r--r--  1 abb  staff   121 Jun 11 12:57 2.key
-rw-r--r--  1 abb  staff   778 Jun 11 12:57 3.key
-rw-r--r--  1 abb  staff  1217 Jun 11 12:57 4.key
-rw-r--r--  1 abb  staff  1216 Jun 11 12:57 5.key
-rw-r--r--  1 abb  staff  1219 Jun 11 12:57 6.key
-rw-r--r--  1 abb  staff  1218 Jun 11 12:57 7.key
-rw-r--r--  1 abb  staff  1218 Jun 11 12:57 8.key
-rw-r--r--  1 abb  staff  1216 Jun 11 12:57 9.key

All files are in DER format. To convert to PEM and export as P12:

$ openssl
OpenSSL> x509 -inform DER -in exported/certs/4.crt -out secret.crt
OpenSSL> rsa -inform DER -in exported/keys/4.key -out secret.key
writing RSA key
OpenSSL> pkcs12 -export -out secret.p12 -inkey secret.key -in secret.crt
Enter Export Password:
Verifying - Enter Export Password:

➜  chainbreaker git:(master) ✗ tree                                                                                                                           [11/1774]
.
├── chainbreaker.py
├── create_certs.bash
├── exported
│   ├── associated
│   │   └── 1
│   │       ├── 3.key
│   │       └── 4.crt
│   ├── certs
│   │   ├── 1.crt
│   │   ├── 2.crt
│   │   ├── 3.crt
│   │   ├── 4.crt
│   │   ├── 5.crt
│   │   ├── 6.crt
│   │   └── 7.crt
│   └── keys
│       ├── 1.key
│       ├── 2.key
│       ├── 3.key
│       ├── 4.key
│       ├── 5.key
│       ├── 6.key
│       ├── 7.key
│       └── 8.key
├── LICENSE
├── match.sh
├── pbkdf2.py
├── pyDes.py
├── README-keydump.txt
├── README.md
├── Schema.py
├── secret.crt
├── secret.key
├── secret.p12
└── validator.py

5 directories, 30 files
➜  chainbreaker git:(master) ✗ openssl x509 -inform DER -in exported/associated/1/4.crt -out secret.crt
➜  chainbreaker git:(master) ✗ openssl rsa -inform DER -in exported/associated/1/3.key -out secret.key
writing RSA key
➜  chainbreaker git:(master) ✗ openssl pkcs12 -export -out secret.p12 -inkey secret.key -in secret.crt

Enter Export Password:
Verifying - Enter Export Password:
