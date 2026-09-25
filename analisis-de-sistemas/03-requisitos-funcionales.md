# Requisitos Funcionales del Sistema

## 1. Lista de Requisitos Funcionales

| ID | Requisito Funcional |
| :--- | :--- |
| **RF01** | El sistema debe permitir buscar productos mediante criterios de búsqueda (palabras clave, categoría, especie). |
| **RF02** | El sistema debe permitir consultar la información, precios y disponibilidad de los productos. |
| **RF03** | El sistema debe permitir registrar, modificar y deshabilitar productos en la plataforma por cada seller. |
| **RF04** | El sistema debe permitir agregar, modificar cantidades y eliminar productos del carrito de compra. |
| **RF05** | El sistema debe permitir generar un pedido a partir de los productos del carrito e iniciar el flujo de pago. |
| **RF06** | El sistema debe permitir consultar los pedidos realizados y su estado de entrega en tiempo real. |
| **RF07** | El sistema debe permitir registrar, validar, actualizar y desactivar sellers de la plataforma. |
| **RF08** | El sistema debe permitir consultar el detalle desglosado de un pedido realizado. |

## 2. Relación entre Historias de Usuario y Requisitos Funcionales

| Historia de Usuario | Descripción | Requisitos Funcionales Relacionados |
| :--- | :--- | :--- |
| **HU01** | Buscar y consultar productos | RF01, RF02 |
| **HU02** | Gestionar productos | RF02, RF03 |
| **HU03** | Gestionar carrito | RF04 |
| **HU04** | Realizar pedido | RF05, RF08 |
| **HU05** | Gestionar sellers | RF07 |
| **HU06** | Consultar pedidos | RF06, RF08 |
| **HU07** | Consultar órdenes de seller | RF06, RF08 |