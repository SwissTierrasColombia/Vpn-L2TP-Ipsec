Se debe utilizar la contraseña secreta y puede utilizar más largo y fuerte. Y este artículo utilizará única línea de comandos - se puede “traducir” a la interfaz gráfica de usuario que utilice, ya sea la interfaz web o Winbox.

En primer lugar, crear un conjunto de direcciones que los clientes VPN se consiguen una vez conectados:
```
/ip pool add name=pool-vpn ranges=10.0.0.80-10.0.0.85
```

Permitir L2TP / IPSec pase a través de la interfaz WAN. Asegúrese de que estas reglas están por encima de las normas de firewall que bloquea todo el tráfico en la interfaz WAN:
```
/ip firewall filter
add chain=input action=accept comment="VPN L2TP UDP 500" in-interface=wan protocol=udp dst-port=500 
add chain=input action=accept comment="VPN L2TP UDP 1701" in-interface=wan protocol=udp dst-port=1701
add chain=input action=accept comment="VPN L2TP 4500" in-interface=wan protocol=udp dst-port=4500
add chain=input action=accept comment="VPN L2TP ESP" in-interface=wan protocol=ipsec-esp
add chain=input action=accept comment="VPN L2TP AH" in-interface=wan protocol=ipsec-ah
```

Crear un perfil VPN que va a determinar las direcciones IP del router, los clientes VPN y el servidor DNS. Puede configurarlo para que sea fuera de la subred local, pero asegúrese de que su firewall permite la conexión:
```
/ppp profile add change-tcp-mss=yes local-address=10.0.0.1 name=vpn-profile remote-address=pool-vpn dns-server=10.0.0.1 use-encryption=yes
```

Ahora podemos crear usuarios VPN:
```
/ppp secret add name="user" password="password" profile=vpn-profile service=any

```

Configurar opciones de IPSec, es decir, los estándares de encriptación, L2TP secreto, que se pueden conectar, NAT transversal:
```
/ip ipsec peer add address=0.0.0.0/0 exchange-mode=main-l2tp nat-traversal=yes generate-policy=port-override secret="sul2tpsecreto" enc-algorithm=aes-128,3des
```
```
/ip ipsec proposal set [ find default=yes ] enc-algorithms=aes-128-cbc,3des
```
Ahora que todo está en su lugar, podemos simplemente permitir que el servidor VPN y elegir el perfil adecuado:
```
/interface l2tp-server server set authentication=mschap2 default-profile=vpn-profile enabled=yes max-mru=1460 max-mtu=1460 use-ipsec=yes
```
