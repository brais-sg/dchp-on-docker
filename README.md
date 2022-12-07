# dchp-on-docker
Esto es un servidor DHCP dentro de un contenedor de docker

Para configurar el DHCP debemos editar el ```fichero dhcpd.conf```
```conf
# subnet 10.0.2.0 netmask 255.255.255.0 {    
#     range 10.0.2.100 10.0.2.200;    
#     option routers 10.0.2.1;    
#     option domain-name-servers 10.0.2.10, 10.0.2.11;    
# }

subnet 172.17.0.0 netmask 255.255.255.0 {
    range 172.17.0.2 172.17.0.255;
    option routers 172.17.0.1;
    option domain-name "asir.com";
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Host estático
    group {
        host apache {
            hardware ethernet 00:10:5a:f1:35:87;
            fixed-address 172.17.0.3;
        }
    }
}

# DHCP
authoritative;
default-lease-time 600;
max-lease-time 7200;

# DNS
option domain-name-servers 8.8.8.8, 8.8.4.4;
ddns-update-style none;

# Logging
log-facility local7;
```

Configuramos la red con ```subnet``` y establecemos la configuración para esta:
- range: Rango de IPs a ofrecer
- routers: IP del router por defecto (puerta de enlace)
- domain-name: Nombre del dominio
- domain-name-servers: Lista de servidores DNS

Luego configuramos una dirección estática para el host ```apache```

En el DHCP es importante configurarlo como autoritativo.
