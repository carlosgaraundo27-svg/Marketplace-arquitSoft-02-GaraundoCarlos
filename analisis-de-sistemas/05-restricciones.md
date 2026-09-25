# Restricciones del Sistema

| ID | Restricción | Descripción |
| :--- | :--- | :--- |
| **RC01** | **Aplicación web** | El sistema debe desarrollarse como una solución web accesible desde navegadores estándar sin requerir instalaciones locales. |
| **RC02** | **Control de versiones** | El código fuente y la documentación deben gestionarse obligatoriamente mediante Git y GitHub. |
| **RC03** | **API REST** | La comunicación entre la capa de presentación y la lógica de negocio debe ejecutarse mediante servicios web RESTful. |
| **RC04** | **Pasarela de pago** | El procesamiento monetario debe delegarse a una pasarela de pago externa que cumpla con los estándares de seguridad vigentes. |
| **RC05** | **Servicio de envío** | La plataforma debe integrarse con servicios logísticos externos para calcular fletes y rastrear el despacho de pedidos. |
| **RC06** | **Base de datos relacional** | Los datos deben persistirse en un motor relacional como PostgreSQL para garantizar la atomicidad de las compras e inventarios. |