# Parmiko connection debugging

It can be frustrating to diagnose issues when using Paramiko to connect to various hosts over SSH. Different settings and versions of Paramiko can have different behavior and issues that need to be circumvented. This is a simple script to make testing and getting debug output easier.

## Setup

It is recommended to create a [virtual environment](https://docs.python.org/3/library/venv.html) so that you can test different versions of Paramiko and other libraries like cryptography without affecting anything else on your host.

Once that is created, install the requirements in requirements.txt:

```
pip install -r requirements.txt
```

Then create a json config file (e.g. config.json) and enter the proper values. **If you specify a keypath that will be used, otherwise it will try to use a password.

```
{
    "host": "localhost",
    "port": 22,
    "username": "",
    "password": "",
    "key_path": " ",
    "key_passphrase": "",
    "disable_rsa2": false
}
```

## Running the test

Run the connection test with: 

```
py connection_test.py <config_file>
```

Results will be logged to `paramiko-debug.log` as well as printed to the console.

## Troubleshooting connection issues

If you are still getting connection issues you can refer to the exceptions as well as the debug output that was logged. You may need to search for these specifics online to see if certain versions of any modules used or configurations may fix your issue.

You can use pip commands to install specific versions of any modules (such as paramiko itself or cryptography). Optionally, you can update the values in `requirements.txt` and re-run the install command to load those specific versions.

## Example

```
(venv) C:\tools\paramiko-debugger>py connection_test.py
C:\tools\paramiko-debugger\venv\lib\site-packages\paramiko\transport.py:236: CryptographyDeprecationWarning: Blowfish has been deprecated
  "class": algorithms.Blowfish,
Using config file: config.json
Trying to connect with key.
starting thread (client mode): 0x7f767df0
Local version/idstring: SSH-2.0-paramiko_2.9.2
Remote version/idstring: SSH-2.0-OpenSSH_8.9p1 Ubuntu-3
Connected (version 2.0, client OpenSSH_8.9p1)
=== Key exchange possibilities ===
kex algos: curve25519-sha256, curve25519-sha256@libssh.org, ecdh-sha2-nistp256, ecdh-sha2-nistp384, ecdh-sha2-nistp521, sntrup761x25519-sha512@openssh.com, diffie-hellman-group-exchange-sha256, diffie-hellman-group16-sha512, diffie-hellman-group18-sha512, diffie-hellman-group14-sha256
server key: rsa-sha2-512, rsa-sha2-256, ecdsa-sha2-nistp256, ssh-ed25519
client encrypt: chacha20-poly1305@openssh.com, aes128-ctr, aes192-ctr, aes256-ctr, aes128-gcm@openssh.com, aes256-gcm@openssh.com
server encrypt: chacha20-poly1305@openssh.com, aes128-ctr, aes192-ctr, aes256-ctr, aes128-gcm@openssh.com, aes256-gcm@openssh.com
client mac: umac-64-etm@openssh.com, umac-128-etm@openssh.com, hmac-sha2-256-etm@openssh.com, hmac-sha2-512-etm@openssh.com, hmac-sha1-etm@openssh.com, umac-64@openssh.com, umac-128@openssh.com, hmac-sha2-256, hmac-sha2-512, hmac-sha1
server mac: umac-64-etm@openssh.com, umac-128-etm@openssh.com, hmac-sha2-256-etm@openssh.com, hmac-sha2-512-etm@openssh.com, hmac-sha1-etm@openssh.com, umac-64@openssh.com, umac-128@openssh.com, hmac-sha2-256, hmac-sha2-512, hmac-sha1
client compress: none, zlib@openssh.com
server compress: none, zlib@openssh.com
client lang: <none>
server lang: <none>
kex follows: False
=== Key exchange agreements ===
Kex: curve25519-sha256@libssh.org
HostKey: ssh-ed25519
Cipher: aes128-ctr
MAC: hmac-sha2-256
Compression: none
=== End of kex handshake ===
kex engine KexCurve25519 specified hash_algo <built-in function openssl_sha256>
Switch to new keys ...
Adding ssh-ed25519 host key for 172.19.71.206: b'e5377f5da435dbda6e50976e8028c598'
Got EXT_INFO: {'server-sig-algs': b'ssh-ed25519,sk-ssh-ed25519@openssh.com,ssh-rsa,rsa-sha2-256,rsa-sha2-512,ssh-dss,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ecdsa-sha2-nistp256@openssh.com,webauthn-sk-ecdsa-sha2-nistp256@openssh.com', 'publickey-hostbound@openssh.com': b'0'}
Trying discovered key b'622404b4ee73d5eaff915d44b79ece20' in newkey
userauth is OK
Finalizing pubkey algorithm for key of type 'ssh-rsa'
Our pubkey algorithm list: ['rsa-sha2-512', 'rsa-sha2-256', 'ssh-rsa']
Server-side algorithm list: ['ssh-ed25519', 'sk-ssh-ed25519@openssh.com', 'ssh-rsa', 'rsa-sha2-256', 'rsa-sha2-512', 'ssh-dss', 'ecdsa-sha2-nistp256', 'ecdsa-sha2-nistp384', 'ecdsa-sha2-nistp521', 'sk-ecdsa-sha2-nistp256@openssh.com', 'webauthn-sk-ecdsa-sha2-nistp256@openssh.com']
Agreed upon 'rsa-sha2-512' pubkey algorithm
Authentication (publickey) successful!
Connected using key.
Closing client.
EOF in transport thread
```
