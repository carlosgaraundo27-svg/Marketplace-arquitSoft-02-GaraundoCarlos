flowchart TB
    %% Actores
    Cliente["Cliente"]
    Seller["Seller"]
    Admin["Administrador"]

    ClienteWeb["Cliente Web<br/>[Navegador · HTML / CSS / JavaScript]"]

    Cliente --> ClienteWeb
    Seller --> ClienteWeb
    Admin --> ClienteWeb

    ClienteWeb -->|HTTPS · JSON<br/>/api/v1/*| MW

    %% Contenedor Backend Monolito
    subgraph Monolito ["«monolito» Marketplace Backend [Node.js 20 LTS - Express]<br/>Una sola aplicación · un solo proceso · un solo despliegue"]
        direction TB

        MW["Middlewares Express (transversales)<br/>cors · express.json() · auth (JWT) · validación de entrada · manejo de errores · logger"]

        %% Capa 1: Presentación
        subgraph CapaPresentacion ["1. CAPA DE PRESENTACIÓN<br/>Recibe peticiones HTTP, autentica, valida la entrada y responde JSON"]
            subgraph ModU ["módulo usuarios<br/>src/modules/usuarios/"]
                U_routes["usuarios.routes.js"] --> U_ctrl["usuarios.controller.js"]
            end
            subgraph ModS ["módulo sellers<br/>src/modules/sellers/"]
                S_routes["sellers.routes.js"] --> S_ctrl["sellers.controller.js"]
            end
            subgraph ModC ["módulo catalogo<br/>src/modules/catalogo/"]
                C_routes["catalogo.routes.js"] --> C_ctrl["catalogo.controller.js"]
            end
            subgraph ModCar ["módulo carrito<br/>src/modules/carrito/"]
                Car_routes["carrito.routes.js"] --> Car_ctrl["carrito.controller.js"]
            end
            subgraph ModP ["módulo pedidos<br/>src/modules/pedidos/"]
                P_routes["pedidos.routes.js"] --> P_ctrl["pedidos.controller.js"]
            end
        end

        %% Capa 2: Lógica de Negocio
        subgraph CapaNegocio ["2. CAPA DE LÓGICA DE NEGOCIO<br/>Reglas de negocio y coordinación entre módulos"]
            U_service["usuarios.service.js<br/>registro, login, roles"]
            S_service["sellers.service.js<br/>alta de tiendas, validación"]
            C_service["catalogo.service.js<br/>productos, categorías, stock"]
            Car_service["carrito.service.js<br/>items, totales"]
            P_service["pedidos.service.js<br/>checkout, estados, pago/envío"]
        end

        %% Capa 3: Capa de Datos
        subgraph CapaDatos ["3. CAPA DE DATOS<br/>Persistencia y consultas a la base de datos"]
            U_repo["usuarios.repository.js"]
            S_repo["sellers.repository.js"]
            C_repo["catalogo.repository.js"]
            Car_repo["carrito.repository.js"]
            P_repo["pedidos.repository.js"]

            SharedDB["Acceso a datos compartido<br/>Sequelize (ORM) · modelos · pool de conexiones (src/shared/db)"]
        end

        %% Flujo de llamadas de arriba a abajo entre capas
        MW --> U_routes
        MW --> S_routes
        MW --> C_routes
        MW --> Car_routes
        MW --> P_routes

        U_ctrl --> U_service
        S_ctrl --> S_service
        C_ctrl --> C_service
        Car_ctrl --> Car_service
        P_ctrl --> P_service

        U_service --> U_repo
        S_service --> S_repo
        C_service --> C_repo
        Car_service --> Car_repo
        P_service --> P_repo

        U_repo --> SharedDB
        S_repo --> SharedDB
        C_repo --> SharedDB
        Car_repo --> SharedDB
        P_repo --> SharedDB

        %% Comunicación entre módulos (solo a través de su service)
        Car_service -.-> C_service
        P_service -.-> Car_service
        P_service -.-> C_service
        P_service -.-> U_service
        S_service -.-> U_service
    end

    %% Base de datos externa
    SharedDB -->|SQL · TCP 5432| DB[("PostgreSQL<br/>marketplace_db")]

    %% Sistemas externos
    Pasarela["«sistema externo»<br/>Pasarela de pagos<br/>(p. ej. Culqi / Niubiz)"]
    Envios["«sistema externo»<br/>Servicio de envíos<br/>(API del courier)"]

    P_service -->|HTTPS / REST| Pasarela
    P_service -->|HTTPS / REST| Envios

    %% Estilos visuales
    classDef pres fill:#EBF3FB,stroke:#4A90E2,stroke-width:1px;
    classDef biz fill:#E8F5E9,stroke:#66BB6A,stroke-width:1px;
    classDef data fill:#FFF9C4,stroke:#FBC02D,stroke-width:1px;
    classDef shared fill:#FFE0B2,stroke:#FB8C00,stroke-width:1px;
    classDef ext fill:#ECEFF1,stroke:#78909C,stroke-width:1px;
    classDef db fill:#EDE7F6,stroke:#5E35B1,stroke-width:1.5px;

    class U_routes,U_ctrl,S_routes,S_ctrl,C_routes,C_ctrl,Car_routes,Car_ctrl,P_routes,P_ctrl pres;
    class U_service,S_service,C_service,Car_service,P_service biz;
    class U_repo,S_repo,C_repo,Car_repo,P_repo data;
    class SharedDB shared;
    class Pasarela,Envios ext;
    class DB db;