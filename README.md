# Ataque-DHCP_STARVATION
# 🛡️ Ataque DHCP Starvation

**Estudiante:** Elianny Bastardo Martich  
**Matrícula:** 2025-0661  
**Materia:** Seguridad de Redes  
**Docente:** Jonathan Esteban Rondon Corniel  
**Fecha:** 5 de junio 2026  

📹 [Video demostración](https://youtu.be/jela0yqsyhc) | 📂 [Repositorio](https://github.com/info-elianny/Ataque-DHCP_STARVATION)

---

## 📌 Objetivo del Laboratorio

Analizar el funcionamiento de un ataque DHCP Starvation mediante el envío masivo de solicitudes DHCP Discover, con el propósito de observar cómo se puede agotar el conjunto de direcciones IP disponibles en un servidor DHCP y afectar la asignación de direcciones a nuevos dispositivos de la red.

---

## 🎯 Objetivo del Script

Automatizar el envío de múltiples solicitudes DHCP Discover utilizando **direcciones MAC aleatorias** con el fin de consumir las direcciones IP disponibles en el servidor DHCP y demostrar el impacto de un ataque DHCP Starvation en un entorno controlado.

---

## ⚙️ Requisitos

### Hardware y Software

- Un equipo atacante con **Kali Linux**
- Un **servidor DHCP** activo en la red
- Al menos un **dispositivo cliente** conectado a la misma red
- Todos los dispositivos en el **mismo segmento de red**
- **Python 3** instalado
- Biblioteca **Scapy** instalada
- Permisos de **administrador (root)** para ejecutar el script
- Una interfaz de red con acceso al servidor DHCP

### Instalación de dependencias

```bash
pip install scapy
```

---

## 🔧 Parámetros Utilizados

| Parámetro | Descripción |
|-----------|-------------|
| Interfaz de red | Adaptador desde el cual se enviarán las solicitudes DHCP Discover |
| Modo de ejecución | Cantidad específica de solicitudes o envío continuo hasta detenerlo |
| Cantidad de solicitudes | Número de paquetes DHCP Discover a enviar |
| Intervalo entre paquetes | Tiempo de espera entre cada solicitud |
| MACs aleatorias | Generadas automáticamente para simular múltiples clientes DHCP |

---

## 🚀 Cómo Ejecutar el Script

```bash
sudo python3 dhcp_starvation.py
```

> ⚠️ **Debe ejecutarse con permisos root (sudo)**

---

## 📋 Funcionamiento del Script

**Paso 1:** El script comprueba que el usuario tenga permisos de administrador (root), ya que la captura y el envío de tramas de red requieren privilegios elevados.

**Paso 2:** Se solicita al usuario la **interfaz de red**, el **modo de ejecución** (cantidad específica o modo continuo), la **cantidad de paquetes** y el **intervalo de tiempo** entre cada envío.

**Paso 3:** El script genera **direcciones MAC aleatorias** y envía solicitudes DHCP Discover a través de la interfaz seleccionada, simulando múltiples clientes para consumir las IPs disponibles en el servidor DHCP.

**Paso 4:** Durante la ejecución, el script muestra en pantalla la cantidad de solicitudes enviadas y la MAC utilizada en cada una, permitiendo monitorear el progreso del ataque.

---

## 📸 Capturas de Pantalla

### Topología de Red
![Topología de Red](images/image2.png)

### IP del Windows 7 por DHCP antes del ataque
![IP Windows 7 antes del ataque](images/image3.png)

### Windows 7 devolviendo la IP al servidor DHCP antes del ataque
![Windows 7 devolviendo IP](images/image4.png)

### Ejecutando el ataque
![Ejecutando el ataque](images/image5.png)

### Tabla DHCP antes del ataque
![Tabla DHCP antes del ataque](images/image6.png)

### Tabla DHCP llenándose durante el ataque
![Tabla DHCP llenándose](images/image7.png)

### Tabla DHCP saturada
![Tabla DHCP saturada](images/image8.png)

### Evidencia adicional del ataque
![Evidencia adicional](images/image9.png)

### Windows 7 solicitando IP después del ataque
![Windows 7 después del ataque](images/image10.png)

### Resultado en el cliente
![Resultado en el cliente](images/image11.png)

### Estado de la red post-ataque
![Estado post-ataque](images/image12.png)

![Estado post-ataque 2](images/image13.png)

![Estado post-ataque 3](images/image14.png)

---

## 🌐 Documentación de la Red

| Dispositivo | Interfaz | Dirección IP | Máscara de Red |
|-------------|----------|--------------|----------------|
| R-1 | E0/0 | 6.6.1.1 | 255.255.255.0 |
| SW-1 | ---- | ----- | ----- |
| Kali Linux (Atacante) | e1 | 6.6.1.11 | 255.255.255.0 |
| Windows 7 (Víctima) | e0 | 6.6.1.12 | 255.255.255.0 |
| Cloud | net | 192.168.206.135 | 255.255.255.0 |

---

## 🛡️ Contramedidas para Mitigar el Ataque

### 1. DHCP Snooping
Limita cuántos paquetes DHCP puede enviar cada puerto por segundo. Si Kali envía más del límite configurado, el puerto se apaga automáticamente.

### 2. Port Security
Limita cuántas MACs puede aprender cada puerto. Como el ataque usa MACs diferentes en cada solicitud, el puerto se bloquea al superar el límite establecido.

---

> ⚠️ **Este script es únicamente con fines educativos y de investigación en entornos controlados.**
> ⚠️ **BY Elianny**
