###
Author: Shreyas Srinivasa
NetSec 2023 - Lecture 9 Covert Channels Activity

# Cover channel using a Certificate

In this activity, we will try to extend a SSL certificate with additional module to inject some payloads. To perform this activity, we will make use of OpenSSL library. 




## Step 1: Check OpenSSL installation
OpenSSL is installed by default in Linux distributions.
To check if openssl is installaed, type the below command in Terminal
```
openssl version
```
you should see an output like below
```
OpenSSL 1.1.1u  30 May 2023
```

If you do not see the above as output, install OpenSSL using the command below
```
sudo apt install openssl
```


## Step 2: Retrieve a certificate

We will retrieve a sample certificate from aau.dk to see the contents. Use the command below for retrieving the certficate

```
openssl s_client -connect aau.dk:443
```
 you will see an output like below 
 ```
CONNECTED(00000005)
depth=2 C = US, O = Internet Security Research Group, CN = ISRG Root X1
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = R3
verify return:1
depth=0 CN = aau.dk
verify return:1 
 ---
Certificate chain
0 s:CN = aau.dk
i:C = US, O = Let's Encrypt, CN = R3
1 s:C = US, O = Let's Encrypt, CN = R3
i:C = US, O = Internet Security Research Group, CN = ISRG Root X1
2 s:C = US, O = Internet Security Research Group, CN = ISRG Root X1
i:O = Digital Signature Trust Co., CN = DST Root CA X3
 ---
Server certificate
-----BEGIN CERTIFICATE-----
MIIE5TCCA82gAwIBAgISA24h8GmQJmVQlg4dwpX2XStoMA0GCSqGSIb3DQEBCwUAMDIxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQswCQYDVQQDEwJSMzAeFw0yMzExMjUxOTMwMTdaFw0yNDAyMjMxOTMwMTZaMBExDzANBgNVBAMTBmFhdS5kazCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAJQIwhVbUgQWqow8iP9TNuRpr74AZ8abZwchIHFHux1eT6kbrwjLUtwgKwwVrf9lIAuv2fGar+ZRjvvX97tNF/po3rq+Wa4uTUtN5IoBh3Pm66v/GVjKsGfizO/NghzngOUfIRlSgAt+qTNMjNXCV/XdSW5iDXwJMufMcHw9wST86WP88qVXGR4qQVX3j++E3lp484uMT6JNUHRJSBYUDe0TPHPEHgcS8v4izbyeuN4CdWbTDZuKFVKiF24OtazfpcWHiePRw/H+Bmod4tZfNSCJ+sfbO/MfhJmwutZVnWAE1vT7T0dMomrQ8t2Zng43MyLYlE944qrvgj9F55ksROkCAwEAAaOCAhQwggIQMA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwDAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQU6gpl3qZXqFCY0dXBPPyLAeIunDswHwYDVR0jBBgwFoAUFC6zF7dYVsuuUAlA5h+vnYsUwsYwVQYIKwYBBQUHAQEESTBHMCEGCCsGAQUFBzABhhVodHRwOi8vcjMuby5sZW5jci5vcmcwIgYIKwYBBQUHMAKGFmh0dHA6Ly9yMy5pLmxlbmNyLm9yZy8wHQYDVR0RBBYwFIIGYWF1LmRrggp3d3cuYWF1LmRrMBMGA1UdIAQMMAowCAYGZ4EMAQIBMIIBBAYKKwYBBAHWeQIEAgSB9QSB8gDwAHYASLDja9qmRzQP5WoC+p0w6xxSActW3SyB2bu/qznYhHMAAAGMCC0GVQAABAMARzBFAiEAt62y39MBsp9+V4fY4dyQP9QUO6X0hTkHOLOalTmL1t8CIA3MkdqzlOBqqh8tzVjWEgiVWVmyhZ7AdxCOgmJXi3/VAHYAdv+IPwq2+5VRwmHM9Ye6NLSkzbsp3GhCCp/mZ0xaOnQAAAGMCC0HLAAABAMARzBFAiEA2mghwTR3Cag+1mtn+xxdM11PHinSDk1p025T44fcbpICIDe7EKF4wWDyPCkY3MrZH5wWWXpATahk4hqTyCi8yWmtMA0GCSqGSIb3DQEBCwUAA4IBAQAH6d0Lmu3gVET3xPX1QgkUS5/CUBYZ0lioavYt8JNU+vw/STMbZ24ebqt/Jsa8MQdBgSov16eygid89td4pBhADoaiWndfMNdVVFlizpcadCDNJHEDxDQ41dCsbHKmePad7PQL8X4jxsekxp7YDBZVeKFwDqxG5eYDqk9wrWpiILZPQxeDbRYkELwi1su8TeoZdAEFgL4Cp66vHJKz0vfxPlBo3+v+KprMjcerjbRPGW4BzvT+1vjxKZ3OdHUI+DE8DEyfZja+1+Ipfe1PiZqy4kWHzSCX0ul/nAHhcUpzuxOO/byjYk2qdir0vs+vBaJ8prZyMDEO/zbTkAjHlCr0
-----END CERTIFICATE-----
subject=CN = aau.dk
issuer=C = US, O = Let's Encrypt, CN = R3
 ---
 No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
 ---
SSL handshake has read 4610 bytes and written 401 bytes
Verification: OK
 ---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
Protocol  : TLSv1.2
Cipher  : ECDHE-RSA-AES256-GCM-SHA384
Session-ID: 59C3E6FE5EA58C944C438621751F6FE928FE958BA8B6D29E41045CADFDA48B6E
Session-ID-ctx:
Master-Key: 632298B94153167361A0C05ACAE327014C0C5ECD252405D3116D9CCFE39DFE354755C5DB0EAC445468B387FCBD96E8C7
PSK identity: None
PSK identity hint: None
SRP username: None
TLS session ticket lifetime hint: 300 (seconds)
TLS session ticket:
0000 - c2 f6 90 be 88 88 e0 81-cf 25 7f eb c1 b3 5d b0 .........%....].
0010 - 52 e8 f2 01 ef 03 7f fb-0a 22 e3 f7 1d f3 fd 61 R........".....a
0020 - c3 9e e5 77 21 7a b8 78-27 5f 60 9c c7 eb 88 8f ...w!z.x'_`.....
0030 - 25 23 43 30 6a a7 34 a9-7a 4c 62 e0 d9 f8 9b 76 %#C0j.4.zLb....v
0040 - c4 48 c8 84 8f e9 d4 70-81 9b 2d 9e 5d f4 e7 23 .H.....p..-.]..#
0050 - 8c cc f7 1b 84 b7 2c b2-11 7c 0d 7f 29 2f cf fd ......,..|..)/..
0060 - 1d f3 d4 06 7e f8 ad 7d-8d ce 07 f8 cc e8 75 46 ....~..}......uF
0070 - 1a 92 64 2f 51 6d 14 e4-43 af 50 d3 da 1a 26 25 ..d/Qm..C.P...&%
0080 - 99 09 f0 ec 0c 98 e5 ef-31 db 78 e3 69 4c 69 e8 ........1.x.iLi.
0090 - 60 45 88 72 04 de 6d ae-5a a0 e2 72 43 a9 ed 8d `E.r..m.Z..rC...
00a0 - 2c b7 ad f0 34 4e 30 7e-31 98 ef bc fe 34 6a 6e ,...4N0~1....4jn
00b0 - 6e 9c 23 73 7a 96 c8 5a-a7 aa 8a b4 2c 2b c3 11 n.#sz..Z....,+..
Start Time: 1700957970
Timeout : 7200 (sec)
Verify return code: 0 (ok)
Extended master secret: yes
 ---
 ```
 
 
