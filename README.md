# 🏴‍☠️ Hacking Notes & Pentesting Grimoire

![Status](https://img.shields.io/badge/Status-En%20Desarrollo-orange)
![Cert](https://img.shields.io/badge/Focus-eJPTv2-blue)
![Type](https://img.shields.io/badge/Type-Personal%20Knowledge%20Base-lightgrey)

Bienvenido a mi cerebro digital. Este repositorio contiene mi base de conocimiento personal sobre **Ciberseguridad, Pentesting y Red Teaming**. Aquí documento todo lo que aprendo durante mi preparación para certificaciones (como eJPTv2), resolución de CTFs y estudios autodidactas.

---

## ⚠️ Disclaimer (Aviso Legal)

> **Leer atentamente:**
> El contenido de este "libro" es estrictamente para **fines educativos y de investigación académica**. Las técnicas aquí descritas deben ser utilizadas únicamente en entornos controlados, laboratorios propios (DockerLabs, máquinas virtuales) o en sistemas donde se cuente con **autorización explícita**.
>
> El autor no se hace responsable del mal uso que se le pueda dar a la información aquí expuesta. **Hacking sin permiso es ilegal.**

---

## 🗺️ Mapa de Navegación

¿Qué estás buscando hoy?

### 🔍 Fase 1: Reconocimiento & Enumeración
Si estás empezando una máquina y necesitas encontrar vectores de ataque.
* Ve a **[Puertos y Servicios](recon/services/README.md)** para ver cómo atacar un puerto 21, 22, 80, 445, etc.
* Revisa **[Comandos de Red](recon/network-commands.md)** para Nmap y detección de hosts.

### 🌐 Fase 2: Hacking Web
Para auditorías de aplicaciones web (OWASP).
* **[Vulnerabilidades](web/vulns/README.md)**: SQLi, XSS, RCE, LFI, etc.
* **[CMS](web/cms/README.md)**: Guías específicas para WordPress, Joomla, Drupal.

### ⚔️ Fase 3: Explotación & PrivEsc
* **[Shells](exploitation/shells.md)**: Cómo generar Reverse Shells y mejorar la TTY.
* **[Escalada de Privilegios](privesc/linux.md)**: Guías para pasar de usuario a Root/Administrator en Linux y Windows.

### 🚩 Fase 4: Práctica (CTFs)
Soluciones y Writeups de máquinas que he resuelto.
* **[DockerLabs](writeups/dockerlabs/README.md)**: Máquinas ligeras para practicar rápido.
* **[TryHackMe & HackTheBox](writeups/thm/README.md)**: Máquinas más complejas.

---

## 🎯 Objetivo Actual: eJPTv2

Actualmente me encuentro preparando la certificación **eJPTv2**. Puedes seguir mi ruta de aprendizaje y notas específicas en la sección dedicada:
👉 **[Ir a notas de eJPTv2](ejpt/roadmap.md)**

---

## 🛠️ Herramientas Favoritas

El arsenal que uso día a día (CheatSheets rápidos):
| Herramienta | Uso Principal | Link |
| :--- | :--- | :--- |
| **Nmap** | Escaneo de red | [Ver Notas](tools/nmap.md) |
| **Burp Suite** | Proxy Web / Intercept | [Ver Notas](tools/burpsuite.md) |
| **Metasploit** | Framework de explotación | [Ver Notas](tools/metasploit.md) |
| **SQLMap** | Inyección SQL automatizada | [Ver Notas](tools/sqlmap.md) |

---

### ¿Encontraste un error?
Si ves algún comando obsoleto o tienes una sugerencia, no dudes en abrir un *Issue* o contactarme.

*Keep Hacking!* 👨‍💻
