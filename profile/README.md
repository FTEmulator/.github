# FTEmulator

**Aplicación web para compra/venta de acciones de forma ficticia, sin riesgo de perder patrimonio real.**

---

## Objetivo

Brindar a estudiantes o personas interesadas en la inversión bursátil una plataforma donde puedan simular operaciones en el mercado de manera realista, sin exponerse a riesgos financieros.

---

## Características principales

- Simulación de compra y venta de acciones en bolsa.
- Gestión de cartera (wallet) con historial y balance.
- Perfil de usuario personalizable.

---

## Tipos de usuarios

Actualmente solo se contempla un tipo de usuario:

- **Cliente:** Usuario que accede al sistema para simular inversiones.

> *Nota: No se implementará un rol de tutor para mantener la simplicidad y reducir la complejidad del sistema.*

---

## Flujo principal de usuario

1. El cliente inicia sesión.
2. Consulta el mercado y visualiza precios de acciones.
3. Realiza compras o ventas de acciones.
4. Revisa el estado de su cartera.

---

## Requisitos funcionales

- Registro e inicio de sesión (incluyendo autenticación con Google).
- Visualización de precios actualizados de acciones.
- Simulación realista de compra y venta de acciones.
- Envío de correos electrónicos para:
  - Recuperación de contraseña.
  - Verificación de cuenta.

---

## Requisitos no funcionales

- Escalabilidad del sistema para soportar múltiples usuarios simultáneamente.
- Seguridad a nivel de comunicaciones y autenticación:
  - HTTPS
  - JWT (JSON Web Tokens)
  - OAuth2

---

## Restricciones técnicas

- Los precios de las acciones se mostrarán con un **retraso de 15 minutos**, debido a restricciones legales sobre el uso de datos bursátiles en tiempo real.
