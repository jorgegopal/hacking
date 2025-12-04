# 🛡️ Hacking Notes & Pentesting Repository

![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Focus](https://img.shields.io/badge/Focus-Web%20Hacking%20%26%20CTFs-blue)
![Cert](https://img.shields.io/badge/Prep-eJPTv2-orange)

Bienvenido a mi repositorio personal de ciberseguridad. Este espacio contiene mis notas de estudio, hojas de trucos (cheatsheets), scripts y soluciones (writeups) de máquinas, con un fuerte enfoque en la preparación para la certificación **eJPTv2** y técnicas avanzadas de **Web Hacking**.

---

## ⚠️ Disclaimer

> **Aviso Legal:**
> Todo el contenido de este repositorio (comandos, técnicas, exploits) tiene fines exclusivamente **educativos y de formación ética**. El uso de esta información para atacar objetivos sin autorización previa es ilegal.

---

## 📚 Contenido del Repositorio

El conocimiento está dividido en las siguientes áreas principales según mi metodología actual:

### 1. 🕵️ Reconocimiento y Enumeración
Notas detalladas sobre comandos de red, host y enumeración específica por puertos y servicios:
* **Protocolos:** FTP (21), SSH (22), Telnet (23), SMTP (25), Web (80/443), RPC (111), SMB (135/445), Oracle (1521), MySQL (3306), RDP (3389), WinRM (5985/5986).
* **CMS:** Enumeración de Joomla, WordPress, Drupal y Magento.
* **Tecnologías:** Microsoft IIS, Jenkins, Tomcat, WebDAV.

### 2. 🌐 Hacking Web (OWASP & Más)
Una extensa colección de vulnerabilidades y técnicas de explotación web:
* **Inyecciones:** SQL Injection (SQLi), NoSQL, XPath, LDAP, SSTI, CSTI, CSS Injection, Latex Injection.
* **Fallas de Lógica y Auth:** XSS, CSRF, IDORs, Race Condition, Session Puzzling, Type Juggling, JWT Attacks, API Abuse.
* **Archivos y Deserialización:** LFI, RFI, Abuso de subida de archivos, Deserialización (Python Pickle, etc.), XXE.
* **Otros:** Shellshock, HTTP Request Smuggling, Prototype Pollution, Padding Oracle, GraphQL, SSRF, Open Redirect, Transferencia de Zona.

### 3. ⚔️ Explotación y Acceso
* **Shells:** Guías para Bind Shells, Reverse Shells y Forward Shells.
* **Tratamiento de la TTY:** Estabilización de terminales.
* **Buffer Overflow:** Metodología básica y explotación.
* **Password Cracking:** Uso de Hashcat, John the Ripper y diccionarios.

### 4. 🪜 Escalada de Privilegios
* **Linux:** Sudo, SUID, Kernel exploits, Cronjobs y "tips curiosos".
* **Windows:** Servicios explotados frecuentemente, Psexec, WinRM.

### 5. 🧟 Post-Explotación & Pivoting
* Técnicas de Pivoting manual y con Metasploit (usando máquinas Windows).
* Persistencia y movimiento lateral.

---

## 🛠️ Arsenal de Herramientas
Guías de uso y "CheatSheets" para las herramientas que utilizo diariamente:

| Categoría | Herramientas |
| :--- | :--- |
| **Web Fuzzing** | `Ffuf`, `Gobuster`, `Wfuzz`, `Dirbuster` |
| **Escaneo** | `Nmap`, `Nessus`, `Nikto` |
| **Explotación** | `Metasploit`, `Burp Suite`, `SQLMap`, `Hydra` |
| **Contenedores** | `Docker` |

---

## 🐍 Scripting & Coding
Notas sobre automatización y creación de herramientas propias:
* **Python:** Formateo de cadenas, listas, operadores, scripting para deserialización.
* **Comandos:** Referencia rápida de comandos para Linux, Windows y MySQL.

---

## 🎓 Preparación eJPTv2
Sección dedicada exclusivamente a la certificación **Junior Penetration Tester**:
* Resolución de laboratorios oficiales.
* Máquinas recomendadas para practicar.
* Hojas de ruta de estudio.

---

## 🚩 CTF Writeups (Soluciones)
Mis soluciones paso a paso para máquinas de diferentes plataformas:

* **🐳 DockerLabs:** ChocolateFire, Domain, FindYourStyle, Trust, Walking CMS.
* **🔥 TryHackMe:** VulnNet, ChillHack, Blog, Ignite, Pickle Morty, Kenobi, Vulnversity.
* **📦 HackTheBox:** Expressway.
* **🕷️ VulnHub:** IMF 1, Casino Royale, Infovere, Presidential, Symfonos.
* **💻 HackMyVM:** Friendly.

---

### 📝 Notas
Este repositorio está en constante evolución ("Work in Progress"). La estructura puede cambiar a medida que añado nuevas máquinas y técnicas.

**Happy Hacking!** 👨‍💻
