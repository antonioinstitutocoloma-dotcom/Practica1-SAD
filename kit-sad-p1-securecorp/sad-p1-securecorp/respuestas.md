# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos:** Antonio Suarez Perez
**Usuario:** asuarez

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: Antonio2026
- mtorres: Marta2026

---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?

El issuer es la propia CA porque es un certificado autofirmado, y su validez es la fecha en que se programó al crearlo. Son iguales en la CA porque se firma a sí misma al ser la autoridad máxima, pero en ldap.crt son distintos porque el certificado del servidor lo emite y firma esa CA.

**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:

```
a) miembros de rrhh:

ldapsearch -x -H ldap://ldap.securecorp.local -b "ou=groups,dc=securecorp,dc=local" "(cn=rrhh)"

b) cn y mail de todas las personas:

ldapsearch -x -H ldap://ldap.securecorp.local -b "ou=people,dc=securecorp,dc=local" "objectClass=inetOrgPerson" cn mail

```

**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?

Para que solo el usuario openldap, que es el que ejecuta el servidor LDAP, pueda leer la clave privada, evitando que otros usuarios del sistema puedan robarla o comprometer la seguridad.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?

He puesto ldap:/// ldaps:/// ldapi:/// para que el servidor acepte peticiones normales sin cifrar, conexiones seguras por LDAPS y conexiones locales al mismo tiempo.

**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?

Porque el cliente no conocía ni confiaba en la CA raíz, así que rechazaba la conexión segura al no poder verificar el certificado del servidor LDAP.

**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

```

```

El primer ticket es el TGT para autenticarnos en Kerberos y el segundo es el TGS para acceder al servicio concreto. No, la contraseña nunca viaja por la red; se usa criptografía basada en tiempos y claves simétricas para demostrar que la sabes sin transmitirla.
kinit asuarez
Password for asuarez@SECURECORP.LOCAL: 
root@CLIENTE:~# klist -A
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: asuarez@SECURECORP.LOCAL

Valid starting     Expires            Service principal
10/10/26 16:31:51  10/11/26 02:31:51  krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL
	renew until 10/17/26 16:31:51

**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?

build crea la imagen localmente usando un Dockerfile del proyecto, mientras que image descarga una imagen ya hecha de internet. La línea - "8081:80" redirige el puerto 8081 de nuestra máquina al puerto 80 del contenedor para poder entrar por la web.

**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?

Porque en la máquina web ya lo dejamos automatizado dentro de su propio Dockerfile al compilarlo. Si ejecutamos ./lab.sh reset, se borrarían los cambios manuales hechos en el cliente y dejaría de funcionar hasta que volviésemos a configurarlo.
