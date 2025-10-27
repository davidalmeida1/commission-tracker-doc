# C4 Models Documentation - Commission Tracker

## Overview

Commission Tracker is a professional SaaS platform for financial document processing and commission tracking with AI-powered data extraction capabilities.

## What is the C4 Model?

The **C4 Model** (Context, Containers, Components, Code) is a software architecture documentation methodology developed by Simon Brown. It provides a structured way to visualize and communicate software system architecture to different audiences, from high-level stakeholders to developers.

### The 4 Levels of the C4 Model:

1. **System Context (Level 1)**: Shows how the system fits into the real world, identifying users and external systems
2. **Containers (Level 2)**: Breaks down the system into high-level containers (applications, databases, etc.)
3. **Components (Level 3)**: Breaks down each container into its main components
4. **Code (Level 4)**: Shows implementation details of specific components

### In Our Commission Tracker System

This documentation uses the C4 Model to represent the complete architecture of **Commission Tracker**, a professional SaaS platform for financial document processing and commission tracking with AI-powered data extraction capabilities.

The C4 diagrams allow us to:
- **Visualize** the system architecture in a clear and understandable way
- **Communicate** the system structure to different audiences (developers, stakeholders, new team members)
- **Document** relationships between components and external dependencies
- **Facilitate** architectural decision-making and system maintenance

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
- **Insurance Agents**: Upload commission documents, review extracted data, configure field mappings
- **Brokers**: Process multiple documents, generate commission reports, manage clients
- **Administrators**: Manage users, configure allowed domains, supervise the system

**External Dependencies:**
- **Email Service (SMTP)**: OTP code delivery and notifications
- **AI Services**: Claude Document AI (primary extraction), Mistral (fallback), GPT-4 (AI operations)
- **Cloud Storage (GCS)**: PDF file storage and processed documents
- **Database (PostgreSQL)**: User data, companies, extractions and commissions persistence
- **Redis Cache**: Session management and temporary OTP storage

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
  - User interface for document upload
  - Extracted data visualization
  - Field mapping management
  - Reports and dashboards
  - OTP authentication
- **Key Components**: Dashboard, Carrier Management, Upload Zone, Table Editor

**Backend Container (FastAPI Backend)**
- **Technology**: FastAPI, Python 3.11+, SQLAlchemy, Pydantic
- **Responsibilities**:
  - REST API for CRUD operations
  - AI-powered document processing
  - Authentication and authorization
  - WebSocket for real-time updates
  - External service integration
- **Key Modules**: Auth, Extraction, Dashboard, Admin, WebSocket

**Database Container (PostgreSQL)**
- **Technology**: PostgreSQL with asyncpg
- **Responsibilities**:
  - User and company data storage
  - Extraction metadata
  - Field mapping configurations
  - Processed commission data
- **Key Tables**: users, companies, statement_uploads, extractions, earned_commissions

## Level 3: Component Diagram - Backend API# C4 Models Documentation - Commission Tracker

## Overview

Commission Tracker is a professional SaaS platform for financial document processing and commission tracking with AI-powered data extraction capabilities.

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
- **Insurance Agents**: Upload commission documents, review extracted data, configure field mappings
- **Brokers**: Process multiple documents, generate commission reports, manage clients
- **Administrators**: Manage users, configure allowed domains, supervise the system

**External Dependencies:**
- **Email Service (SMTP)**: OTP code delivery and notifications
- **AI Services**: Claude Document AI (primary extraction), Mistral (fallback), GPT-4 (AI operations)
- **Cloud Storage (GCS)**: PDF file storage and processed documents
- **Database (PostgreSQL)**: User data, companies, extractions and commissions persistence
- **Redis Cache**: Session management and temporary OTP storage

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
  - User interface for document upload
  - Extracted data visualization
  - Field mapping management
  - Reports and dashboards
  - OTP authentication
- **Key Components**: Dashboard, Carrier Management, Upload Zone, Table Editor

