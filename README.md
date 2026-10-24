# Laboratorio 3 — Criptografía y Seguridad en Redes

Auditoría de la implementación de **hashing de contraseñas del lado del cliente**
y ataque **Pass the Hash (PtH)** sobre el sitio **toonpool.com**. Se verifica que
el registro envía la contraseña en texto plano, que el inicio de sesión la hashea
con **SHA-1** en el navegador, y que el servidor valida la identidad únicamente a
partir de ese hash, por lo que basta con presentarlo para iniciar sesión.

- **Autor:** Jonathan Flores
- **Correo:** jonathan.flores@mail.udp.cl
- **Sección:** 4
- **Sitio auditado:** [toonpool.com](https://toonpool.com)

Este repositorio contiene las imágenes y capturas utilizadas en el informe del
laboratorio.

## Estructura del repositorio

```
lab3/
└── images/   # Las 9 capturas usadas en el informe
```

## Herramientas utilizadas

| Herramienta | Uso |
|---|---|
| Burp Suite | Interceptación del registro y del login; reemplazo del hash (Inspector) |
| Consola del navegador (DevTools) | Inspección del formulario y generación del hash con `hex_sha1()` |
| hashes.com (Hash Identifier) | Identificación del algoritmo a partir del hash |

## Relación de capturas con las secciones del informe

| Sección | Captura | Figura | Descripción |
|---|---|---|---|
| 2.1 Algoritmo hash en el **registro** | `images/2-1-registro-burp.png` | Figura 1 | Petición de registro interceptada: `pass1` en texto plano y `pass1Hash` vacío. |
| 2.1 Algoritmo hash en el **registro** | `images/2-1-checkform.png` | Figura 2 | Función `checkForm()` con `return true;` en la primera línea, que anula el hasheo. |
| 2.2 Algoritmo hash en el **login** | `images/2-2-login-form.png` | Figura 3 | Formulario de login: el `onsubmit` calcula `hex_sha1` de la contraseña y la guarda en `passHash`. |
| 2.3 Generar el hash desde la **consola** | `images/2-3-consola.png` | Figura 4 | Generación del hash desde la consola: `hex_sha1("…")` devuelve el hash SHA-1. |
| 2.3 Generar el hash desde la **consola** | `images/2-3-hash-identifier.png` | Figura 5 | Identificador de hashes de hashes.com: el valor se reconoce como SHA-1. |
| 2.4 Interceptar el **login** con Burp | `images/2-4-login-burp.png` | Figura 6 | Petición de login interceptada: `passHash` contiene el hash SHA-1 y `pass` va vacío. |
| 2.5 Intento de login **por el hash** | `images/2-5-hash-malo.png` | Figura 7 | Petición con la contraseña incorrecta: `passHash` contiene el hash de la clave mala. |
| 2.5 Intento de login **por el hash** | `images/2-5-hash-reemplazado.png` | Figura 8 | Reemplazo manual en Burp: `passHash` se cambió por el hash de la contraseña correcta. |
| 2.5 Intento de login **por el hash** | `images/2-5-logueado.png` | Figura 9 | Sesión iniciada tras reemplazar el hash por el correcto, sin enviar la contraseña correcta. |
| 2.6 Políticas de privacidad o seguridad | — | — | Sección de análisis (sin captura asociada). |
| 2.7 Conclusiones sobre la seguridad | — | — | Sección de análisis (sin captura asociada). |

## Nota

Trabajo realizado con fines exclusivamente educativos en el marco del ramo
Criptografía y Seguridad en Redes (CIT2113, UDP). Las capturas emplean una cuenta
y una contraseña de prueba desechables; dicha contraseña fue cambiada tras el
laboratorio y no se incluye ningún dato sensible real.
