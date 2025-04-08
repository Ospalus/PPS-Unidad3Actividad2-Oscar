# PPS-Unidad3Actividad2-Oscar
## Aprendizaje

1. Obtención de información pública:
 
 **Protocolo WHOIS**

El protocolo WHOIS permite consultar información sobre dominios registrados:

- Uso básico: *whois dominio.com*

Proporciona datos como:

- Registrante del dominio
- Fechas de creación y expiración
- Servidores DNS asociados
- Información de contacto (a veces oculta por privacidad)


**Web DomainTools**

Herramienta online que amplía la información WHOIS con:

- Historial de cambios en el dominio
- Relación con otros dominios
- Información de IP asociada
- Herramientas como Reverse IP, DNS History, etc.

**DNSRecon**
- Objetivo: Enumerar registros DNS (A, MX, TXT, subdominios) y detectar posibles fugas de información.

Comandos útiles:

- Escaneo básico de un dominio:

>dnsrecon -d ejemplo.com

- Búsqueda de subdominios (fuerza bruta con diccionario):

>dnsrecon -d ejemplo.com -t brt -D /usr/share/wordlists/subdomains.txt

- Consultar registros DNS específicos (MX, TXT, etc.):

>dnsrecon -d ejemplo.com -t std

- Reverse DNS Lookup (buscar dominios en una IP):

>dnsrecon -r 192.168.1.0/24

## **¿Cómo podemos utilizar Nmap y nikto, para buscar equipos, puertos abiertos, servicios, vulnerabilidades?**

Uso de Nmap y Nikto para Escaneo de Red y Vulnerabilidades:

1. **Nmap (Escaneo de equipos, puertos y servicios)**

Objetivo: Descubrir hosts, puertos abiertos y servicios en la red.

**Comandos básicos:**

- Escaneo de hosts en una red:

>nmap -sn 192.168.1.0/24 # Escaneo Ping (descubre equipos activos).
- Escaneo de puertos abiertos:
>nmap -p 1-1000 192.168.1.1 # Escanea puertos en un rango
>nmap -p- 192.168.1.1 # Escanea TODOS los puertos (65535)

- Detección de servicios y versiones:
>nmap -sV 192.168.1.1 # Identifica servicios y versiones

- Escaneo agresivo (incluye detección de OS y scripts básicos):
>nmap -A 192.168.1.1

-Uso de scripts de vulnerabilidad (NSE - Nmap Scripting Engine):
>nmap --script vuln 192.168.1.1 # Busca vulnerabilidades conocidas

2. **Nikto (Escaneo de vulnerabilidades en servicios web)**

Objetivo: Analizar servidores web (HTTP/HTTPS) en busca de vulnerabilidades conocidas.

**Comandos básicos:**
- Escaneo básico a un servidor web:

>nikto -h http://192.168.1.1

- Escaneo con puerto específico:

>nikto -h http://192.168.1.1 -p 8080

- Guardar resultados en un archivo:

>nikto -h http://192.168.1.1 -output resultado.txt

- Flujo de trabajo recomendado:

>Descubrimiento de hosts con nmap -sn.

- Escaneo de puertos y servicios con nmap -sV.

- Análisis de vulnerabilidades con nmap --script vuln y nikto (si hay servicios web).

- Profundizar con herramientas como Metasploit, Burp Suite o OWASP ZAP según los resultados.

*Nota: Siempre usa estas herramientas con autorización en redes propias o bajo consentimiento. Su uso no ético es ilegal.*

## **¿Cómo utilizar Wfuzz, Dirb para localizar recursos web en servidores?**


## 1. WFuzz (Para fuzzing avanzado)

### Instalación
```bash
sudo apt install wfuzz
```

### Comandos útiles

- **Escaneo básico**:
  ```bash
  wfuzz -w /usr/share/wordlists/dirb/common.txt http://ejemplo.com/FUZZ
  ```

