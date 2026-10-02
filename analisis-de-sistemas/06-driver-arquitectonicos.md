# Drivers Arquitectónicos

| ID | Driver Arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- |
| **DA01** | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales. | AC03 – Escalabilidad | Influye directamente en la necesidad de diseñar componentes desacoplados que puedan escalar horizontalmente. |
| **DA02** | El sistema debe mantener tiempos de respuesta adecuados durante una alta concurrencia. | AC01 – Rendimiento | Condiciona la optimización de consultas, separación de lectura/escritura y uso de caché en capas intermedias. |
| **DA03** | El sistema debe proteger los datos de usuarios y operaciones de compra. | AC04 – Seguridad | Define mecanismos de autenticación, control de accesos por roles y cifrado en tránsito y reposo. |
| **DA04** | El sistema debe integrarse con una pasarela de pago externa mediante una API. | RC04 – Pasarela de pago | Exige desacoplar el procesamiento transaccional mediante adaptadores o webhooks para tolerar respuestas asíncronas. |
| **DA05** | El sistema debe utilizar una API REST para la comunicación entre frontend y backend. | RC03 – API REST | Establece una arquitectura cliente-servidor desacoplada entre la interfaz de usuario y la lógica de negocio. |
| **DA06** | El sistema debe permitir modificar funionalidades sin afectar innecesariamente otros módulos. | AC05 - Mantenibilidad | Influye en la separación de responsabilidades, modularidad y dependencias internas. |