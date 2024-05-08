# Pfsense 2.7.2 + OpenVPN + Active Directory Auth
## 1 - Crear entidad certificadora
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
---



