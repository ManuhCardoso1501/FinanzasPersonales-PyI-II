# 💰 Finanzas Personales

[![Build Status](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/workflows/CI/badge.svg)](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Next.js](https://img.shields.io/badge/Next.js-14.2-black)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue)](https://www.typescriptlang.org/)

Una aplicación web moderna para la gestión de finanzas personales, construida con Next.js 14, React y TypeScript. Permite a los usuarios gestionar cuentas, transacciones y generar reportes financieros de manera simple y efectiva.

## ✨ Características

- 📊 **Dashboard interactivo** - Visualiza tus métricas financieras en tiempo real
- 💳 **Gestión de cuentas** - Administra múltiples cuentas (efectivo, banco, tarjetas)
- 💸 **Registro de transacciones** - Lleva control de ingresos y gastos
- 📈 **Reportes detallados** - Analiza tus gastos por categoría
- 🎨 **Interfaz moderna** - UI responsiva construida con Tailwind CSS y Radix UI
- 🗄️ **Doble modo de operación** - Funciona con base de datos PostgreSQL o datos mock
- ⚡ **Rendimiento optimizado** - Server-side rendering con Next.js 14
- 🔒 **Type-safe** - Desarrollo completo en TypeScript

## 📋 Requisitos Previos

- **Node.js** 18.x o superior
- **npm** 9.x o superior (o pnpm/yarn)
- **PostgreSQL** (opcional, para modo producción)
- **Neon Database** (opcional, para despliegue en la nube)

## 🚀 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II.git
cd FinanzasPersonales-PyI-II
```

### 2. Instalar dependencias

```bash
npm install
```

### 3. Configurar variables de entorno

Crea un archivo `.env.local` en la raíz del proyecto:

```env
# Base de datos (opcional)
DATABASE_URL=postgresql://usuario:contraseña@host:puerto/database

# Analytics (opcional)
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=tu-id-de-analytics
```

**Nota:** Si no configuras `DATABASE_URL`, la aplicación funcionará en modo mock con datos de prueba.

### 4. Configurar la base de datos (opcional)

Si deseas usar una base de datos real:

```bash
# Crear las tablas
psql $DATABASE_URL -f scripts/01-create-tables.sql

# Insertar categorías iniciales
psql $DATABASE_URL -f scripts/02-seed-categories.sql

# Verificar la conexión
node scripts/test-connection.js
```

### 5. Ejecutar en desarrollo

```bash
npm run dev
```

La aplicación estará disponible en [http://localhost:3000](http://localhost:3000)

## 🏗️ Estructura del Proyecto

```
FinanzasPersonales-PyI-II/
├── app/                      # Páginas y rutas de Next.js App Router
│   ├── api/                  # API Routes
│   │   ├── accounts/         # Endpoints de cuentas
│   │   ├── categories/       # Endpoints de categorías
│   │   ├── transactions/     # Endpoints de transacciones
│   │   ├── dashboard/        # Endpoints de métricas
│   │   └── reports/          # Endpoints de reportes
│   ├── cuentas/              # Página de gestión de cuentas
│   ├── transacciones/        # Página de transacciones
│   ├── reportes/             # Página de reportes
│   ├── layout.tsx            # Layout principal
│   └── page.tsx              # Página de inicio (dashboard)
├── components/               # Componentes React reutilizables
│   ├── ui/                   # Componentes de UI base (Radix UI)
│   └── ...                   # Componentes de negocio
├── lib/                      # Utilidades y lógica de negocio
│   ├── db.ts                 # Funciones de base de datos
│   ├── mock-data.ts          # Datos mock para desarrollo
│   ├── types.ts              # Tipos de TypeScript
│   ├── format.ts             # Funciones de formateo
│   └── utils.ts              # Utilidades generales
├── scripts/                  # Scripts de utilidad
│   ├── 01-create-tables.sql  # Script de creación de tablas
│   ├── 02-seed-categories.sql # Script de datos iniciales
│   └── test-connection.js    # Script de prueba de conexión
├── public/                   # Archivos estáticos
├── styles/                   # Estilos globales
└── .github/                  # Configuración de GitHub
    └── workflows/            # Workflows de CI/CD
```

## 📚 Uso

### Gestión de Cuentas

Administra tus cuentas financieras:

```bash
# Crear una cuenta
curl -X POST http://localhost:3000/api/accounts \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Cuenta Principal",
    "type": "banco",
    "balance": 5000,
    "currency": "USD"
  }'

# Listar cuentas
curl http://localhost:3000/api/accounts

# Actualizar una cuenta
curl -X PATCH http://localhost:3000/api/accounts/1 \
  -H "Content-Type: application/json" \
  -d '{
    "balance": 6000
  }'

# Eliminar una cuenta
curl -X DELETE http://localhost:3000/api/accounts/1
```

### Gestión de Transacciones

Registra ingresos y gastos:

```bash
# Crear una transacción
curl -X POST http://localhost:3000/api/transactions \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": 1,
    "category_id": 6,
    "type": "gasto",
    "amount": 150.50,
    "description": "Compras de supermercado",
    "date": "2025-01-20"
  }'

# Listar transacciones con filtros
curl "http://localhost:3000/api/transactions?type=gasto&startDate=2025-01-01&endDate=2025-01-31"

# Actualizar una transacción
curl -X PATCH http://localhost:3000/api/transactions/1 \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 175.00
  }'
```

### Consultar Métricas

Obtén métricas del dashboard:

```bash
# Métricas generales
curl http://localhost:3000/api/dashboard/metrics

