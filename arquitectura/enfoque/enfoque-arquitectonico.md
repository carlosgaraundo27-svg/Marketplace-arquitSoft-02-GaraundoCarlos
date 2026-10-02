
# PASO 5: DEFINIR PATRONES O ENFOQUE ARQUITECTÓNICO

Las dependencias internas se gestionan mediante **Clean Architecture**.

| Elemento | Descripción aplicada al Marketplace |
| :--- | :--- |
| **Patrón / enfoque arquitectónico** | Clean Architecture (Arquitectura Limpia). |
| **Objetivo** | Separar responsabilidades y controlar las dependencias hacia el dominio. |
| **¿Qué problema resuelve?** | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas, como bases de datos, API y servicios de pago. |
| **Capas definidas** | Presentación, Aplicación, Dominio e Infraestructura. |
| **Beneficios** | • Facilita el mantenimiento y las pruebas unitarias.<br>• Permite cambiar implementaciones técnicas sin modificar innecesariamente las reglas del negocio.<br>• Mejora la organización y separación de responsabilidades del código. |

## Diagrama de Clean Architecture (Marketplace Web - Angular 18)

```mermaid
flowchart TB
    Usuario["Usuario<br/>(Cliente)"]

    subgraph AppScope ["«aplicación» Marketplace Web [Angular 18 · TypeScript] src/app/"]
        direction TB

        subgraph Frameworks ["ADAPTADORES Y FRAMEWORKS — dependen de Angular, HttpClient, RxJS"]
            direction TB

            subgraph Presentacion ["PRESENTACIÓN<br/>src/app/presentacion/"]
                direction TB
                CatalogoComp["«componente»<br/><b>CatalogoComponent</b><br/>lista y filtra productos"]
                EstadoCarrito["«servicio de estado»<br/><b>EstadoCarrito</b><br/>signals · sin reglas"]
                CarritoComp["«componente»<br/><b>CarritoComponent</b><br/>resumen y confirmar compra"]
                AppComp["«componente»<br/><b>AppComponent</b><br/>shell de la aplicación"]
            end

            subgraph Aplicacion ["APLICACIÓN — casos de uso · src/app/aplicacion/"]
                direction TB
                CU1["«caso de uso»<br/><b>ConsultarCatalogoCasoUso</b><br/>ejecutar()"]
                CU2["«caso de uso»<br/><b>AgregarAlCarritoCasoUso</b><br/>ejecutar()"]
                CU3["«caso de uso»<br/><b>RegistrarCompraCasoUso</b><br/>ejecutar()"]

                subgraph Dominio ["DOMINIO — núcleo · src/app/dominio/"]
                    direction TB
                    subgraph Modelos ["Modelos (entidades y reglas)"]
                        Prod["«entidad»<br/><b>Producto</b><br/>stock, categoría, precio"]
                        Cart["«entidad»<br/><b>Carrito</b><br/>inmutable · subtotal, total"]
                        Ped["«entidad»<br/><b>Pedido</b><br/>estados · cancelación"]
                        Precios["«reglas»<br/><b>precios.ts</b><br/>comisión 10% · IGV 18%"]
                    end

                    subgraph Contratos ["Contratos (puertos)"]
                        RepoProdPort["«interface»<br/><b>RepositorioProductos</b>"]
                        RepoPedPort["«interface»<br/><b>RepositorioPedidos</b>"]
                        ProcPagoPort["«interface»<br/><b>ProcesadorPagos</b>"]
                        NotifPort["«interface»<br/><b>NotificadorCliente</b>"]
                    end
                end
            end

            subgraph Infra ["INFRAESTRUCTURA<br/>src/app/infraestructura/"]
                direction TB
                RepoProdAdapt["«adaptador»<br/><b>RepositorioProductosMemoria</b><br/><b>RepositorioProductosHttp</b>"]
                RepoPedAdapt["«adaptador»<br/><b>RepositorioPedidosMemoria</b>"]
                ProcPagoAdapt["«adaptador»<br/><b>ProcesadorPagosSimulado</b><br/><b>ProcesadorPagosNiubiz</b>"]
                NotifAdapt["«adaptador»<br/><b>NotificadorConsola</b><br/><b>NotificadorWhatsApp</b>"]
                Tokens["«Angular DI»<br/><b>tokens.ts</b><br/>InjectionToken por contrato"]
            end

            AppConfig["«raíz de composición»<br/><b>app.config.ts</b><br/>único archivo que elige qué adaptador cumple cada contrato (useFactory + InjectionToken) y lo inyecta en los casos de uso"]
        end
    end

    Backend["«sistema externo»<br/><b>Marketplace API REST</b><br/>Backend Node.js · monolito modular<br/><br/>/api/productos<br/>/api/pedidos<br/>/api/authorization<br/>/api/mensajes<br/><br/><i>Se integra con Niubiz y WhatsApp;<br/>las credenciales viven solo aquí.</i>"]

    Usuario -->|navegador| Presentacion
    Presentacion -->|invoca| Aplicacion
    EstadoCarrito -.-> Cart

    CU1 -.-> RepoProdPort
    CU2 -.-> Cart
    CU3 -.-> RepoPedPort
    CU3 -.-> ProcPagoPort
    CU3 -.-> NotifPort

    RepoProdAdapt -.-> RepoProdPort
    RepoPedAdapt -.-> RepoPedPort
    ProcPagoAdapt -.-> ProcPagoPort
    NotifAdapt -.-> NotifPort

    Tokens -.->|registra| AppConfig

    RepoProdAdapt -->|HTTP / JSON| Backend
    ProcPagoAdapt --> Backend
    NotifAdapt --> Backend