# NexusHR Frontend - Developer Documentation

> [!CAUTION]
> This project is currently in development and may not be stable. Use at your own risk.

Welcome to the **NexusHR** frontend application developer guide. This application is a modern, responsive single-page application (SPA) built using React, TypeScript, and Vite, featuring a premium glassmorphic UI design with rich HSL theme styling.

---

## 🛠️ Tech Stack & Design System

1. **Core Framework**: React 18+ with TypeScript
2. **Build Tool**: Vite (configured with Rollup bundling)
3. **Styling**: Tailwind CSS + Custom Vanilla CSS for the custom glassmorphism design tokens (shadows, blurs, gradients, and custom micro-animations)
4. **Theme**: Dynamic Light/Dark Mode toggle (managed using client-side theme classes)
5. **Aesthetics**: Premium color palettes utilizing Outfit (headings) and Plus Jakarta Sans (body text) fonts

---

## 📂 Directory Layout

```text
nexushr-frontend/
├── src/
│   ├── assets/           # Dynamic assets, fonts, and icons
│   ├── types/
│   │   └── domain.ts     # Domain models & type definitions matching Spring Boot DTOs
│   ├── App.tsx           # Primary layout, routing logic, and central application state
│   ├── api.ts            # Centralized API service layer with authentication and headers wrapper
│   ├── index.css         # Tailwind directives, custom glassmorphism components, and HSL tokens
│   └── main.tsx          # React application bootstrapping entry point
├── .env.example          # Environment variables reference template
├── nginx.conf            # Custom Nginx configuration for production routing
├── Dockerfile            # Single-stage Alpine-Nginx deployment image builder
├── package.json          # Node scripts and dependencies
└── tsconfig.json         # TypeScript compiler configurations
```

---

## ⚙️ Environment Variables

The application communicates with the Spring Boot backend using a configurable base URL. Copy the example template to activate local configurations:

```bash
cp .env.example .env
```

Inside `.env`, define your backend URL prefix:
```env
VITE_API_BASE_URL=http://localhost:8080/api
```

---

## 🚀 Running the App Locally

### 1. Install Dependencies
```bash
npm install
```

### 2. Start the Development Server
```bash
npm run dev
```
The application will default to running on [http://localhost:5173](http://localhost:5173).

### 3. Compile Production Bundle
Verify TypeScript type-correctness and compile static production files:
```bash
npm run build
```

---

## 🔒 Role-Based Access Controls (RBAC)

The application implements client-side tab protections matching the Spring Boot backend credentials. Tabs are hidden or protected depending on the logged-in user's role:

| Tab Name | Icons / Purpose | Authorized Roles |
| :--- | :--- | :--- |
| **Dashboard** | Clock Tracker, Job Details, Recent Shifts | All Roles |
| **My Goals** | Goal assignments, toggle completions | All Roles |
| **Attendance** | Check-in/out logs, Full shift history table | All Roles (Managers see team logs) |
| **Leave Requests** | Submit absence request form, personal leave ledger | All Roles (Managers see approval tools) |
| **Team Directory** | Browse directory cards, link new users, appraisals | `MANAGER`, `ADMIN`, `HR` |
| **Reports** | Analytics charts, AI Predictive Attrition | `MANAGER`, `ADMIN`, `HR` |

---

## 🐳 Docker Deployment

The production-ready frontend runs inside an Nginx Alpine container:

```bash
# Build the production docker image
docker build -t nexushr-frontend:latest .

# Run the container mapping Nginx port 80 to host port 3000
docker run -d -p 3000:80 nexushr-frontend:latest
```

---

## 📋 Integration Specifications

* **Auth DTO Matching**: Coordinates payload exchanges for `/auth/login` and `/auth/register` endpoints.
* **JWT Handling**: Token is stored in client storage and attached dynamically as a `Bearer` token to the `Authorization` header on all secured API requests.
* **Profile Integration**: Uses `/employees/me` to safely initialize profile contexts, and `PUT /employees/{id}` to process profile corrections or directory modifications.

