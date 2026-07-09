# Bug Bounty - Divulgación de información por validación incorrecta de entrada en `/track/items`

> **Estado:** ✅ Aceptado
>
> **Plataforma:** Intigriti
>
> **Programa:** bpost
>
> **Severidad:** Alta (CVSS 7.5)
>
> **Tipo:** Validación incorrecta de entrada / Divulgación de información

---

# Resumen

Durante una evaluación de seguridad dentro del programa de Bug Bounty de **bpost** en Intigriti, identifiqué una vulnerabilidad de validación incorrecta de entrada en el endpoint encargado del seguimiento de paquetes.

La aplicación procesaba un identificador de seguimiento malformado y, bajo determinadas condiciones, devolvía información del envío en lugar de rechazar la petición.

Aunque inicialmente el comportamiento parecía indicar una posible inyección SQL, el impacto demostrado fue una **divulgación de información** causada por una validación insuficiente de los parámetros de entrada.

---

# Endpoint afectado

```
GET /track/items
```

Ejemplo:

```
https://track-ac2.bpost.cloud/track/items
```

---

# Descripción técnica

El parámetro `itemIdentifier` no validaba correctamente determinadas entradas malformadas.

Al enviar un identificador inválido junto con un código postal válido, el servidor procesaba la solicitud y respondía con información del paquete en lugar de devolver un error de validación.

El comportamiento observado indicaba que la aplicación no verificaba correctamente la relación entre ambos parámetros antes de generar la respuesta.

---

# Impacto

El comportamiento permitía acceder a información relacionada con envíos sin proporcionar un identificador de seguimiento válido.

Entre la información expuesta se encontraba:

- Nombre del destinatario
- Dirección
- Municipio
- Código postal
- Información del remitente
- Información del envío
- Enlace relacionado con las preferencias de entrega

Este comportamiento suponía una exposición no autorizada de información.

---

# Evidencia

Para la demostración de la vulnerabilidad se documentó:

- Captura de la petición legítima.
- Captura de la petición utilizando una entrada malformada.
- Respuesta obtenida mediante Burp Suite.
- Comparación entre ambas respuestas para demostrar el comportamiento inesperado.

Por motivos de divulgación responsable, las capturas con información sensible no se incluyen en este repositorio.

---

# Análisis

Inicialmente el comportamiento sugería una posible vulnerabilidad de inyección.

Tras un análisis más detallado se concluyó que el problema principal era la validación insuficiente del parámetro de entrada, lo que ocasionaba la divulgación de información cuando la aplicación procesaba solicitudes malformadas.

---

# Recomendaciones

Se recomendó al equipo de desarrollo:

- Validar estrictamente el formato del parámetro `itemIdentifier`.
- Rechazar caracteres inválidos antes de procesar la solicitud.
- Validar conjuntamente el identificador de seguimiento y el código postal.
- Utilizar consultas parametrizadas cuando exista interacción con bases de datos.
- Devolver respuestas genéricas cuando la validación falle.

---

# Cronología

| Fecha | Evento |
|--------|--------|
| 28/05/2026 | Reporte enviado a Intigriti |
| 15/06/2026 | Superó la fase inicial de triaje |
| 08/07/2026 | Estado cambiado a Pending |
| 09/07/2026 | Reporte aceptado por el programa |

---

# Resultado

- ✅ Reporte aceptado.
- ✅ Clasificado con severidad **Alta (7.5)**.
- ✅ Validado por el equipo de seguridad de **bpost**.

---

# Lecciones aprendidas

Este hallazgo reforzó la importancia de:

- No asumir el tipo de vulnerabilidad únicamente por el comportamiento observado.
- Centrar el reporte en el impacto real demostrado.
- Documentar adecuadamente las evidencias obtenidas.
- Mantener una comunicación clara durante todo el proceso de divulgación responsable.

---

# Divulgación responsable

Este documento omite deliberadamente:

- Payloads completos.
- Datos personales.
- Tokens.
- Respuestas originales del servidor.
- Cualquier información que permita reproducir el problema.

El objetivo es compartir la metodología empleada sin comprometer la seguridad del activo evaluado.