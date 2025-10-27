# C4 Models Documentation - Commission Tracker

## Overview

Commission Tracker es una plataforma SaaS profesional para el procesamiento de documentos financieros y seguimiento de comisiones con capacidades de extracción de datos impulsadas por IA.

## Level 1: System Context

```mermaid
graph TB
    subgraph "External Users"
        U1[Insurance Agents]
        U2[Brokers]
        U3[Administrators]
    end
    
    subgraph "Commission Tracker System"
        SYS[Commission Tracker Platform]
    end
    
    subgraph "External Systems"
        E1[Email Service<br/>SMTP]
        E2[AI Services<br/>Claude, Mistral, GPT-4]
        E3[Cloud Storage<br/>Google Cloud Storage]
        E4[Database<br/>PostgreSQL]
        E5[Redis Cache]
    end
    
    U1 -->|Upload PDFs<br/>View Reports| SYS
    U2 -->|Process Documents<br/>Track Commissions| SYS
    U3 -->|Manage Users<br/>Configure System| SYS
    
    SYS -->|Send OTP<br/>Notifications| E1
    SYS -->|Extract Data<br/>Process Documents| E2
    SYS -->|Store Files<br/>Retrieve Documents| E3
    SYS -->|Persist Data<br/>Query Information| E4
    SYS -->|Cache Sessions<br/>Store OTP| E5
```

### System Context Description

**Primary Users:**
- **Insurance Agents**: Suben documentos de comisiones, revisan datos extraídos, configuran mapeos de campos
- **Brokers**: Procesan múltiples documentos, generan reportes de comisiones, gestionan clientes
- **Administrators**: Gestionan usuarios, configuran dominios permitidos, supervisan el sistema

**External Dependencies:**
- **Email Service (SMTP)**: Envío de códigos OTP y notificaciones
- **AI Services**: Claude Document AI (extracción principal), Mistral (fallback), GPT-4 (operaciones de IA)
- **Cloud Storage (GCS)**: Almacenamiento de archivos PDF y documentos procesados
- **Database (PostgreSQL)**: Persistencia de datos de usuarios, empresas, extracciones y comisiones
- **Redis Cache**: Gestión de sesiones y códigos OTP temporales

## Level 2: Container Diagram

```mermaid
graph TB
    subgraph "Users"
        U1[Insurance Agents]
        U2[Brokers] 
        U3[Administrators]
    end
    
    subgraph "Commission Tracker System"
        subgraph "Frontend Container"
            WEB[Next.js Web App<br/>React 19 + TypeScript]
        end
        
        subgraph "Backend Container"
            API[FastAPI Backend<br/>Python 3.11+]
        end
        
        subgraph "Database Container"
            DB[(PostgreSQL Database<br/>Render/Supabase)]
        end
        
        subgraph "Cache Container"
            CACHE[(Redis Cache<br/>Session Management)]
        end
        
        subgraph "Storage Container"
            STORAGE[Google Cloud Storage<br/>File Storage]
        end
    end
    
    subgraph "External AI Services"
        AI1[Claude Document AI<br/>Primary Extraction]
        AI2[Mistral Document AI<br/>Fallback Extraction]
        AI3[GPT-4 Vision<br/>AI Operations]
    end
    
    subgraph "External Services"
        EMAIL[SMTP Email Service<br/>OTP Delivery]
    end
    
    U1 --> WEB
    U2 --> WEB
    U3 --> WEB
    
    WEB <-->|REST API<br/>WebSocket| API
    API <-->|SQL Queries| DB
    API <-->|Cache Operations| CACHE
    API <-->|File Operations| STORAGE
    API <-->|AI Processing| AI1
    API <-->|AI Processing| AI2
    API <-->|AI Processing| AI3
    API <-->|Send OTP| EMAIL
```

### Container Descriptions

**Frontend Container (Next.js Web App)**
- **Technology**: Next.js 15, React 19, TypeScript, Tailwind CSS
- **Responsibilities**: 
  - Interfaz de usuario para carga de documentos
  - Visualización de datos extraídos
  - Gestión de mapeos de campos
  - Reportes y dashboards
  - Autenticación OTP