## Step 3: Certificate extensions

We saw from the previous step that we can retrieve certificates using OpenSSL. In this step, we will try to embed x.509 extensions into a certificate in the form of OIDs.

To do this, first let us create a ssl.conf file for ease of use:

First open an editor
```
nano ssl.cnf
```

Paste the below and save by Ctrl+X
```
[req]
default_bit = 4096
distinguished_name = covertTextDemo
x509_extensions = v3_req
prompt = no

[covertTextDemo]
countryName             = US
stateOrProvinceName     = Virginia
localityName            = Virginia
organizationName        = ABCInc RSA 2048 M02

[v3_req]
1.1.1.1=ASN1:UTF8String:SecretCovertText

```
the data in the  `testsec`  section are just to fill up the common values that will be inserted into the certificate. What’s different is the  `v3_req`  section, which defines an area for custom OIDs and their values (and types).



## Step 4: Generating the certificate

We will use the conf file created in Step3 to create a certificate with an extension

```
# generate cert using the previous ssl.cnf file
openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout key.pem -out covertcert.pem -config ssl.cnf
```
## Step 5: Examine the output for a new field

With the previous step, we extended the certificate with a new OID and injected some random text in the cert. Now examine the new certificate with the command below:

```
openssl x509 -in covertcert.pem -text -noout
```
you will see the ouput
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            16:5d:c1:0f:cf:ed:f9:b3:16:0e:a8:2a:10:6a:b9:c6:e0:2d:6a:b6
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = US, ST = Virginia, L = Virginia, O = ABCInc RSA 2048 M02
        Validity
            Not Before: Nov 26 01:00:56 2023 GMT
            Not After : Nov 25 01:00:56 2024 GMT
        Subject: C = US, ST = Virginia, L = Virginia, O = ABCInc RSA 2048 M02
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus:
                    00:ab:e8:cc:43:1b:5d:c6:6c:19:8a:a4:2e:1e:00:
                    07:ec:0c:a1:42:a5:dc:31:0c:7c:84:11:bb:a9:43:
                    c9:19:c1:f5:31:45:53:9f:b1:b7:a8:62:09:e5:2f:
                    95:57:c7:75:6b:27:65:8b:c9:04:b9:57:c4:cb:37:
                    2b:6b:49:c8:18:55:49:2b:ca:eb:55:18:74:fa:6d:
                    16:bb:60:32:02:f6:c6:45:2f:66:53:0b:7f:af:5a:
                    9a:0f:70:9a:4b:30:00:2e:2c:47:16:05:d4:14:33:
                    a8:b8:31:de:df:8e:d2:03:e5:bf:b1:c9:1c:b4:41:
                    35:88:f6:d3:a3:92:90:68:c3:6d:26:f7:56:d9:d7:
                    0c:2f:d5:8d:60:b9:98:29:b7:26:8d:16:31:6d:2e:
                    3d:e3:4a:15:4c:20:d4:ea:87:8f:55:e1:69:54:14:
                    bf:6b:7b:cf:40:78:06:36:ed:1d:22:97:d6:1e:2d:
                    ab:15:85:33:48:83:af:51:ef:0a:67:ac:48:b5:3b:
                    f7:cc:e0:0e:88:14:1b:d7:cd:f3:da:c3:d4:23:cd:
                    97:3f:ae:ac:d6:16:7e:57:e7:41:34:52:eb:96:55:
                    0e:9a:f2:05:09:90:59:8b:a0:4e:ff:55:d8:87:e4:
                    19:50:4c:11:25:c2:6a:10:e3:07:c4:bc:51:15:fb:
                    0b:82:33:e8:42:6d:c3:4c:1c:36:ce:1d:bc:c1:bb:
                    2d:5c:f1:18:7d:aa:ac:14:b2:fc:7b:9c:69:18:30:
                    c8:75:c4:bd:87:f1:f2:7e:e8:68:ee:64:7b:af:9b:
                    04:4a:46:ab:e6:26:7e:51:5e:06:95:1d:26:5f:66:
                    01:30:1c:ad:f7:de:f5:4b:63:1a:ff:74:0b:60:a2:
                    2d:55:d9:e9:64:e9:42:ef:d6:72:02:eb:08:53:be:
                    75:07:36:af:3b:c2:f7:82:48:bb:ad:ac:4c:d6:77:
                    51:ab:ad:f3:94:15:73:02:8e:e9:87:ce:45:1a:ad:
                    31:5b:3e:92:8c:5f:88:62:9f:c9:bd:d6:6b:cc:bf:
                    5b:3d:1a:8e:ea:5f:09:5d:45:72:04:69:52:79:d1:
                    02:26:ff:c7:ce:b5:33:34:31:db:22:4d:6a:e2:5f:
                    fa:90:22:1c:ed:1a:e8:b4:b2:f9:ff:f3:e4:d0:2c:
                    b6:77:3e:10:83:90:f3:1c:ff:04:cf:b8:b4:7a:39:
                    44:f3:7c:0a:4d:8b:22:9f:3c:a5:f3:92:c7:37:ad:
                    a0:45:f3:20:a7:eb:58:6e:c5:5e:bd:bf:36:92:c5:
                    61:f1:dd:3d:c9:40:5a:8e:7c:a7:49:6e:d9:5c:ec:
                    a6:36:10:06:14:b3:8b:2d:8d:c7:9c:47:12:6a:27:
                    8f:ba:9b
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            1.1.1.1: 
                . SecretCovertText
            X509v3 Subject Key Identifier: 
                E6:C5:46:98:EE:47:4D:47:78:8E:36:0C:52:BD:FF:B5:C9:4A:FE:A7
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        71:46:8d:1d:05:2c:71:62:c9:77:97:76:28:52:08:59:32:f0:
        bf:5e:b4:9f:2b:40:28:ea:92:23:2f:ea:c9:34:34:84:74:da:
        25:6c:fc:84:18:18:59:64:86:9b:4f:a4:0b:5f:3f:77:41:3f:
        08:f0:8e:bc:63:20:43:13:0c:d8:55:42:67:5b:03:9c:9f:08:
        3f:d0:f5:51:39:a4:a2:6d:40:b9:2c:ea:d3:6c:d6:7f:2a:83:
        ab:ce:ad:ff:45:06:2c:39:a1:28:74:de:90:c3:a9:54:ba:8d:
        34:9d:f2:67:56:80:1d:d5:ec:65:f4:e8:56:ba:01:65:82:c5:
        f9:2e:1a:73:10:5d:91:86:89:cf:d4:dd:ed:7b:3f:00:1f:3b:
        49:6c:49:28:6f:35:5a:9c:4d:07:d0:85:49:a9:b2:76:13:f9:
        ee:cd:ad:2a:75:30:4d:b4:fb:ae:dd:47:55:ec:d1:7e:6c:b9:
        20:ab:bb:f1:eb:54:2d:3a:a9:11:c5:ba:52:7c:a3:72:ff:25:
        7e:34:74:26:9f:85:39:81:2e:d8:13:1d:61:60:c7:26:12:f4:
        c9:35:83:07:87:47:be:25:6d:cd:d7:17:97:cc:b1:54:bf:a0:
        b1:c0:a5:c8:c4:74:64:10:1b:62:1d:7e:ab:90:0c:37:a7:ac:
        f3:49:01:35:c5:13:9c:dc:42:3a:84:33:4b:b2:f4:9a:93:df:
        12:b7:64:43:3f:5a:e8:d8:bf:75:06:d2:fc:8e:24:46:ab:4b:
        7e:e5:a8:c7:e2:14:5b:d6:ad:bb:6c:fe:12:d4:97:df:fd:00:
        fa:ed:4a:29:df:dc:9f:ee:b0:bb:19:17:08:e1:1b:c8:a2:fb:
        66:6f:16:96:29:d9:1d:aa:85:0e:a8:38:93:bc:34:0c:53:cd:
        b3:1c:f2:7f:bf:79:27:f2:15:53:67:2c:89:e6:af:94:fe:e6:
        3f:0f:4d:e8:ce:1f:6e:9e:77:68:ea:05:8b:1c:ac:63:cd:0e:
        ba:f5:52:3d:51:25:01:2b:9f:4a:8a:56:1b:a5:7f:20:0f:22:
        ac:49:22:58:65:c6:e1:13:5f:10:04:3e:7d:47:7d:9b:ba:90:
        c7:a3:d5:57:fe:a3:45:ea:64:d1:75:85:17:1e:fb:e3:36:28:
        90:81:4f:78:11:e6:19:4a:16:2c:6d:81:47:64:6d:3d:e6:df:
        81:4a:3e:a5:12:71:6a:31:da:dc:33:c5:32:4b:b3:df:07:86:
        90:6c:60:48:75:78:a6:fc:ed:8d:50:6d:c2:62:a2:5d:1f:85:
        b3:2c:7e:8c:9f:df:55:6a:de:0b:4e:ba:2f:62:4a:0e:9c:df:
        58:a6:bb:93:d2:ee:4b:a8	
```
## Step 6: Add your Covert Secret
Replace the CovertText in the ssl.cnf file to the secret message you want to transmit and regenerate the certificate from the command in Step 4. After creation, go to the path of the created certificate and open it by a double click. Examine the details of the certificate.
