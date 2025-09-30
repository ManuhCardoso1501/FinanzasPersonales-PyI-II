# 📚 Documentación del Sistema - Finanzas Personales

## 📑 Índice

1. [Arquitectura del Sistema](#arquitectura-del-sistema)
2. [Requisitos del Sistema](#requisitos-del-sistema)
3. [Instalación y Configuración](#instalación-y-configuración)
4. [Dependencias](#dependencias)
5. [Estructura de Carpetas](#estructura-de-carpetas)
6. [Guía de Uso](#guía-de-uso)
7. [API Reference](#api-reference)
8. [Base de Datos](#base-de-datos)
9. [CI/CD Pipeline](#cicd-pipeline)
10. [Deployment](#deployment)
11. [Troubleshooting](#troubleshooting)

---

## 🏗️ Arquitectura del Sistema

### Visión General

Finanzas Personales es una aplicación web full-stack construida con Next.js 14 (App Router), que implementa el patrón de arquitectura moderna para aplicaciones React:

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Next.js 14 (App Router) + React 18 + TypeScript     │  │
│  │  • Server Components                                  │  │
│  │  • Client Components                                  │  │
│  │  • Streaming & Suspense                              │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↕
┌─────────────────────────────────────────────────────────────┐
│                     API LAYER                                │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Next.js API Routes (app/api/*)                       │  │
│  │  • RESTful endpoints                                  │  │
│  │  • Request/Response handling                          │  │
│  │  • Data validation (Zod)                             │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↕
┌─────────────────────────────────────────────────────────────┐
│                   BUSINESS LOGIC                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Database Queries (lib/db.ts)                         │  │
│  │  • Smart connection handling                          │  │
│  │  • Graceful fallback to mock data                     │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↕
┌─────────────────────────────────────────────────────────────┐
│                     DATA LAYER                               │
│  ┌─────────────────┐          ┌──────────────────────────┐ │
│  │  PostgreSQL DB  │    OR    │  Mock Data (lib/mock)    │ │
│  │  (Neon)         │          │  (Development/Fallback)  │ │
│  └─────────────────┘          └──────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Principios de Diseño

1. **Server-First**: Utiliza Server Components por defecto para mejor rendimiento
2. **Progressive Enhancement**: Funciona sin JavaScript, mejora con él
3. **Type Safety**: TypeScript en todo el stack para prevenir errores
4. **Graceful Degradation**: Fallback automático a datos mock si la BD falla
5. **API-First**: Separación clara entre frontend y backend

### Tecnologías Core

- **Framework**: Next.js 14.2 (App Router)
- **Runtime**: React 18 con Server Components
- **Lenguaje**: TypeScript 5
- **Styling**: Tailwind CSS 4 + Radix UI
- **Database**: PostgreSQL (Neon Serverless)
- **Forms**: React Hook Form + Zod
- **Charts**: Recharts

---

## 📋 Requisitos del Sistema

### Requisitos de Software

| Software | Versión Mínima | Versión Recomendada | Propósito |
|----------|----------------|---------------------|-----------|
| Node.js  | 18.17.0        | 20.x LTS            | Runtime de JavaScript |
| npm      | 9.6.7          | 10.x                | Gestor de paquetes |
| PostgreSQL | 12.x         | 15.x                | Base de datos (opcional) |
| Git      | 2.x            | 2.40+               | Control de versiones |

### Requisitos de Hardware

**Desarrollo Local:**
- CPU: 2 cores mínimo
- RAM: 4 GB mínimo (8 GB recomendado)
- Disco: 1 GB espacio libre

**Producción:**
- Variable según el servicio de hosting elegido
- Vercel/Netlify: Maneja automáticamente

### Navegadores Soportados

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Opera 76+

---

## 🔧 Instalación y Configuración

### Instalación Paso a Paso

#### 1. Clonar el Repositorio

```bash
# HTTPS
git clone https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II.git

# SSH
git clone git@github.com:ManuhCardoso1501/FinanzasPersonales-PyI-II.git

cd FinanzasPersonales-PyI-II
```

#### 2. Instalar Dependencias

```bash
# Usando npm (recomendado)
npm install

# Usando pnpm (alternativa más rápida)
pnpm install

# Usando yarn (alternativa)
yarn install
```

#### 3. Configurar Variables de Entorno

```bash
# Copiar archivo de ejemplo
cp .env.example .env.local

# Editar con tu editor favorito
nano .env.local
# o
code .env.local
```

**Variables disponibles:**

```env
# Base de datos (OPCIONAL)
# Omitir para usar datos mock
DATABASE_URL=postgresql://user:pass@host:5432/dbname

# Para Neon Database
DATABASE_URL=postgresql://user:pass@ep-xxx.region.aws.neon.tech/neondb?sslmode=require

# Analytics (OPCIONAL)
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=your_analytics_id

# Entorno
NODE_ENV=development  # development | production | test
```

#### 4. Configurar Base de Datos (Opcional)

**Opción A: Sin Base de Datos (Mock Data)**
- No hacer nada adicional
- La aplicación funcionará con datos de prueba

**Opción B: Con PostgreSQL Local**

```bash
# 1. Crear base de datos
createdb finanzas_personales

# 2. Ejecutar scripts SQL
psql finanzas_personales < scripts/01-create-tables.sql
psql finanzas_personales < scripts/02-seed-categories.sql

# 3. Configurar DATABASE_URL
DATABASE_URL=postgresql://localhost:5432/finanzas_personales
```

**Opción C: Con Neon Database (Recomendado para producción)**

```bash
# 1. Crear cuenta en https://neon.tech
# 2. Crear un nuevo proyecto
# 3. Copiar la connection string
# 4. Ejecutar scripts vía psql

psql "postgresql://your-connection-string" < scripts/01-create-tables.sql
psql "postgresql://your-connection-string" < scripts/02-seed-categories.sql
```

#### 5. Verificar Instalación

```bash
# Probar conexión a base de datos
node scripts/test-connection.js

# Compilar proyecto
npm run build

# Iniciar en modo desarrollo
npm run dev
```

Abrir http://localhost:3000 en el navegador.

### Configuración Avanzada

#### ESLint

El proyecto incluye ESLint pre-configurado:

```bash
# Ejecutar linter
npm run lint

# Auto-fix de problemas
npm run lint -- --fix
```

#### TypeScript

```bash
# Verificar tipos sin compilar
npx tsc --noEmit

# Generar tipos
npx tsc --declaration --emitDeclarationOnly
```

---

## 📦 Dependencias

### Dependencias de Producción

| Paquete | Versión | Descripción |
|---------|---------|-------------|
| next | 14.2.16 | Framework React con SSR |
| react | 18.x | Biblioteca UI |
| react-dom | 18.x | React para web |
| typescript | 5.x | Lenguaje con tipado estático |
| @neondatabase/serverless | latest | Cliente PostgreSQL serverless |
| tailwindcss | 4.1.9 | Framework CSS utility-first |
| @radix-ui/* | varies | Componentes accesibles |
| lucide-react | 0.454.0 | Iconos |
| recharts | latest | Gráficos y visualizaciones |
| react-hook-form | 7.60.0 | Manejo de formularios |
| zod | 3.25.67 | Validación de esquemas |
| date-fns | 4.1.0 | Utilidades de fechas |
| next-themes | 0.4.6 | Soporte para temas |
| @vercel/analytics | latest | Analytics |

### Dependencias de Desarrollo

| Paquete | Versión | Descripción |
|---------|---------|-------------|
| eslint | 8.57.0 | Linter de JavaScript/TypeScript |
| eslint-config-next | latest | Configuración ESLint para Next.js |
| @types/node | 22.x | Tipos de Node.js |
| @types/react | 18.x | Tipos de React |
| @types/react-dom | 18.x | Tipos de React DOM |
| postcss | 8.5.x | Procesador CSS |
| @tailwindcss/postcss | 4.1.9 | Plugin Tailwind para PostCSS |

### Actualizar Dependencias

```bash
# Verificar paquetes desactualizados
npm outdated

# Actualizar todos a la última versión compatible
npm update

# Actualizar a versiones mayores (cuidado!)
npm install -g npm-check-updates
ncu -u
npm install
```

---

## 📁 Estructura de Carpetas

```
FinanzasPersonales-PyI-II/
│
├── .github/                    # Configuración de GitHub
│   └── workflows/              # GitHub Actions workflows
│       └── ci.yml              # Pipeline CI/CD
│
├── app/                        # Directorio principal de Next.js App Router
│   ├── api/                    # API Routes
│   │   ├── accounts/           # CRUD de cuentas
│   │   │   ├── route.ts        # GET, POST /api/accounts
│   │   │   └── [id]/
│   │   │       └── route.ts    # GET, PATCH, DELETE /api/accounts/:id
│   │   ├── categories/
│   │   │   └── route.ts        # GET /api/categories
│   │   ├── transactions/       # CRUD de transacciones
│   │   │   ├── route.ts        # GET, POST /api/transactions
│   │   │   └── [id]/
│   │   │       └── route.ts    # GET, PATCH, DELETE /api/transactions/:id
│   │   ├── dashboard/
│   │   │   └── metrics/
│   │   │       └── route.ts    # GET /api/dashboard/metrics
│   │   └── reports/
│   │       └── expenses-by-category/
│   │           └── route.ts    # GET /api/reports/expenses-by-category
│   │
│   ├── cuentas/                # Página de gestión de cuentas
│   │   └── page.tsx
│   ├── transacciones/          # Página de transacciones
│   │   └── page.tsx
│   ├── reportes/               # Página de reportes
│   │   └── page.tsx
│   │
│   ├── layout.tsx              # Layout raíz (HTML, metadata)
│   ├── page.tsx                # Página principal (dashboard)
│   └── globals.css             # Estilos globales
│
├── components/                 # Componentes React reutilizables
│   ├── ui/                     # Componentes UI base (Radix)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   ├── form.tsx
│   │   ├── input.tsx
│   │   ├── select.tsx
│   │   ├── table.tsx
│   │   └── ... (más componentes)
│   │
│   ├── accounts/               # Componentes específicos de cuentas
│   ├── transactions/           # Componentes de transacciones
│   ├── dashboard/              # Componentes del dashboard
│   └── reports/                # Componentes de reportes
│
├── lib/                        # Lógica de negocio y utilidades
│   ├── db.ts                   # Funciones de base de datos
│   ├── mock-data.ts            # Datos mock para desarrollo
│   ├── types.ts                # Tipos TypeScript compartidos
│   ├── format.ts               # Funciones de formateo
│   └── utils.ts                # Utilidades generales
│
├── scripts/                    # Scripts de utilidad
│   ├── 01-create-tables.sql    # Script creación de tablas
│   ├── 02-seed-categories.sql  # Script datos iniciales
│   └── test-connection.js      # Verificar conexión BD
│
├── public/                     # Archivos estáticos
│   ├── favicon.ico
│   └── images/
│
├── styles/                     # Estilos adicionales
│
├── .env.example                # Ejemplo de variables de entorno
├── .env.local                  # Variables locales (no committed)
├── .eslintrc.json              # Configuración ESLint
├── .gitignore                  # Archivos ignorados por Git
├── components.json             # Configuración de componentes UI
├── next.config.mjs             # Configuración de Next.js
├── package.json                # Dependencias y scripts
├── postcss.config.mjs          # Configuración PostCSS
├── tsconfig.json               # Configuración TypeScript
├── tailwind.config.ts          # Configuración Tailwind (si existe)
├── README.md                   # Documentación principal
├── CONTRIBUTING.md             # Guía de contribución
├── LICENSE                     # Licencia MIT
└── test-database-integration.md # Documentación de pruebas
```

### Convenciones de Nombres

- **Componentes**: PascalCase (`TransactionList.tsx`)
- **Utilities**: camelCase (`formatCurrency.ts`)
- **API Routes**: kebab-case en URLs, camelCase en código
- **Constants**: SCREAMING_SNAKE_CASE (`MAX_RETRIES`)
- **Types/Interfaces**: PascalCase (`Transaction`, `Account`)

---

## 📖 Guía de Uso

### Uso Básico

#### 1. Iniciar la Aplicación

```bash
# Modo desarrollo (con hot reload)
npm run dev

# Modo producción
npm run build
npm start
```

#### 2. Acceder a la Aplicación

- **Dashboard**: http://localhost:3000
- **Cuentas**: http://localhost:3000/cuentas
- **Transacciones**: http://localhost:3000/transacciones
- **Reportes**: http://localhost:3000/reportes

### Casos de Uso Comunes

#### Crear una Nueva Cuenta

1. Navegar a `/cuentas`
2. Click en "Nueva Cuenta"
3. Llenar el formulario:
   - Nombre de la cuenta
   - Tipo (efectivo/banco/tarjeta)
   - Balance inicial
   - Moneda
4. Guardar

#### Registrar una Transacción

1. Navegar a `/transacciones`
2. Click en "Nueva Transacción"
3. Seleccionar:
   - Tipo (ingreso/gasto)
   - Cuenta
   - Categoría
   - Monto
   - Descripción
   - Fecha
4. Guardar

#### Ver Reportes

1. Navegar a `/reportes`
2. Seleccionar período de tiempo
3. Ver gráficos y análisis

### Modo Mock vs Base de Datos

**Modo Mock (Sin DATABASE_URL):**
- Datos de ejemplo precargados
- Sin persistencia
- Ideal para desarrollo y demos

**Modo Base de Datos (Con DATABASE_URL):**
- Datos reales persistentes
- Múltiples usuarios
- Para producción

---

## 🔌 API Reference

### Categorías

#### GET /api/categories

Obtener lista de categorías.

**Query Parameters:**
- `type` (opcional): `"ingreso"` | `"gasto"`

**Response:**
```json
[
  {
    "id": 1,
    "name": "Salario",
    "type": "ingreso",
    "icon": "Wallet",
    "color": "hsl(145, 60%, 45%)",
    "created_at": "2025-01-20T00:00:00.000Z"
  }
]
```

### Cuentas

#### GET /api/accounts

Obtener lista de cuentas.

**Query Parameters:**
- `includeArchived` (opcional): `boolean`

**Response:**
```json
[
  {
    "id": 1,
    "name": "Cuenta Principal",
    "type": "banco",
    "balance": 5000.00,
    "currency": "USD",
    "is_archived": false,
    "created_at": "2025-01-20T00:00:00.000Z",
    "updated_at": "2025-01-20T00:00:00.000Z"
  }
]
```

#### POST /api/accounts

Crear nueva cuenta.

**Request Body:**
```json
{
  "name": "Mi Cuenta",
  "type": "banco",
  "balance": 1000,
  "currency": "USD"
}
```

#### PATCH /api/accounts/:id

Actualizar cuenta existente.

#### DELETE /api/accounts/:id

Eliminar cuenta.

### Transacciones

#### GET /api/transactions

Obtener lista de transacciones.

**Query Parameters:**
- `type`: `"ingreso"` | `"gasto"`
- `accountId`: number
- `categoryId`: number
- `startDate`: ISO date string
- `endDate`: ISO date string

**Response:**
```json
[
  {
    "id": 1,
    "account_id": 1,
    "category_id": 6,
    "type": "gasto",
    "amount": 150.50,
    "description": "Compras",
    "date": "2025-01-20",
    "account_name": "Cuenta Principal",
    "category_name": "Alimentación",
    "category_icon": "UtensilsCrossed",
    "category_color": "hsl(25, 70%, 50%)",
    "created_at": "2025-01-20T00:00:00.000Z",
    "updated_at": "2025-01-20T00:00:00.000Z"
  }
]
```

#### POST /api/transactions

Crear transacción.

#### PATCH /api/transactions/:id

Actualizar transacción.

#### DELETE /api/transactions/:id

Eliminar transacción.

### Dashboard

#### GET /api/dashboard/metrics

Obtener métricas del dashboard.

**Query Parameters:**
- `startDate`: ISO date string
- `endDate`: ISO date string

**Response:**
```json
{
  "totalIncome": 5000.00,
  "totalExpenses": 2500.00,
  "balance": 2500.00,
  "accountsCount": 3,
  "transactionsCount": 15
}
```

### Reportes

#### GET /api/reports/expenses-by-category

Reportes de gastos por categoría.

**Response:**
```json
[
  {
    "category_name": "Alimentación",
    "category_icon": "UtensilsCrossed",
    "category_color": "hsl(25, 70%, 50%)",
    "total": 800.00,
    "percentage": 32.0
  }
]
```

---

## 🗄️ Base de Datos

### Esquema Completo

Consultar [README.md](README.md#-esquema-de-base-de-datos) para esquema detallado.

### Migraciones

Actualmente el proyecto usa scripts SQL directos:

```bash
# Crear tablas
psql $DATABASE_URL < scripts/01-create-tables.sql

# Insertar datos iniciales
psql $DATABASE_URL < scripts/02-seed-categories.sql
```

### Backup y Restore

```bash
# Backup
pg_dump $DATABASE_URL > backup.sql

# Restore
psql $DATABASE_URL < backup.sql
```

---

## 🚀 CI/CD Pipeline

### GitHub Actions Workflow

El proyecto incluye un workflow de CI/CD en `.github/workflows/ci.yml`:

**Jobs Configurados:**

1. **build-and-test**
   - Ejecuta en Node.js 18.x y 20.x (matrix)
   - Instala dependencias
   - Ejecuta linter
   - Compila la aplicación
   - Verifica build exitoso

2. **lint**
   - Ejecuta ESLint
   - Verifica tipos TypeScript
   - Reporta problemas de calidad

3. **security**
   - Auditoría de seguridad (npm audit)
   - Verifica paquetes desactualizados

**Triggers:**
- Push a `main` o `develop`
- Pull requests a `main` o `develop`

### Workflow Local

```bash
# Simular el workflow localmente
npm ci                    # Instalar (como en CI)
npm run lint             # Verificar código
npm run build            # Compilar
```

---

## 🌐 Deployment

### Vercel (Recomendado)

1. Conectar repositorio en https://vercel.com
2. Configurar variables de entorno
3. Deploy automático en cada push

### Netlify

```bash
# netlify.toml
[build]
  command = "npm run build"
  publish = ".next"

[build.environment]
  NODE_VERSION = "20"
```

### Railway

1. Conectar repositorio
2. Añadir DATABASE_URL
3. Deploy

### Docker

```dockerfile
# Dockerfile (ejemplo)
FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

EXPOSE 3000
CMD ["npm", "start"]
```

---

## 🔍 Troubleshooting

### Problemas Comunes

**Build falla con error de TypeScript**
```bash
# Limpiar cache
rm -rf .next node_modules
npm install
npm run build
```

**ESLint errores de configuración**
```bash
# Reinstalar ESLint
npm install --save-dev eslint@^8.57.0 eslint-config-next
```

**Base de datos no conecta**
```bash
# Verificar conexión
node scripts/test-connection.js

# Verificar variable de entorno
echo $DATABASE_URL
```

**Hot reload no funciona**
```bash
# Reiniciar servidor
# Verificar puertos en uso
lsof -i :3000
```

### Logs y Debugging

```bash
# Ver logs de producción
vercel logs

# Debug local
NODE_ENV=development npm run dev
```

---

## 📞 Soporte

- 📖 [README](README.md)
- 🤝 [Guía de Contribución](CONTRIBUTING.md)
- 🐛 [Reportar Issues](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/issues)
- 💬 [Discussions](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/discussions)

---

**Documentación actualizada:** Enero 2025
**Versión:** 0.1.0
