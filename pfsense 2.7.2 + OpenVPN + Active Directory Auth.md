# Pfsense 2.7.2 + OpenVPN + Active Directory Auth
## 1 - Crear entidad certificadora
---
+ **System/Certificate/Authorities/Add ->**
**Descriptive name:** ``Openvpn_CA``
**Method:** ``Create an internal Certificate Authority``
**Trust Store:**  [x] ``Add this Certificate Authority to the Operating System Trust Store``
**Randomize Serial:** `default`
**Internal Certificate Authority:** `defaul`
**Key type:** `default`
**Digest Algorithm:** `default`
**weaker digest algorithms invalid.** `default`
**Lifetime (days)** `default`
**Common Name:** `Openvpn_CA`
**Country Code:** `CU`
**State or Province**: `PROVINCIA`
**City:** `CIUDAD`
**Organization:** `Nombre empresa`
**Organizational Unit:** `My department name`
## 2 - Crear certificado
---
+ **System/Certificates/Certificates/Add ->**
***Add/Sign a New Certificate***
**Method:** `Create an internal Certificate`
**Descriptive name:** `Openvpn_Cert`
***Internal Certificate***
**Certificate authority:** `Openvpn_CA`
**Key type:** `default`
**Digest Algorithm:** `default`
**Lifetime (days):** `365`
**Common Name:** `Openvpn_Cert`
**Country Code:** `CU`
**State or Province**: `PROVINCIA`
**City:** `CIUDAD`
**Organization:** `Nombre empresa`
**Organizational Unit:** `My department name`
**Certificate Attributes**
**Certificate Type:** `Server Certificate`
**Alternative Names:** `FQDN or Hostname`
## 3 - Agregar servidor de autenticación `Active directory`
---
+ **System/User Manager/Authentication Servers/Add**
***Server Settings***
**Descriptive name:** `ActiveDirectory_Auth`
***LDAP Server Settings***
**Hostname or IP address:** `192.168.26.3`
**Port value:** `389`
**Transport:** `Standar TCP`
**Peer Certificate Authority:** `Openvpn_CA`
**Protocol version:** `3`
**Server Timeout** `default`
**Search scope:** `Entire Subtree`
**Base DN:** `DC=metunas,DC=co,DC=cu`
**Authentication containers:** `CN=Users,DC=metunas,DC=co,DC=cu` `(Especificar todos los contenedores de usuarios)`
***Example: CN=Users;DC=example,DC=com or OU=Staff;OU=Freelancers***
***Extended query***
**Enable extended query:** `[] Enable extended query`
**Bind anonymous:** `[] Use anonymous binds to resolve distinguished names`
**Bind credentials:** `Usuario con acceso a consultas (se requiere para las consultas a AD)`
**Initial Template:** `Microsoft AD`
**User naming attribute:** `samAccountName`
**Group naming attribute:** `cn`
**Group member attribute:** `memberOf`
## 4 - Diagnóstico de autenticación
---
+ **Diagnostics/Authentication**
**Authentication Server:** ` ActiveDirectory_Auth`
**Username:** `usuario AD`
**Password:** `pass AD`
## 5 - Configurar servidor OpenVPN ##
---
+ **VPN/OpenVPN/Servers/Add**
***General Information:***
**Description:** `Empresa_VPN (Nombre descriptivo)`
**Server mode:** `Remote Access ( User Auth )`
**Backend for authentication:** `ActiveDirectory_Auth`
**Device mode:** `tun - Layer 3 Tunnel Mode`
***Endpoint Configuration***
**Protocol:**`TCP on IPv4 only`
**Interface:**`WAN`
**Local port:**`11940`
***Cryptographic Settings***
**TLS Configuration:** `[x] Use a TLS Key` `[x] Automatically generate a TLS Key`
**Peer Certificate Authority:**`Openvpn_CA`
**Peer Certificate Revocation list:**
**Server certificate:**`Openvpn_Cert (Server: Yes, CA: Openvpn_CA)`
**DH Parameter Length:**`2048 bit`
**ECDH Curve:**`Use default`
**Data Encryption Algorithms:**`AES-128-CBC AES-256-CBC AES-256-GCM`
**Fallback Data Encryption Algorithm:**`defaul`
**Auth digest algorithm:**`default`
**Hardware Crypto:**`default`
**Certificate Depth:**`default`
**Client Certificate Key:**`[x] Usage Validation Enforce key usage`
***Tunnel Settings***
**IPv4 Tunnel Network:** `192.168.100.0/24`
**IPv6 Tunnel Network**``
**Redirect IPv4 Gateway:**`[x] Force all client-generated IPv4 traffic through the tunnel.`
**Concurrent connections:**`100`
**Allow Compression:**`Refuse any non-stub compression (Most secure)`
***Client Settings***
**Dynamic IP:** `[x] Allow connected clients to retain their connections if their IP address changes.`
***Advanced Client Settings***
**DNS Default Domain:**`[x] Provide a default domain name to clients`
**DNS Default Domain:**`metunas.co.cu`
**DNS Server enable**`[x] Provide a DNS server list to clients. Addresses may be IPv4 or IPv6.`
**DNS Server 1:** `192.168.26.3`
**DNS Server 2:**`192.168.26.1`
**NTP Server enable:**`[x] Provide an NTP server list to clients`
**NTP Server 1:**`192.168.26.3`
**NetBIOS enable:**`[x] Enable NetBIOS over TCP/IP`
**Username as Common Name:**`[x] Use the authenticated client username instead of the certificate common name (CN).`
 **Gateway creation** `[x] IPv4 only`
## 6 - Crear Reglas de firewall
---
+ **Firewall/Rules/WAN/Add**
**Action:**`Pass`
**Interface:**`WAN`
**Address Family:**`IPv4`
**Protocol:**`UDP`
**Source:**`Any`
**Destination:**`Any`
**Destination Port Range :**`11940`
**Description:** `Allow to Server OpenVPN from WAN`
+ **Firewall/Rules/OpenVPN**
**Action:**`Pass`
**Interface:**`WAN`
**Address Family:**`IPv4`
**Protocol:**`UDP`
**Source:**`Any`
**Destination:**`Any`
**Destination Port Range :**`11940`
**Description:** `Allow to Server OpenVPN from WAN`
## 7 - Instalar paquete openvpn-client-export ##
---
+ ***System/Package Manager/Package Installer***
**openvpn-client-export:** `Install`
+ ***OpenVPN/Client Export Utility***
**Todo por defecto**
**OpenVPN Clients:** `MostClient` (guardar archivo)
# 8 - Clientes
+ ***Descargar openvpn connect client software:*** [https://openvpn.net/client/](https://openvpn.net/client/)
+ **Instalar e importar el archivo antes guardado**
+ **Establecer un nombre al perfil**
+ **escribir usuario de AD**
+ **Guardar contraseña**
+ **Conectar**