- **Filtrar respuestas (ej. omitir 404)**:
  ```bash
  wfuzz -w wordlist.txt --hc 404 http://ejemplo.com/FUZZ
  ```

- **Buscar extensiones específicas**:
  ```bash
  wfuzz -w wordlist.txt -z list,php-txt-html http://ejemplo.com/FUZZ.FUZ2Z
  ```

---

## 2. Dirb (Para escaneos rápidos)

### Instalación
```bash
sudo apt install dirb
```

### Comandos útiles

- **Escaneo básico**:
  ```bash
  dirb http://ejemplo.com /usr/share/wordlists/dirb/common.txt
  ```

- **Usar wordlist grande**:
  ```bash
  dirb http://ejemplo.com /usr/share/wordlists/dirb/big.txt
  ```

- **Ignorar códigos HTTP (ej. 403)**:
  ```bash
  dirb http://ejemplo.com -N 403
  ```

### Wordlists recomendadas

- `/usr/share/wordlists/dirb/common.txt`
- `/usr/share/wordlists/dirb/big.txt`
- `/usr/share/wordlists/wfuzz/others.txt`

---

## Diferencia principal

- **Wfuzz**: Más potente y configurable  
- **Dirb**: Más simple y rápido

---

> ⚠️ **Nota importante:** Solo usar en sitios con permiso explícito. El uso no autorizado es ilegal.

## **¿Que scripts que podemos utilizar con Nmap para la búsqueda de vulnerabilidades?**

# Scripts NSE de Nmap para Búsqueda de Vulnerabilidades

Nmap incluye un motor de scripts llamado **NSE (Nmap Scripting Engine)**, que permite ejecutar scripts para detectar vulnerabilidades, enumerar servicios, y más.

---

## 🔍 Detección general de vulnerabilidades

```bash
nmap --script vuln <IP>
```

- Usa todos los scripts etiquetados como `vuln`.
- Ideal para un escaneo rápido de vulnerabilidades comunes.

---

## ✔️ Scripts específicos útiles

### SMB (puerto 445)

```bash
nmap -p 445 --script smb-vuln* <IP>
```

- `smb-vuln-ms17-010`: Detecta EternalBlue.
- `smb-vuln-ms08-067`: Detecta vulnerabilidad crítica en SMB.

---

### HTTP (puertos 80, 443)

```bash
nmap -p 80,443 --script http-vuln* <IP>
```

- `http-vuln-cve2017-5638`: Apache Struts RCE.
- `http-slowloris`: Ataque de denegación de servicio tipo Slowloris.

---

### FTP (puerto 21)

```bash
nmap -p 21 --script ftp-vsftpd-backdoor <IP>
```

- Detecta la backdoor en vsFTPd 2.3.4.

---

### SSL/TLS (puerto 443)

- **Heartbleed (CVE-2014-0160):**
  ```bash
  nmap --script ssl-heartbleed -p 443 <IP>
  ```

- **POODLE (SSLv3 fallback attack):**
  ```bash
  nmap --script ssl-poodle -p 443 <IP>
  ```

---

## 📦 Buscar scripts disponibles

Listar todos los scripts:
```bash
ls /usr/share/nmap/scripts/
```

Buscar por palabra clave:
```bash
locate *.nse | grep <palabra>
```

---

## 🧠 Ejemplo completo

```bash
nmap -sV --script vuln -oN resultado.txt <IP>
```

---

## 📚 Recomendación adicional

Escaneo de puertos + scripts por defecto:

```bash
nmap -sC -sV -p- <IP>
```

- `-sC`: Scripts por defecto
- `-sV`: Detección de versiones
- `-p-`: Todos los puertos

---

> ⚠️ **Importante:** Solo usar en entornos autorizados. Escanear redes sin permiso es ilegal.

## **¿Cómo podemos buscar información de explotación de vulnerabilidades con searchsploit?**

