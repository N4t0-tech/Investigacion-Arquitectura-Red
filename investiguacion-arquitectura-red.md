### 1. Conceptos base

Completar tabla:

|Concepto|Definición simple|Nivel técnico breve|Ejemplo real|
|---|---|---|---|
|Modelo OSI|Modelo de 7 capas que explica cómo viaja la información por la red. Divide el proceso en pasos ordenados.|Estándar ISO/IEC 7498-1 que separa las funciones de red en capas lógicas independientes.|Cuando envías un correo, el mensaje pasa por cada capa antes de llegar a internet.|
|Subred (Subnet)|División de una red grande en redes más pequeñas. Permite organizar y separar dispositivos.|Rango de IPs definido por una máscara (ej: 192.168.1.0/24). Limita el dominio de broadcast.|Tu red de casa (192.168.1.x) está separada de la red de tu trabajo.|
|DNS|Sistema que traduce nombres de dominio a direcciones IP. Es como la agenda de contactos de internet.|Protocolo distribuido en jerarquía: root → TLD → autoritativo → caché local. Puerto 53 UDP/TCP.|Cuando escribes google.com, DNS lo convierte a una IP para que tu navegador pueda conectarse.|

👉 Reglas:

- Máximo 3 líneas por celda
- Explicación clara para alguien sin experiencia


---

### 2. Modelo OSI (lo justo y necesario)

**¿Qué es el modelo OSI?**
Es un modelo de referencia que describe cómo los sistemas de comunicación se conectan entre sí, dividiendo el proceso en 7 capas bien definidas. Cada capa tiene una responsabilidad específica y solo interactúa con las capas adyacentes.

**¿Para qué sirve?**
Estandariza cómo dispositivos y protocolos deben comunicarse, facilitando la interoperabilidad entre diferentes fabricantes y sistemas. También ayuda a diagnosticar problemas de red por capas.

👉 Completar tabla:

|Capa|Nombre|¿Qué hace? (simple)|
|---|---|---|
|1|Física|Transmite bits (0s y 1s) por el medio físico: cable, fibra óptica o señal WiFi.|
|2|Enlace|Agrupa los bits en tramas y controla el acceso al medio. Usa direcciones MAC.|
|3|Red|Determina la ruta que toman los datos entre redes distintas. Usa direcciones IP.|
|4|Transporte|Asegura la entrega completa y ordenada de los datos. Usa TCP o UDP.|
|5|Sesión|Establece, mantiene y cierra las sesiones de comunicación entre aplicaciones.|
|6|Presentación|Traduce, cifra o comprime los datos para que la aplicación pueda leerlos.|
|7|Aplicación|Es la interfaz con el usuario final. Incluye protocolos como HTTP, FTP, SMTP.|

---
### 3. DNS (lo que usas todos los días)

**¿Qué es DNS?**
DNS (Domain Name System) es el sistema que traduce nombres legibles por humanos (google.com) en direcciones IP numéricas (142.250.80.46) que las máquinas entienden.

**¿Por qué no usamos IP directamente?**
Porque las IPs son difíciles de recordar y pueden cambiar. Un nombre de dominio es estable aunque la IP detrás se actualice.

👉 Explicar paso a paso:

> ¿Qué pasa cuando escribes:
>
> `google.com` en el navegador?

1. El navegador revisa su **caché local**: ¿ya conozco la IP de google.com?
2. Si no, consulta al **DNS resolver** de tu proveedor de internet (ISP).
3. El resolver pregunta al **servidor raíz** cuál es el servidor para `.com`.
4. Luego pregunta al **servidor TLD** de `.com` quién maneja `google.com`.
5. Finalmente consulta el **servidor autoritativo** de Google, que devuelve la IP.
6. Tu navegador recibe la IP y realiza la conexión al servidor.

👉 Flujo esperado:

- Usuario → DNS resolver → Root → TLD → Autoritativo → IP → servidor

---
### 4. Subnets (nivel básico pero clave)

