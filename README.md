# 🛡️ Ataque DHCP Starvation

📹 [Video demostración](https://youtu.be/jela0yqsyhc) 

---

## 📌 Objetivo del Laboratorio

Analizar el funcionamiento de un ataque DHCP Starvation mediante el envío masivo de solicitudes DHCP Discover, con el propósito de observar cómo se puede agotar el conjunto de direcciones IP disponibles en un servidor DHCP y afectar la asignación de direcciones a nuevos dispositivos de la red.

### Topología de Red
![Topología de Red](https://res.cloudinary.com/drzcveg06/image/upload/v1780766632/STARVATION-1_pjhsrg.png)

---

## 🎯 Objetivo del Script

Automatizar el envío de múltiples solicitudes DHCP Discover utilizando *direcciones MAC aleatorias* con el fin de consumir las direcciones IP disponibles en el servidor DHCP y demostrar el impacto de un ataque DHCP Starvation en un entorno controlado.

---

## ⚙️ Requisitos

### Hardware y Software

- Un equipo atacante con *Kali Linux*
- Un *servidor DHCP* activo en la red
- Al menos un *dispositivo cliente* conectado a la misma red
- Todos los dispositivos en el *mismo segmento de red*
- *Python 3* instalado
- Biblioteca *Scapy* instalada
- Permisos de *administrador (root)* para ejecutar el script
- Una interfaz de red con acceso al servidor DHCP

### Instalación de dependencias

bash
pip install scapy


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

bash
sudo python3 dhcp_starvation.py


> ⚠️ *Debe ejecutarse con permisos root (sudo)*

---

## 📋 Funcionamiento del Script

*Paso 1:* El script comprueba que el usuario tenga permisos de administrador (root), ya que la captura y el envío de tramas de red requieren privilegios elevados.

*Paso 2:* Se solicita al usuario la *interfaz de red, el **modo de ejecución* (cantidad específica o modo continuo), la *cantidad de paquetes* y el *intervalo de tiempo* entre cada envío.

*Paso 3:* El script genera *direcciones MAC aleatorias* y envía solicitudes DHCP Discover a través de la interfaz seleccionada, simulando múltiples clientes para consumir las IPs disponibles en el servidor DHCP.

*Paso 4:* Durante la ejecución, el script muestra en pantalla la cantidad de solicitudes enviadas y la MAC utilizada en cada una, permitiendo monitorear el progreso del ataque.

---

## 📸 Capturas de Pantalla

### IP del Windows 7 por DHCP antes del ataque
![IP Windows 7 antes del ataque](https://res.cloudinary.com/drzcveg06/image/upload/v1780766884/STARVATION-CAP-1_wrmuan.png)

### Windows 7 devolviendo la IP al servidor DHCP antes del ataque
![Windows 7 devolviendo IP](https://res.cloudinary.com/drzcveg06/image/upload/v1780766936/STARVATUON-CAP-2_jdac63.png)

### Ejecutando el ataque
![Ejecutando el ataque](https://res.cloudinary.com/drzcveg06/image/upload/v1780766979/STARVATION-CAP-3_binfsv.png)

![Ejecutando el ataque 2](https://res.cloudinary.com/drzcveg06/image/upload/v1780767173/STARVATION-CAP-4_yrdxvn.png)

### Tabla DHCP antes del ataque
![Tabla DHCP antes del ataque](https://res.cloudinary.com/drzcveg06/image/upload/v1780767206/STARVATION-5_dnpupl.png)

### Tabla DHCP llenándose durante el ataque
![Tabla DHCP llenándose](https://res.cloudinary.com/drzcveg06/image/upload/v1780767234/STRAVATION-6_mlmjcu.png)

![Tabla DHCP llenándose 2](https://res.cloudinary.com/drzcveg06/image/upload/v1780767259/STARVATION-6-6_rau2qr.png)

### Windows 7 solicitando IP después del ataque
![Windows 7 después del ataque](https://res.cloudinary.com/drzcveg06/image/upload/v1780767293/windows-stavation_n4kxla.png)

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


SW(config)# ip dhcp snooping
SW(config)# ip dhcp snooping vlan 1
SW(config)# interface range e0/3, e0/1
SW(config-if)# ip dhcp snooping limit rate 10
SW(config-if)# exit


![DHCP Snooping resultado](https://res.cloudinary.com/drzcveg06/image/upload/v1780767322/STARVATION-M-1_zkkbnl.png)

![DHCP Snooping evidencia 2](https://res.cloudinary.com/drzcveg06/image/upload/v1780767344/STARVATION-M-2_xs8vnq.png)

### 2. Port Security
Limita cuántas MACs puede aprender cada puerto. Como el ataque usa MACs diferentes en cada solicitud, el puerto se bloquea al superar el límite establecido.


SW(config-if)# switchport port-security
SW(config-if)# switchport port-security maximum 2
SW(config-if)# switchport port-security violation shutdown

![Port Security resultado](https://res.cloudinary.com/drzcveg06/image/upload/v1780767373/STARVATION-M-3_t6v9pj.png)

---

> ⚠️ *Este script es únicamente con fines educativos y de investigación en entornos controlados.*  
> ⚠️ *BY: Elianny*
