# Instalación en GNOME (Distribuciones basadas en Fedora, OpenSuse y Debian)

1) Se necesita instalar el paquete de conexión VPN L2TP para GNOME esto permite la conexión de VPN con interfaz gráfica GUI.

Debian:
```bash
sudo apt install l2tp-ipsec-vpn-daemon
```
OpenSuse:
```bash
sudo zypper install NetworkManager-l2tp-gnome
```
Fedora:
```bash
sudo dnf install NetworkManager-l2tp-gnome
```

2) Seleccionar el tipo de conexión.
- Ir al panel de configuraciones de GNOME "Settings".
- En la categoría "Hardware" seleccionar "Network".
- Agregue una nueva conexión haciendo clic en el botón "+".
- En el diálogo "Add Network Connection" seleccione "VPN" y luego "Layer 2 Tunneling Protocol (L2TP)"

3) Configurar la conexión.

Utilice los siguientes datos de conexión.

Name: SWISSPHOTO
Gateway: portal.proadmintierra.info
Username: su_nombre_de_usuario
Password: su_password
Group Name: default
Pre-shared key: pregunte_a_su_admin
Autentication methods: MSCHAPV2
Use Point to Point encription (MPPE) Security: 128 bit