**¿Qué es una subred?**
Una subred es una porción de una red más grande con su propio rango de IPs. Es como dividir un edificio en pisos o departamentos independientes.

**¿Para qué sirve?**
Para organizar, aislar y controlar el tráfico entre grupos de dispositivos. Mejora la seguridad y la eficiencia de la red.

**¿Por qué dividir redes?**
Para reducir el tráfico broadcast, aplicar reglas distintas a cada grupo, mejorar la seguridad y aprovechar mejor las IPs disponibles.

👉 Ejemplo developer:

- **Red local**: tu máquina tiene `192.168.1.5`, el router es `192.168.1.1` — misma subred `/24`.
- **Red en cloud (AWS VPC)**: tu app en `10.0.1.0/24` (subred pública) y tu base de datos en `10.0.2.0/24` (subred privada, sin acceso desde internet).

---
### 5. Conexión directa con desarrollo (CRÍTICO)

**¿Qué pasa cuando tu app...?**

- **No encuentra el servidor**: el hostname o IP están mal configurados, el servidor está caído, o hay un problema de routing.
- **No resuelve el dominio**: DNS falló al traducir el nombre a IP. Puede ser dominio mal escrito, DNS caído, o caché con dato incorrecto.
- **No puede conectarse**: el servidor existe y resuelve, pero algo bloquea la conexión: firewall, puerto cerrado, o la app no está escuchando.

👉 Relacionar con:

- **DNS**: si falla la resolución, la app ni intenta conectarse. Error típico: `getaddrinfo ENOTFOUND hostname`.
- **IP**: si la IP es incorrecta o no alcanzable, la conexión falla a nivel red. Error: `ECONNREFUSED` o `ETIMEDOUT`.
- **Red**: si hay un problema de enrutamiento o firewall entre tu app y el servidor, la conexión nunca llega aunque todo lo demás esté bien.
---

### 6. Problemas reales (debugging)

**¿Qué significa...?**

- **”DNS not found”**: el sistema no pudo traducir el dominio a una IP. Causas: dominio inexistente, DNS mal configurado, o caché corrupta.
- **”Host unreachable”**: la IP existe pero no se puede llegar a ella. Causas: sin conectividad de red, rutas mal configuradas, o firewall bloqueando.
- **”Network error”**: error genérico de red. Puede ser cualquier fallo: sin internet, servidor caído o puerto cerrado.

**¿Qué revisarías primero como developer?**

1. `ping dominio.com` — ¿hay conectividad básica?
2. `nslookup dominio.com` o `dig dominio.com` — ¿resuelve el dominio?
3. `curl -v http://dominio.com:puerto` — ¿el puerto está abierto y responde?
4. Logs del servidor — ¿qué dice la app del lado del servidor?
---
### 7. Caso práctico real

Escenario:

> “Tu aplicación está deployada, pero nadie puede acceder”

- **¿Es problema de DNS?** Verificar con `nslookup tudominio.com`. Si no resuelve o apunta a una IP incorrecta, el problema está en DNS (registros A/CNAME mal configurados o propagación pendiente).
- **¿Es problema de red?** Verificar con `ping IP` y `traceroute IP`. Si no hay respuesta, revisar reglas de firewall, security groups en cloud, y que el servidor esté encendido.
- **¿Es problema de configuración?** Si el servidor responde pero la app falla, verificar con `curl -v https://tudominio.com`. Causas: app no iniciada, escucha en puerto equivocado, o certificado SSL expirado.

---
### 8. Analogía obligatoria

Crear analogía:

|Concepto|Analogía|
|---|---|
|OSI||
|DNS||
|Subnet||

👉 Ejemplo esperado:

- OSI = proceso de envío de paquete paso a paso
- DNS = agenda de contactos
- Subnet = barrios dentro de una ciudad

---
Responder:

- ¿Qué es un VPC en cloud?
- ¿Qué es una IP privada vs pública?
- ¿Qué es un load balancer?