# Métricas con filtros de fecha
curl "http://localhost:3000/api/dashboard/metrics?startDate=2025-01-01&endDate=2025-01-31"
```

### Generar Reportes

Analiza tus gastos por categoría:

```bash
# Reporte de gastos por categoría
curl http://localhost:3000/api/reports/expenses-by-category

# Reporte con filtros de fecha
curl "http://localhost:3000/api/reports/expenses-by-category?startDate=2025-01-01&endDate=2025-01-31"
```

## 🗄️ Esquema de Base de Datos

### Tabla: categories

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | SERIAL PRIMARY KEY | Identificador único |
| name | VARCHAR(100) | Nombre de la categoría |
| category_type | VARCHAR(20) | Tipo: 'ingreso' o 'gasto' |
| icon | VARCHAR(50) | Nombre del ícono |
| color | VARCHAR(50) | Color en formato HSL |
| created_at | TIMESTAMP | Fecha de creación |

### Tabla: accounts

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | SERIAL PRIMARY KEY | Identificador único |
| name | VARCHAR(100) | Nombre de la cuenta |
| account_type | VARCHAR(20) | Tipo: 'efectivo', 'banco', 'tarjeta' |
| balance | DECIMAL(12,2) | Balance actual |
| currency | VARCHAR(3) | Código de moneda (USD, EUR, etc.) |
| is_archived | BOOLEAN | Estado de archivo |
| created_at | TIMESTAMP | Fecha de creación |
| updated_at | TIMESTAMP | Fecha de actualización |

### Tabla: transactions

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | SERIAL PRIMARY KEY | Identificador único |
| account_id | INTEGER | Referencia a cuenta |
| category_id | INTEGER | Referencia a categoría |
| transaction_type | VARCHAR(20) | Tipo: 'ingreso' o 'gasto' |
| amount | DECIMAL(12,2) | Monto de la transacción |
| description | TEXT | Descripción opcional |
| transaction_date | DATE | Fecha de la transacción |
| created_at | TIMESTAMP | Fecha de creación |
| updated_at | TIMESTAMP | Fecha de actualización |

## 🏗️ Scripts Disponibles

```bash
# Desarrollo
npm run dev          # Inicia el servidor de desarrollo

# Producción
npm run build        # Construye la aplicación para producción
npm run start        # Inicia el servidor de producción

# Calidad de código
npm run lint         # Ejecuta ESLint

# Scripts de base de datos
node scripts/test-connection.js  # Verifica la conexión a la base de datos
```

## 🚢 Despliegue

### Vercel (Recomendado)

1. Crea una cuenta en [Vercel](https://vercel.com)
2. Importa el repositorio desde GitHub
3. Configura las variables de entorno:
   - `DATABASE_URL` (opcional)
   - Otras variables según necesites
4. Despliega automáticamente

### Otros Servicios

La aplicación es compatible con cualquier plataforma que soporte Next.js:

- **Netlify**: Usa el plugin de Next.js
- **Railway**: Despliega directamente desde GitHub
- **Digital Ocean App Platform**: Soporta Next.js nativamente
- **AWS Amplify**: Compatible con Next.js SSR

### Variables de Entorno para Producción

```env
DATABASE_URL=postgresql://...           # Base de datos PostgreSQL
NODE_ENV=production                     # Modo de producción
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=...    # Analytics (opcional)
```

## 🛠️ Tecnologías Utilizadas

### Frontend
- **Next.js 14.2** - Framework de React con SSR
- **React 18** - Biblioteca de UI
- **TypeScript 5** - Tipado estático
- **Tailwind CSS 4** - Framework de estilos
- **Radix UI** - Componentes accesibles
- **Lucide React** - Iconos
- **Recharts** - Gráficos y visualizaciones
- **React Hook Form** - Manejo de formularios
- **Zod** - Validación de esquemas

### Backend
- **Next.js API Routes** - API REST
- **Neon Database** - PostgreSQL serverless
- **@neondatabase/serverless** - Cliente de base de datos

### Herramientas de Desarrollo
- **ESLint** - Linter de código
- **PostCSS** - Procesador de CSS
- **Vercel Analytics** - Análisis de rendimiento

## 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor, sigue estos pasos:

1. **Fork** el repositorio
2. **Crea una rama** para tu feature (`git checkout -b feature/AmazingFeature`)
3. **Commit** tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. **Push** a la rama (`git push origin feature/AmazingFeature`)
5. **Abre un Pull Request**

Por favor, lee [CONTRIBUTING.md](CONTRIBUTING.md) para más detalles sobre nuestro código de conducta y el proceso de contribución.

### Guías de Contribución

- Usa TypeScript para todo el código nuevo
- Sigue las convenciones de código existentes
- Escribe mensajes de commit descriptivos
- Asegúrate de que el código pase el linter: `npm run lint`
- Verifica que el build funcione: `npm run build`
- Documenta funciones y componentes complejos

## 📝 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

## 👥 Autores

- **Manuel Cardoso** - [@ManuhCardoso1501](https://github.com/ManuhCardoso1501)

## 🙏 Agradecimientos

- [Next.js](https://nextjs.org/) por el excelente framework
- [Vercel](https://vercel.com/) por el hosting y analytics
- [Radix UI](https://www.radix-ui.com/) por los componentes accesibles
- [Shadcn/ui](https://ui.shadcn.com/) por la inspiración en el diseño
- [Neon](https://neon.tech/) por la base de datos PostgreSQL serverless

## 📞 Soporte

Si tienes alguna pregunta o problema:

- 🐛 [Reporta un bug](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/issues/new?labels=bug)
- 💡 [Solicita una característica](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/issues/new?labels=enhancement)
- 📧 Contacta al autor a través de GitHub

---

**Hecho con ❤️ usando Next.js y TypeScript**
