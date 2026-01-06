# FCT AutoTest - ESD Testing Management System

![Project Status](https://img.shields.io/badge/status-active-brightgreen)
![Backend](https://img.shields.io/badge/backend-.NET%208.0-purple)
![Frontend](https://img.shields.io/badge/frontend-React%2018-blue)
![Database](https://img.shields.io/badge/database-Oracle%20XE-orange)
![Embedded](https://img.shields.io/badge/embedded-C%2B%2B%20%2F%20Arduino-yellow)

A comprehensive system for **ESD (Electrostatic Discharge) testing** in production environments, designed to manage **production lines**, **test stations**, **ESD monitors**, and **jigs**. The system integrates physical components such as **anti-static wrist straps** and **test jigs** using Arduino microcontrollers, providing real-time monitoring and quality control.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Project Structure](#-project-structure)
- [Technology Stack](#-technology-stack)
- [Prerequisites](#-prerequisites)
- [Installation and Configuration](#-installation-and-configuration)
- [Running the System](#-running-the-system)
- [Network Configuration](#-network-configuration)
- [API Endpoints](#-api-endpoints)
- [Authentication](#-authentication)
- [Docker](#-docker)
- [Development](#-development)
- [Database Structure](#-database-structure)
- [Embedded Components](#-embedded-components)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)

---

## 🎯 Overview

**FCT AutoTest** is a complete solution for automation and monitoring of ESD tests in production lines. The system provides:

- **Real-Time Monitoring**: Bidirectional communication with physical devices via SignalR
- **Station Management**: Control of test stations, production lines, and ESD monitors
- **Operator Management**: Authentication system and role-based access control
- **Traceability**: Complete logging of production activities and test status
- **Modern Interface**: Intuitive dashboard with real-time visualizations
- **Facial Biometrics**: Integration with facial recognition system for authentication

---

## 📂 Project Structure

```
FCTAUTOTEST/
├── backend/                    # REST API in .NET 8.0
│   ├── Controllers/           # API endpoints
│   ├── Models/                # Database entities
│   ├── Repositories/          # Data access layer
│   ├── Services/              # Business logic
│   ├── Hubs/                  # SignalR Hubs (real-time communication)
│   ├── Authentication/        # JWT configuration
│   ├── Security/              # Security and encryption services
│   ├── Middleware/            # Custom middlewares
│   ├── Data/                  # Entity Framework context
│   ├── SwaggerSettings/       # Swagger configuration
│   ├── SqlInterceptor/        # SQL interceptors
│   ├── OraScripts/            # SQL scripts
│   ├── Dockerfile             # Backend Docker image
│   └── Program.cs             # Application entry point
│
├── frontend/                   # React interface
│   ├── src/
│   │   ├── api/              # API clients and services
│   │   ├── components/       # Reusable components
│   │   │   ├── ESD/         # ESD-specific components
│   │   │   └── Navbar/      # Navigation
│   │   ├── pages/           # Main pages
│   │   │   ├── DashboardPage/
│   │   │   ├── ESD/
│   │   │   ├── LoginPage/
│   │   │   └── Menu/
│   │   ├── context/         # Context API (state management)
│   │   └── models/          # TypeScript models
│   ├── public/              # Static files
│   ├── Dockerfile           # Frontend Docker image
│   └── nginx.conf           # Nginx configuration
│
├── embarcados/               # Firmware for physical devices
│   ├── 1ESD_Monitor/        # ESD Monitor firmware
│   └── 2ESD_BaseJig/        # Test Jig firmware
│
├── init-scripts/             # Database initialization scripts
│   ├── 01_create_schema.sql
│   └── 02_create_table_FCT_TEST.sql
│
├── docker-compose.yaml       # Container orchestration
├── start.ps1                 # Startup script (PowerShell)
└── README.md                 # This file
```

---

## 🛠 Technology Stack

### Backend
- **.NET 8.0** - Main framework
- **Entity Framework Core 6.0** - ORM
- **Oracle.EntityFrameworkCore 6.21.4** - Oracle provider
- **SignalR** - Real-time communication
- **JWT Bearer** - Authentication
- **AutoMapper** - Object mapping
- **Dapper** - High-performance data access
- **Swagger/OpenAPI** - API documentation
- **NodaTime** - Date/time handling

### Frontend
- **React 18.1** - UI framework
- **TypeScript** - Static typing
- **Material-UI (MUI)** - UI components
- **Ant Design** - Component library
- **React Query (TanStack Query)** - Server state management
- **SignalR Client** - Real-time communication client
- **ApexCharts** - Charts and visualizations
- **Axios** - HTTP client
- **React Router** - Routing
- **i18next** - Internationalization

### Database
- **Oracle Database XE 11g** - Relational database

### Infrastructure
- **Docker** - Containerization
- **Docker Compose** - Orchestration
- **Nginx** - Web server for frontend
- **Python** - Automation scripts

### Embedded
- **Arduino/C++** - Microcontroller firmware

---

## 📋 Prerequisites

Before getting started, make sure you have installed:

### Local Development
- [.NET SDK 8.0](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Node.js 16+](https://nodejs.org/) and npm/yarn
- [Oracle Database XE 11g](https://www.oracle.com/database/technologies/xe-downloads.html) or use Docker
- [Python 3.x](https://www.python.org/downloads/)
- [Git](https://git-scm.com/)

### With Docker (Recommended)
- [Docker Desktop](https://www.docker.com/products/docker-desktop) or Docker Engine
- [Docker Compose](https://docs.docker.com/compose/install/)

### Additional Tools
- [Arduino IDE](https://www.arduino.cc/en/software) (for embedded development)
- Code editor (VS Code, Visual Studio, Rider, etc.)

---

## 🚀 Installation and Configuration

### 1. Clone the Repository

```bash
git clone <repository-url>
cd FCTAUTOTEST
```

### 2. Backend Configuration

#### 2.1. Install Python Dependencies

```bash
cd backend
python -m pip install --upgrade pip
pip install psutil
```

#### 2.2. Generate .env File

The Python script automatically detects the Wi-Fi interface IP and creates the `.env` file:

```bash
python generate_env.py
```

This will create a `.env` file with:
```
DB_HOST=<your-local-ip>
```

#### 2.3. Configure appsettings.json

Edit `backend/appsettings.json` and adjust the settings:

```json
{
  "ConnectionStrings": {
    "ora": "User Id=system;Password=oracle;Data Source=${DB_HOST}:49000/xe"
  },
  "jwt": {
    "audience": "http://calcomp-icct.org.br",
    "issuer": "calcomp-icct.org.br",
    "secretKey": "your-secret-key-here"
  },
  "security": {
    "key": "your-encryption-key",
    "iv": "your-encryption-iv"
  }
}
```

### 3. Frontend Configuration

#### 3.1. Install Dependencies

```bash
cd frontend
npm install
```

#### 3.2. Generate .env File

```bash
python generate_env_frontend.py
```

This will create a `.env` file with:
```
REACT_APP_API_URL_FCT=http://<your-local-ip>:7080/
REACT_APP_HOST=<your-local-ip>
```

### 4. Database Configuration

#### 4.1. Using Docker (Recommended)

Docker Compose already includes Oracle Database configuration. The initialization scripts in `init-scripts/` will be executed automatically.

#### 4.2. Manual Configuration

If you prefer to use an existing Oracle Database:

1. Execute the scripts in `init-scripts/` in order:
   - `01_create_schema.sql` - Creates user and schema
   - `02_create_table_FCT_TEST.sql` - Creates tables

2. Adjust the connection string in `appsettings.json`

---

## 🏃 Running the System

### Option 1: Docker Compose (Recommended)

#### Windows (PowerShell)

```powershell
.\start.ps1
```

#### Linux/Mac

```bash
# Generate .env files
cd backend && python generate_env.py && cd ..
cd frontend && python generate_env_frontend.py && cd ..

# Start containers
docker-compose up -d --build
```

#### Check Status

```bash
docker-compose ps
```

#### View Logs

```bash
# All services
docker-compose logs -f

# Backend only
docker-compose logs -f api

# Frontend only
docker-compose logs -f frontend

# Database only
docker-compose logs -f oracle-db
```

#### Stop Containers

```bash
docker-compose down
```

#### Stop and Remove Volumes

```bash
docker-compose down -v
```

### Option 2: Local Development

#### Backend

```bash
cd backend
dotnet restore
dotnet build
dotnet run
```

The backend will be available at:
- HTTP: `http://localhost:7080`
- Swagger: `http://localhost:7080/swagger`

#### Frontend

```bash
cd frontend
npm install
npm start
```

The frontend will be available at:
- `http://localhost:3000`

#### Database

Make sure Oracle Database is running and accessible on the configured port.

---

## 🌐 Network Configuration

For the system to work correctly with physical devices on the local network:

### 1. Detect Local IP

The Python scripts (`generate_env.py` and `generate_env_frontend.py`) automatically detect the Wi-Fi interface IP. If you need to adjust manually:

```python
# backend/generate_env.py
# Modify the get_wifi_ip_address() function to use your specific interface
```

### 2. Configure CORS

The backend is configured to accept connections from:
- `http://localhost:3000`
- `http://<your-local-ip>:3000`

To add more origins, edit `backend/Program.cs`:

```csharp
options.AddPolicy("AllowSpecificOrigins", policy =>
     policy
         .WithOrigins("http://localhost:3000", 
                     "http://192.168.0.102:3000", 
                     $"http://{oracleHost}:3000")
         .AllowAnyHeader()
         .AllowAnyMethod()
         .AllowCredentials());
```

### 3. System Ports

- **Backend API**: `7080` (host) → `8080` (container)
- **Frontend**: `3000` (host) → `80` (container)
- **Oracle Database**: `49000` (host) → `1521` (container)
- **SignalR Hub**: `/loghub` (same host as backend)

---

## 📡 API Endpoints

### Authentication

- `POST /api/Authentication/login` - User login
- `POST /api/Authentication/register` - Register new user

### Users

- `GET /api/User` - List users
- `GET /api/User/{id}` - Get user by ID
- `POST /api/User` - Create user
- `PUT /api/User/{id}` - Update user
- `DELETE /api/User/{id}` - Delete user

### Stations

- `GET /api/Station` - List stations
- `GET /api/Station/{id}` - Get station by ID
- `POST /api/Station` - Create station
- `PUT /api/Station/{id}` - Update station
- `DELETE /api/Station/{id}` - Delete station

### Production Lines

- `GET /api/Line` - List lines
- `GET /api/Line/{id}` - Get line by ID
- `POST /api/Line` - Create line
- `PUT /api/Line/{id}` - Update line
- `DELETE /api/Line/{id}` - Delete line

### ESD Monitors

- `GET /api/MonitorEsd` - List monitors
- `GET /api/MonitorEsd/{id}` - Get monitor by ID
- `POST /api/MonitorEsd` - Create monitor
- `PUT /api/MonitorEsd/{id}` - Update monitor
- `DELETE /api/MonitorEsd/{id}` - Delete monitor

### Jigs

- `GET /api/Jig` - List jigs
- `GET /api/Jig/{id}` - Get jig by ID
- `POST /api/Jig` - Create jig
- `PUT /api/Jig/{id}` - Update jig
- `DELETE /api/Jig/{id}` - Delete jig

### Production Activities

- `GET /api/ProduceActivity` - List activities
- `POST /api/ProduceActivity` - Register activity

### ESD Logs

- `GET /api/LogMonitorEsd` - List logs
- `GET /api/LogMonitorEsd/last` - Latest logs

### Biometrics

- `POST /api/Biometric` - Process facial biometrics

### SignalR Hub

- **Endpoint**: `/loghub`
- **Available methods**: See `backend/Hubs/CommunicationHub.cs`

### Swagger Documentation

Access `http://localhost:7080/swagger` for interactive API documentation.

---

## 🔐 Authentication

The system uses **JWT (JSON Web Tokens)** for authentication.

### Authentication Flow

1. **Login**: Send credentials to `/api/Authentication/login`
2. **Token**: Receive JWT token in response
3. **Usage**: Include token in `Authorization: Bearer <token>` header

### Example Request

```bash
curl -X POST http://localhost:7080/api/Authentication/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "user",
    "password": "password"
  }'
```

### Example with Token

```bash
curl -X GET http://localhost:7080/api/User \
  -H "Authorization: Bearer <your-jwt-token>"
```

### JWT Configuration

JWT settings are in `backend/appsettings.json`:

```json
{
  "jwt": {
    "audience": "http://calcomp-icct.org.br",
    "issuer": "calcomp-icct.org.br",
    "secretKey": "your-secret-key-here"
  }
}
```

**⚠️ IMPORTANT**: Change the `secretKey` in production!

---

## 🐳 Docker

### Container Structure

The `docker-compose.yaml` defines three services:

1. **api** (Backend)
   - Image: `.NET 8.0`
   - Port: `7080:8080`
   - Healthcheck: `/health`

2. **oracle-db** (Database)
   - Image: `oracleinanutshell/oracle-xe-11g`
   - Port: `49000:1521`
   - Volume: `oracle-data` (persistence)

3. **frontend** (Web Interface)
   - Image: `nginx:alpine`
   - Port: `3000:80`
   - Build: React app

### Manual Build

```bash
# Backend
cd backend
docker build -t fct-backend .

# Frontend
cd frontend
docker build -t fct-frontend .
```

### Environment Variables

Environment variables can be configured in `docker-compose.yaml` or in `.env` files.

---

## 💻 Development

### Code Structure

#### Backend

- **Controllers**: REST endpoints
- **Repositories**: Repository pattern for data access
- **Services**: Business logic
- **Models**: Database entities
- **Hubs**: SignalR for real-time communication

#### Frontend

- **Components**: Reusable React components
- **Pages**: Main application pages
- **API**: HTTP clients and services
- **Context**: Global state management

### Adding a New Endpoint

1. Create Model in `backend/Models/`
2. Create Repository in `backend/Repositories/`
3. Create Service in `backend/Services/`
4. Create Controller in `backend/Controllers/`
5. Register in `Program.cs`

### Adding a New Frontend Component

1. Create component in `frontend/src/components/`
2. Create API client in `frontend/src/api/` (if needed)
3. Add route in `frontend/src/AppRoutes.tsx`

### Hot Reload

- **Backend**: `dotnet watch run`
- **Frontend**: `npm start` (already includes hot reload)

---

## 🗄️ Database Structure

### Main Schema

The database uses the `FCT_AUTO_TEST` schema (user: `fct_auto_test`).

### Main Tables

- **Users**: Operator management
- **Stations**: Test stations
- **Lines**: Production lines
- **ESD Monitors**: Monitoring devices
- **Jigs**: Test jigs
- **Logs**: Activity records
- **Production Activities**: Production history
- **Biometrics**: Facial recognition data

### Initialization Scripts

The scripts in `init-scripts/` are automatically executed when the Oracle container is created for the first time.

---

## 🔌 Embedded Components

### 1ESD_Monitor

Firmware for the ESD monitor that:
- Monitors anti-static wrist strap status
- Communicates with backend via network
- Sends real-time data via SignalR

### 2ESD_BaseJig

Firmware for the test jig that:
- Controls ESD tests on jigs
- Validates connections
- Reports status to the system

### Uploading Firmware

1. Open the `.ino` file in Arduino IDE
2. Configure board and port
3. Adjust network settings (backend IP)
4. Upload to device

---

## 🔧 Troubleshooting

### Issue: Backend cannot connect to database

**Solution**:
1. Check if Oracle is running: `docker-compose ps`
2. Check connection string in `appsettings.json`
3. Check `.env` file in backend
4. Check logs: `docker-compose logs api`

### Issue: Frontend cannot connect to backend

**Solution**:
1. Check `.env` file in frontend
2. Check if backend is running
3. Check CORS in `Program.cs`
4. Check browser console for errors

### Issue: SignalR not working

**Solution**:
1. Check if hub is mapped in `Program.cs`
2. Check SignalR URL in frontend
3. Check firewall/proxy
4. Check backend logs

### Issue: Python scripts cannot find IP

**Solution**:
1. Check if `psutil` is installed
2. Adjust interface name in `get_wifi_ip_address()`
3. Run `list_interfaces()` to see available interfaces

### Issue: Docker build fails

**Solution**:
1. Clear cache: `docker system prune -a`
2. Rebuild without cache: `docker-compose build --no-cache`
3. Check Dockerfile for syntax errors

---

## 📝 Environment Variables

### Backend (.env)

```
DB_HOST=<local-ip>
```

### Frontend (.env)

```
REACT_APP_API_URL_FCT=http://<local-ip>:7080/
REACT_APP_HOST=<local-ip>
```

### Docker Compose

Variables can be defined in `docker-compose.yaml` or in a `.env` file at the root.

---

## 🧪 Testing

### Backend

```bash
cd backend
dotnet test
```

### Frontend

```bash
cd frontend
npm test
```

---

## 📦 Production Build

### Backend

```bash
cd backend
dotnet publish -c Release -o ./publish
```

### Frontend

```bash
cd frontend
npm run build
```

Production files will be in:
- Backend: `backend/publish/`
- Frontend: `frontend/build/`

---

## 🤝 Contributing

1. Fork the project
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is proprietary. All rights reserved.

---

## 👥 Authors

- **FCT Development Team**

---

## 📞 Support

For support, contact the development team or open an issue in the repository.

---

## 🔄 Changelog

### Version 0.1.0
- Initial system implementation
- .NET 8.0 backend
- React 18 frontend
- Oracle Database integration
- JWT authentication system
- Real-time communication with SignalR
- Firmware for embedded devices

---

## 🎯 Roadmap

- [ ] Automated tests (unit and integration)
- [ ] Expanded API documentation
- [ ] Advanced metrics dashboard
- [ ] Report export
- [ ] Push notifications
- [ ] Mobile app

---

**Developed with ❤️ by the FCT team**
