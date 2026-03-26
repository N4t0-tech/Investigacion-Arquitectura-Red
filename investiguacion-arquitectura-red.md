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

Responder:

- ¿Qué es DNS?
- ¿Por qué no usamos IP directamente?

👉 Explicar paso a paso:

> ¿Qué pasa cuando escribes:
> 
> `google.com` en el navegador?

👉 Flujo esperado:

- Usuario → DNS → IP → servidor

---
### 4. Subnets (nivel básico pero clave)

Responder:

- ¿Qué es una subred?
- ¿Para qué sirve?
- ¿Por qué dividir redes?

👉 Ejemplo developer:

- Red local vs red en cloud

---
### 5. Conexión directa con desarrollo (CRÍTICO)

Responder:

- ¿Qué pasa cuando tu app:
    - No encuentra el servidor?
    - No resuelve el dominio?
    - No puede conectarse?

👉 Relacionar con:

- DNS
- IP
- Red
---

### 6. Problemas reales (debugging)

Responder:

- ¿Qué significa:
    - “DNS not found”?
    - “Host unreachable”?
    - “Network error”?
- ¿Qué revisarías primero como developer?
---
### 7. Caso práctico real

Escenario:

> “Tu aplicación está deployada, pero nadie puede acceder”

Responder:

- ¿Es problema de DNS?
- ¿Es problema de red?
- ¿Es problema de configuración?

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