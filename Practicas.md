# PPS-Unidad3Actividad2-Oscar
## Prácticas

**Instala en tu navegador la extensión de Shodan y muestra la información que tenemos tanto de ip, como de dominio del sitio http://iesvalledeljerteplasencia.es**

Instalamos la extensión de **Shodan** y buscamos la dirección solicitada:

![1](/Imagenes/Shodan.png)

**Sobre la red del laboratorio PPS con kali, bWAPP, Multidillae y DVWA:<** 
Ayudándote del fichero docker-compose localiza las diferentes máquinas y puertos que deberían de tener abiertos.
- Identifica los equipos de la Red con Nmap.
- Realiza análisis de puertos en las MV.
- Encuentra los Servicios y Sistemas Operativos de las MV.
- Inspecciona los puertos con nikto.
- Busca las vulnerabilidades de las MV con los scripts de Nmap.
- Localiza los servicios web que tienen las diferentes máquinas (Wfuzz y Dirb).
- Utiliza el comando searchsploit para buscar información de explotación de vulnerabilidades presentes en linux con kernel 5

Para realizar este ejercicio, invocamos el siguiente comando:
>nmap -sV -p- -T4 127.0.0.1 

Y este es el resultado:
![2](/Imagenes/Nmap.png)

Ahí tenemos los puertos y los servicios que están corriendo dentro del escenario que tenemos montado en **/home/kali/LaboratorioPPS/docker-DVWA-bWapp-PhpMyadmin**

Vamos a lanzar **Wfuzz** invocando el siguiente comando sobre la máquina **Multidillae**:

> wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --hc 404 "http://localhost:8080/index.php?page=FUZZ"

Y esta es una parte del resultado:

![3](/Imagenes/Wfuzz.png)

Vamos a lanzar **Dirb** sobre la misma máquina invocando el comando:

>dirb http://localhost:8080 /usr/share/wordlists/dirb/common.txt 

Y esta es una parte del resultado:

![4](/Imagenes/Dirb.png)

y por último vamos a usar **Searchsploit** para buscar las vulnerabilidades de Linux con el kernel 5. Para ello invocamos el siguiente comando:

> searchsploit linux 5

Y una parte del ENORME resultado, es esta:

![5](/Imagenes/Searchsploit.png)

En resumen, herramientas como **Wfuzz, Dirb, Nmap y Searchsploit** son esenciales para la evaluación de seguridad en servidores web.

**Wfuzz y Dirb** son ideales para enumerar directorios y archivos ocultos en el servidor.

**Nmap** se utiliza para escanear puertos y descubrir servicios activos en el servidor, ayudando a identificar posibles vectores de ataque.

**Searchsploit** permite buscar exploits específicos de vulnerabilidades conocidas basadas en el software y versiones que encuentres con **Nmap o Wfuzz**.

Usadas en conjunto, estas herramientas proporcionan una evaluación integral de las posibles vulnerabilidades en un sistema, especialmente en pruebas locales de seguridad.
