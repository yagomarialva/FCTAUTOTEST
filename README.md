# ESD Monitor Management System

![Project Status](https://img.shields.io/badge/status-active-brightgreen)
![Backend](https://img.shields.io/badge/backend-.NET%20Core-purple)
![Frontend](https://img.shields.io/badge/frontend-React-blue)
![Embedded](https://img.shields.io/badge/embedded-C%2B%2B%20%2F%20Arduino-orange)

A comprehensive system for **ESD (Electrostatic Discharge) testing**, designed to manage production **lines**, **stations**, and **monitors**. It integrates physical components such as **wrist straps** and **jigs** using microcontrollers.

---

## 📋 Table of Contents

- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [Running the System](#-running-the-system)
- [API Endpoints](#-api-endpoints)
- [Testing (E2E)](#-e2e-testing-with-cypress)
- [Access Credentials](#-access-credentials)

---

## 📂 Project Structure

The project is organized into the following main directories:

### **Backend (`./backend`)**
RESTful API developed in **C# (.NET)** to manage data and business logic.
- **Controllers**: API endpoints.
- **Models**: Database entities.
- **Repositories**: Data access layer.
- **Services**: Business logic.
- **Hubs**: **SignalR** configuration for real-time communication.
- **SwaggerSettings**: API documentation config.

### **Frontend (`./frontend`)**
User interface developed in **React**.
- **src/components**: Reusable components (Tables, Forms, Modals).
- **src/pages**: Main views (Dashboard, ESD Management, Login).
- **src/context**: Global state management via Context API.

### **Embedded (`./embarcados`)**
Firmware for physical devices developed in **C++ (Arduino)**.
- **1ESD_Monitor**: Firmware for the ESD Monitor.
- **2ESD_BaseJig**: Firmware for the Testing Jig.

---

## 🚀 Getting Started

### Prerequisites
- .NET SDK
- Node.js & npm/yarn
- SQL Server (or configured database)
- Python (for IP script)

### Network Configuration
To ensure the backend communicates correctly with physical devices and the frontend on the local network:

1. **Find your Local IP:**
   Use the provided Python script to get your machine's IP address:
   ```python
   import socket

   def get_ip():
       s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
       s.connect(('8.8.8.8', 80))
       print(s.getsockname()[0])
       s.close()

   get_ip()
