# Laboratorio #2 Administración de Redes

## Integrantes 
Bernal Pérez Luis Ángel

## La documentación contenida en esta Wiki tendra la siguiente información:

-1.-Diagrama

-2.- Subneteo

-3.- Configuraciones

-Respuestas a las preguntas solicitadas en la aplicación presentada.

-Validaciones


## 1.- Diagrama
![image](https://github.com/ThirtforBep/Redes/blob/main/Diagrama.png)

## 2.- Subneteo
### Subneteo INTRANET (BOG)
![image](https://github.com/ThirtforBep/Redes/blob/main/Subneteo1.png)
### Subneteo INTRANET (MAD)
![image](https://github.com/ThirtforBep/Redes/blob/main/Subneteo2.png)
### Subneteo DMZ -ISP/BOG
![image](https://github.com/ThirtforBep/Redes/blob/main/Subneteo3.png)
### Subneteo ISP/MAD
![image](https://github.com/ThirtforBep/Redes/blob/main/Subneteo4.png)
### Subneteo Internet
![image](https://github.com/ThirtforBep/Redes/blob/main/Subneteo5.png)

## 3.- Configuracion
### 3.1.1- Configuracion basica de routers
Router>enable

Router#config term

Enter configuration commands, one per line.  End with CNTL/Z.

Router(config)#hostname ISP_NET

ISP_NET(config)#line console 0

ISP_NET(config-line)#password cisco

ISP_NET(config-line)#login

ISP_NET(config-line)#exit

ISP_NET(config)#banner motd #Se encuentra configurando el Router ISP_NET. Ingrese su contrasena#

ISP_NET(config)#exit

ISP_NET#
### 3.1.2- Configurar router para ipv6
ISP_FL>enable

Password: cisco

ISP_FL#config term

Enter configuration commands, one per line.  End with CNTL/Z.

ISP_FL(config)#IPv6 unicast-routing
### 3.1.3- Encender interfaz
R1_BOG(config)#interface fa0/1

R1_BOG(config-if)#no shutdown

R1_BOG(config-if)#exit

### 3.1.4-Hacer encapsulacion
R1_BOG(config-subif)#interface fa0/1.25

R1_BOG(config-subif)#encapsulation dot1q 25

R1_BOG(config-subif)#ipv6 address 2001:1200:A5:2::1/64

R1_BOG(config-subif)#interface fa0/1.50

R1_BOG(config-subif)#encapsulation dot1q 50

R1_BOG(config-subif)#ipv6 address 2001:1200:A5:3::1/64

R1_BOG(config-subif)#interface fa0/1.75

R1_BOG(config-subif)#encapsulation dot1q 75

R1_BOG(config-subif)#ipv6 address 2001:1200:A5:4::1/64

R1_BOG(config-subif)#interface fa0/1.100

R1_BOG(config-subif)#encapsulation dot1q 100

R1_BOG(config-subif)#ipv6 address 2001:1200:A5:5::1/64

R1_BOG(config-subif)#interface fa0/1.99

R1_BOG(config-subif)#encapsulation dot1q 99

R1_BOG(config-subif)#ipv6 address 2001:1200:A5:1::1/64

R1_BOG(config-subif)#exit

### 3.1.5-Usar router como proveedor DHCP
R1_BOG(config-dhcpv6)#address prefix 2001:1200:A5::/48

R1_BOG(config-dhcpv6)#dns-server 2001:1200:C5:1::3

R1_BOG(config-dhcpv6)#exit

R1_BOG(config)#interface fa0/1

R1_BOG(config-if)#ipv6 dhcp server bogota

R1_BOG(config-if)#ipv6 nd other-config-flag

R1_BOG(config-if)#exit

R1_BOG(config)#interface fa0/1.25

R1_BOG(config-subif)#ipv6 dhcp server bogota

R1_BOG(config-subif)#ipv6 nd other-config-flag

R1_BOG(config-subif)#exit

R1_BOG(config)#interface fa0/1.50

R1_BOG(config-subif)#ipv6 dhcp server bogota

R1_BOG(config-subif)#ipv6 nd other-config-flag

R1_BOG(config-subif)#exit

R1_BOG(config)#interface fa0/1.75

R1_BOG(config-subif)#ipv6 dhcp server bogota

R1_BOG(config-subif)#ipv6 nd other-config-flag

R1_BOG(config-subif)#exit

R1_BOG(config)#interface fa0/1.100

R1_BOG(config-subif)#ipv6 dhcp server bogota

R1_BOG(config-subif)#ipv6 nd other-config-flag

R1_BOG(config-subif)#exit

R1_BOG(config)#interface fa0/1.99

R1_BOG(config-subif)#ipv6 dhcp server bogota

R1_BOG(config-subif)#ipv6 nd other-config-flag

R1_BOG(config-subif)#exit

R1_BOG(config)#exit

### 3.1.6-Correr configuracion del router bogota
R1_BOG#copy running-config startup-config

Destination filename [startup-config]? 

Building configuration...

[OK]
### 3.1.7-No pueden pasar por el 80
R1_BOG(config)#ipv6 access-list NO-HTTP-BOGOTA

R1_BOG(config-ipv6-acl)#deny tcp 2001:1200:A5:2::/64 any eq 80

R1_BOG(config-ipv6-acl)#exit

R1_BOG(config)#interface Fa0/0

R1_BOG(config-if)#ipv6 traffic-filter NO-HTTP-BOGOTA out

R1_BOG(config-if)#exit

### 3.1.8-Pueden pasar por el 443
R1_BOG(config)#ipv6 access-list HTTPs-BOGOTA

R1_BOG(config-ipv6-acl)#permit tcp 2001:1200:A5:2::/64 any eq 443

R1_BOG(config-ipv6-acl)#exit

R1_BOG(config)#interface Fa0/0

R1_BOG(config-if)#ipv6 traffic-filter HTTP-BOGOTA out

R1_BOG(config-if)#exit

R1_BOG(config)#exit

### 3.1.9-Ver datos
R1_BOG#show access-lists

IPv6 access list NO-HTTP-BOGOTA

    deny tcp 2001:1200:A5:2::/64 any eq www
    
IPv6 access list HTTPs-BOGOTA

    permit tcp 2001:1200:A5:2::/64 any eq 443
### 3.2.1- Configuracion basica de switches
Router>enable

Router#config term

Enter configuration commands, one per line.  End with CNTL/Z.

Router(config)#hostname ISP_NET

ISP_NET(config)#line console 0

ISP_NET(config-line)#password cisco

ISP_NET(config-line)#login

ISP_NET(config-line)#exit

ISP_NET(config)#banner motd #Se encuentra configurando el Router ISP_NET. Ingrese su contrasena#

ISP_NET(config)#exit

ISP_NET#
### 3.2.2- Habilitar ipv6 e ipv4 en switches
SW4_Intranet>enable

Password: cisco

SW4_Intranet#config term

Enter configuration commands, one per line.  End with CNTL/Z.

SW4_Intranet(config)#SDM PREFER DUAL-IPV4-AND-IPV6 DEFAULT

Changes to the running SDM preferences have been stored, but cannot take effect until the next reload.

Use 'show sdm prefer' to see what SDM preference is currently active.

SW4_Intranet(config)#end

SW4_Intranet#
### 3.2.3- Apagar interfaces para hacer vlans

SW3_Intranet(config)#ipv6 route ::/0 2001:1200:A5:1::1

SW3_Intranet(config)#interface range fa0/1-24

SW3_Intranet(config-if-range)#shutdown

### 3.2.4- Asignar interfaces troncales
SW3_Intranet(config-if-range)#exit

SW3_Intranet(config)#interface range fa0/1-5

SW3_Intranet(config-if-range)#switchport mode trunk

SW3_Intranet(config-if-range)#switchport trunk native vlan 99

SW3_Intranet(config-if-range)#no shutdown 

### 3.2.5- Crear vlans
SW3_Intranet(config)#vlan 25

SW3_Intranet(config-vlan)#name Guest

SW3_Intranet(config-vlan)#vlan 50

SW3_Intranet(config-vlan)#name Internal

SW3_Intranet(config-vlan)#vlan 75

SW3_Intranet(config-vlan)#name Server

SW3_Intranet(config-vlan)#vlan 100 

SW3_Intranet(config-vlan)#name Voip

SW3_Intranet(config-vlan)#vlan 99

SW3_Intranet(config-vlan)#name Native

SW3_Intranet(config-vlan)#exit

### 3.2.6- Rangos de vlans
SW3_Intranet(config)#interface range fa0/6-11

SW3_Intranet(config-if-range)#Switchport access vlan 25

SW3_Intranet(config-if-range)#exit

SW3_Intranet(config)#interface range fa0/12-16

SW3_Intranet(config-if-range)#Switchport access vlan 50

SW3_Intranet(config-if-range)#exit

SW3_Intranet(config)#interface range fa0/17-20

SW3_Intranet(config-if-range)#Switchport access vlan 75

SW3_Intranet(config-if-range)#exit

SW3_Intranet(config)#interface range fa0/21-24

SW3_Intranet(config-if-range)#Switchport access vlan 100

SW3_Intranet(config-if-range)#exit

SW3_Intranet(config)#interface range fa0/1-5

SW3_Intranet(config-if-range)#Switchport access vlan 99

SW3_Intranet(config-if-range)#exit

### 3.2.7- Asignar IPV6 a las interfaces 99 y reactivar interfaces
SW3_Intranet(config)#interface vlan 99

SW3_Intranet(config-if)#ipv6 address 2001:1200:A5:1::2/64

SW3_Intranet(config-if)#exit

SW3_Intranet(config)#interface range fa0/6-24

SW3_Intranet(config-if-range)#no shutdown

SW3_Intranet(config-if-range)#exit

SW3_Intranet(config)#exit

SW3_Intranet#

%SYS-5-CONFIG_I: Configured from console by console

SW3_Intranet#copy running-config startup-config 

Destination filename [startup-config]? 

Building configuration...

[OK]