- **Key Components**: Dashboard, Carrier Management, Upload Zone, Table Editor

**Backend Container (FastAPI Backend)**
- **Technology**: FastAPI, Python 3.11+, SQLAlchemy, Pydantic
- **Responsibilities**:
  - API REST para operaciones CRUD
  - Procesamiento de documentos con IA
  - Autenticación y autorización
  - WebSocket para actualizaciones en tiempo real
  - Integración con servicios externos
- **Key Modules**: Auth, Extraction, Dashboard, Admin, WebSocket

**Database Container (PostgreSQL)**
- **Technology**: PostgreSQL con asyncpg
- **Responsibilities**:
  - Almacenamiento de datos de usuarios y empresas
  - Metadatos de extracciones
  - Configuraciones de mapeo de campos
  - Datos de comisiones procesadas
- **Key Tables**: users, companies, statement_uploads, extractions, earned_commissions

## Level 3: Component Diagram - Backend API

```mermaid
graph TB
    subgraph "FastAPI Backend Container"
        subgraph "API Layer"
            AUTH[Auth API<br/>OTP Authentication]
            EXTRACT[Extraction API<br/>Document Processing]
            DASH[Dashboard API<br/>Statistics & Reports]
            ADMIN[Admin API<br/>User Management]
            WS[WebSocket API<br/>Real-time Updates]
        end
        
        subgraph "Service Layer"
            AUTH_SVC[Auth Service<br/>JWT & OTP]
            EXTRACT_SVC[Extraction Service<br/>AI Processing]
            DASH_SVC[Dashboard Service<br/>Analytics]
            ADMIN_SVC[Admin Service<br/>User Management]
            WS_SVC[WebSocket Service<br/>Progress Tracking]
        end
        
        subgraph "AI Services"
            CLAUDE[Claude Service<br/>Primary Extraction]
            MISTRAL[Mistral Service<br/>Fallback Extraction]
            GPT4[GPT-4 Service<br/>AI Operations]
        end
        
        subgraph "Data Layer"
            CRUD[CRUD Operations<br/>Database Access]
            MODELS[Data Models<br/>SQLAlchemy]
        end
        
        subgraph "External Integrations"
            GCS[GCS Utils<br/>File Storage]
            EMAIL[Email Service<br/>SMTP]
            REDIS[Redis Client<br/>Caching]
        end
    end
    
    AUTH --> AUTH_SVC
    EXTRACT --> EXTRACT_SVC
    DASH --> DASH_SVC
    ADMIN --> ADMIN_SVC
    WS --> WS_SVC
    
    AUTH_SVC --> CRUD
    EXTRACT_SVC --> CLAUDE
    EXTRACT_SVC --> MISTRAL
    EXTRACT_SVC --> GPT4
    EXTRACT_SVC --> GCS
    DASH_SVC --> CRUD
    ADMIN_SVC --> CRUD
    WS_SVC --> REDIS
    
    CRUD --> MODELS
    AUTH_SVC --> EMAIL
    AUTH_SVC --> REDIS
```

### Backend Component Descriptions

**API Layer:**
- **Auth API**: Manejo de autenticación OTP, login/logout, gestión de sesiones
- **Extraction API**: Endpoints para procesamiento de documentos, extracción de tablas, mapeo de campos
- **Dashboard API**: Estadísticas, reportes de comisiones, datos analíticos
- **Admin API**: Gestión de usuarios, dominios permitidos, configuraciones del sistema
- **WebSocket API**: Comunicación en tiempo real para progreso de extracciones

**Service Layer:**
- **Auth Service**: Lógica de autenticación, generación de JWT, validación de OTP
- **Extraction Service**: Orquestación del procesamiento de documentos con múltiples servicios de IA
- **Dashboard Service**: Cálculos de estadísticas, agregaciones de datos de comisiones
- **Admin Service**: Gestión de usuarios, permisos, configuraciones
- **WebSocket Service**: Gestión de conexiones WebSocket, notificaciones de progreso