**Backend Container (FastAPI Backend)**
- **Technology**: FastAPI, Python 3.11+, SQLAlchemy, Pydantic
- **Responsibilities**:
  - REST API for CRUD operations
  - AI-powered document processing
  - Authentication and authorization
  - WebSocket for real-time updates
  - External service integration
- **Key Modules**: Auth, Extraction, Dashboard, Admin, WebSocket

**Database Container (PostgreSQL)**
- **Technology**: PostgreSQL with asyncpg
- **Responsibilities**:
  - User and company data storage
  - Extraction metadata
  - Field mapping configurations
  - Processed commission data
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
- **Auth API**: OTP authentication handling, login/logout, session management
- **Extraction API**: Document processing endpoints, table extraction, field mapping
- **Dashboard API**: Statistics, commission reports, analytical data
- **Admin API**: User management, allowed domains, system configurations
- **WebSocket API**: Real-time communication for extraction progress

**Service Layer:**
- **Auth Service**: Authentication logic, JWT generation, OTP validation
- **Extraction Service**: Document processing orchestration with multiple AI services
- **Dashboard Service**: Statistics calculations, commission data aggregations
- **Admin Service**: User management, permissions, configurations
- **WebSocket Service**: WebSocket connection management, progress notifications

**AI Services:**
- **Claude Service**: Primary extraction using Claude Document AI (superior accuracy)
- **Mistral Service**: Fallback extraction using Mistral Document AI
- **GPT-4 Service**: AI operations for field mapping, plan type detection

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
- **Landing Page**: Homepage with OTP authentication
- **Dashboard Page**: Main interface with statistics and navigation
- **Upload Page**: Document processing with real-time progress
- **Admin Page**: User management and configurations (admin only)

**Components:**
- **Auth Components**: OTP login/registration forms
- **Dashboard Components**: Statistics cards, charts, modals
- **Upload Components**: Upload zone, table editor, field mapping
- **Carrier Components**: Carrier management, data visualization
- **Table Components**: Table editor, data validation

**Context & State:**
- **Auth Context**: User state, permissions, authentication
- **Submission Context**: Upload state, extraction progress
- **Theme Context**: Interface theme, user preferences

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

*This C4 models documentation provides a comprehensive view of the Commission Tracker system architecture, from system context to specific implementation details.*


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
- **Auth API**: OTP authentication handling, login/logout, session management
- **Extraction API**: Document processing endpoints, table extraction, field mapping
- **Dashboard API**: Statistics, commission reports, analytical data
- **Admin API**: User management, allowed domains, system configurations
- **WebSocket API**: Real-time communication for extraction progress

**Service Layer:**
- **Auth Service**: Authentication logic, JWT generation, OTP validation
- **Extraction Service**: Document processing orchestration with multiple AI services
- **Dashboard Service**: Statistics calculations, commission data aggregations
- **Admin Service**: User management, permissions, configurations
- **WebSocket Service**: WebSocket connection management, progress notifications

**AI Services:**
- **Claude Service**: Primary extraction using Claude Document AI (superior accuracy)
- **Mistral Service**: Fallback extraction using Mistral Document AI
- **GPT-4 Service**: AI operations for field mapping, plan type detection

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
- **Landing Page**: Homepage with OTP authentication
- **Dashboard Page**: Main interface with statistics and navigation
- **Upload Page**: Document processing with real-time progress
- **Admin Page**: User management and configurations (admin only)

**Components:**
- **Auth Components**: OTP login/registration forms
- **Dashboard Components**: Statistics cards, charts, modals
- **Upload Components**: Upload zone, table editor, field mapping
- **Carrier Components**: Carrier management, data visualization
- **Table Components**: Table editor, data validation

**Context & State:**
- **Auth Context**: User state, permissions, authentication
- **Submission Context**: Upload state, extraction progress
- **Theme Context**: Interface theme, user preferences

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

*This C4 models documentation provides a comprehensive view of the Commission Tracker system architecture, from system context to specific implementation details.*