**AI Services:**
- **Claude Service**: Extracción principal usando Claude Document AI (superior precisión)
- **Mistral Service**: Extracción de respaldo usando Mistral Document AI
- **GPT-4 Service**: Operaciones de IA para mapeo de campos, detección de tipos de plan

## Level 3: Component Diagram - Frontend

```mermaid
graph TB
    subgraph "Next.js Frontend Container"
        subgraph "Pages"
            LANDING[Landing Page<br/>Authentication]
            DASHBOARD[Dashboard Page<br/>Main Interface]
            UPLOAD[Upload Page<br/>Document Processing]
            ADMIN[Admin Page<br/>User Management]
        end
        
        subgraph "Components"
            AUTH_COMP[Auth Components<br/>OTP Login/Register]
            DASH_COMP[Dashboard Components<br/>Stats & Reports]
            UPLOAD_COMP[Upload Components<br/>File Processing]
            CARRIER_COMP[Carrier Components<br/>Data Management]
            TABLE_COMP[Table Components<br/>Data Editing]
        end
        
        subgraph "Context & State"
            AUTH_CTX[Auth Context<br/>User State]
            SUB_CTX[Submission Context<br/>Upload State]
            THEME_CTX[Theme Context<br/>UI State]
        end
        
        subgraph "Services & Hooks"
            AUTH_SVC[Auth Service<br/>API Communication]
            WS_HOOK[WebSocket Hook<br/>Real-time Updates]
            DASH_HOOK[Dashboard Hook<br/>Data Fetching]
        end
        
        subgraph "Utils & Libraries"
            UTILS[Utils<br/>Formatters & Helpers]
            VALIDATION[Validation<br/>Form Validation]
            ANALYTICS[Analytics<br/>Usage Tracking]
        end
    end
    
    LANDING --> AUTH_COMP
    DASHBOARD --> DASH_COMP
    UPLOAD --> UPLOAD_COMP
    ADMIN --> CARRIER_COMP
    
    AUTH_COMP --> AUTH_CTX
    DASH_COMP --> SUB_CTX
    UPLOAD_COMP --> SUB_CTX
    CARRIER_COMP --> AUTH_CTX
    
    AUTH_CTX --> AUTH_SVC
    SUB_CTX --> WS_HOOK
    DASH_COMP --> DASH_HOOK
    
    AUTH_SVC --> UTILS
    WS_HOOK --> VALIDATION
    DASH_HOOK --> ANALYTICS
```

### Frontend Component Descriptions

**Pages:**
- **Landing Page**: Página de inicio con autenticación OTP
- **Dashboard Page**: Interfaz principal con estadísticas y navegación
- **Upload Page**: Procesamiento de documentos con progreso en tiempo real
- **Admin Page**: Gestión de usuarios y configuraciones (solo administradores)

**Components:**
- **Auth Components**: Formularios de login/registro con OTP
- **Dashboard Components**: Tarjetas de estadísticas, gráficos, modales
- **Upload Components**: Zona de carga, editor de tablas, mapeo de campos
- **Carrier Components**: Gestión de transportistas, visualización de datos
- **Table Components**: Editor de tablas, validación de datos

**Context & State:**
- **Auth Context**: Estado del usuario, permisos, autenticación
- **Submission Context**: Estado de cargas, progreso de extracciones
- **Theme Context**: Tema de la interfaz, preferencias de usuario

## Level 4: Code Diagram - Authentication Flow

```mermaid
sequenceDiagram
    participant U as User
    participant F as Frontend
    participant A as Auth API
    participant E as Email Service
    participant D as Database
    participant R as Redis
    
    U->>F: Enter email
    F->>A: POST /api/auth/otp/request
    A->>D: Check user exists
    A->>R: Store OTP (10 min TTL)
    A->>E: Send OTP email
    E-->>U: OTP code via email
    A-->>F: OTP sent confirmation
    
    U->>F: Enter OTP code
    F->>A: POST /api/auth/otp/verify
    A->>R: Validate OTP
    A->>D: Update user session
    A->>A: Generate JWT tokens
    A-->>F: Set httpOnly cookies
    F->>F: Update auth state
    F-->>U: Redirect to dashboard
```

## Level 4: Code Diagram - Document Processing Flow

```mermaid
sequenceDiagram
    participant U as User
    participant F as Frontend
    participant A as Extraction API
    participant W as WebSocket
    participant C as Claude AI
    participant G as GCS Storage
    participant D as Database
    
    U->>F: Upload PDF file
    F->>A: POST /api/extract-tables-smart/
    A->>G: Upload file to GCS
    A->>D: Create upload record
    A->>W: Send progress update
    
    A->>C: Extract metadata
    C-->>A: Document metadata
    A->>W: Update progress (20%)
    
    A->>C: Extract table data
    C-->>A: Structured table data
    A->>W: Update progress (60%)
    
    A->>A: Process and validate data
    A->>D: Save extraction results
    A->>W: Send completion (100%)
    
    W-->>F: Real-time progress updates
    F-->>U: Show extraction results
```

## Technology Stack

### Frontend Technologies
- **Framework**: Next.js 15 (React 19)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **State Management**: React Context API
- **HTTP Client**: Axios
- **Real-time**: WebSocket
- **PDF Processing**: PDF.js, react-pdf-viewer
- **Charts**: Chart.js, Recharts
- **UI Components**: Radix UI, Lucide React

### Backend Technologies
- **Framework**: FastAPI
- **Language**: Python 3.11+
- **Database ORM**: SQLAlchemy (async)
- **Database**: PostgreSQL (Render/Supabase)
- **Cache**: Redis
- **Authentication**: JWT + OTP
- **File Storage**: Google Cloud Storage
- **AI Services**: Claude Document AI, Mistral, GPT-4
- **PDF Processing**: PyMuPDF, pdfplumber, Docling
- **Email**: SMTP (aiosmtplib)

### Infrastructure
- **Frontend Hosting**: Vercel
- **Backend Hosting**: Render
- **Database**: PostgreSQL (Render/Supabase)
- **File Storage**: Google Cloud Storage
- **Cache**: Redis
- **Containerization**: Docker

## Key Features

### Document Processing
- **Multi-format Support**: PDF, Excel (XLSX, XLS, XLSM, XLSB)
- **AI-Powered Extraction**: Claude Document AI (primary), Mistral (fallback)
- **Real-time Progress**: WebSocket updates during processing
- **Intelligent Mapping**: Automatic field mapping with AI assistance
- **Quality Validation**: Confidence scoring and data validation

### User Management
- **OTP Authentication**: Secure email-based authentication
- **Role-based Access**: Admin, User, Read-only roles
- **Domain Whitelisting**: Company-specific access control
- **Session Management**: JWT tokens with automatic refresh

### Data Management
- **Commission Tracking**: Monthly breakdowns and analytics
- **Carrier Management**: Multiple insurance carrier support
- **Field Mapping**: Customizable field configurations per company
- **Duplicate Detection**: SHA-256 hash-based duplicate prevention

### Analytics & Reporting
- **Dashboard Analytics**: Real-time statistics and KPIs
- **Commission Reports**: Monthly and yearly commission tracking
- **Data Visualization**: Interactive charts and graphs
- **Export Capabilities**: Data export in multiple formats

## Security Features

- **HTTP-Only Cookies**: Secure token storage
- **CORS Configuration**: Cross-origin request security
- **Rate Limiting**: API endpoint protection
- **Input Validation**: Pydantic model validation
- **SQL Injection Prevention**: SQLAlchemy ORM protection
- **File Type Validation**: Secure file upload handling
- **Session Timeout**: Automatic session expiration

## Performance Optimizations

- **Async Processing**: Non-blocking I/O operations
- **Connection Pooling**: Database connection optimization
- **Caching**: Redis for session and OTP storage
- **File Streaming**: Efficient large file handling
- **Progress Tracking**: Real-time user feedback
- **Timeout Management**: Configurable timeouts for long operations

---

*Esta documentación C4 models proporciona una vista completa de la arquitectura del sistema Commission Tracker, desde el contexto del sistema hasta los detalles de implementación específicos.*
