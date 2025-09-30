# ⚡ Quick Start Guide

Una guía rápida para comenzar a trabajar con Finanzas Personales.

## 🚀 Inicio Rápido (5 minutos)

### Opción 1: Sin Base de Datos (Más Rápido)

```bash
# 1. Clonar el repositorio
git clone https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II.git
cd FinanzasPersonales-PyI-II

# 2. Instalar dependencias
npm install

# 3. Iniciar servidor de desarrollo
npm run dev

# 4. Abrir en navegador
# http://localhost:3000
```

¡Listo! La aplicación correrá con datos de ejemplo.

### Opción 2: Con Base de Datos

```bash
# 1-2. Igual que opción 1
git clone https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II.git
cd FinanzasPersonales-PyI-II
npm install

# 3. Configurar base de datos
echo 'DATABASE_URL=tu_postgresql_url' > .env.local

# 4. Crear tablas
psql $DATABASE_URL < scripts/01-create-tables.sql
psql $DATABASE_URL < scripts/02-seed-categories.sql

# 5. Verificar conexión
node scripts/test-connection.js

# 6. Iniciar
npm run dev
```

## 📝 Comandos Esenciales

```bash
# Desarrollo
npm run dev              # Iniciar servidor desarrollo (puerto 3000)
npm run build           # Compilar para producción
npm run start           # Iniciar servidor producción
npm run lint            # Verificar código con ESLint

# Base de datos
node scripts/test-connection.js  # Probar conexión BD

# Git
git add .               # Añadir cambios
git commit -m "mensaje" # Commit
git push               # Subir cambios
```

## 🗂️ Estructura Rápida

```
app/
├── api/              → API endpoints
├── page.tsx          → Dashboard (home)
├── cuentas/          → Página de cuentas
├── transacciones/    → Página de transacciones
└── reportes/         → Página de reportes

components/
├── ui/               → Componentes base (botones, inputs, etc.)
└── [feature]/        → Componentes por funcionalidad

lib/
├── db.ts             → Consultas base de datos
├── mock-data.ts      → Datos de ejemplo
├── types.ts          → Tipos TypeScript
└── utils.ts          → Funciones auxiliares
```

## 🔧 Configuración Común

### Variables de Entorno (.env.local)

```env
# Mínimo (para producción con BD)
DATABASE_URL=postgresql://user:pass@host:5432/db

# Opcional
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=your_id
NODE_ENV=development
```

### package.json - Scripts

```json
{
  "scripts": {
    "dev": "next dev",           // Desarrollo
    "build": "next build",       // Compilar
    "start": "next start",       // Producción
    "lint": "next lint"          // Linter
  }
}
```

## 🎯 Flujo de Trabajo Típico

### Añadir Nueva Funcionalidad

1. **Crear rama**
   ```bash
   git checkout -b feature/mi-nueva-funcion
   ```

2. **Hacer cambios**
   - Editar archivos necesarios
   - Seguir convenciones de código

3. **Probar localmente**
   ```bash
   npm run lint    # Verificar estilo
   npm run build   # Compilar
   npm run dev     # Probar en navegador
   ```

4. **Commit y push**
   ```bash
   git add .
   git commit -m "feat: añadir nueva función"
   git push origin feature/mi-nueva-funcion
   ```

5. **Crear Pull Request**
   - Ir a GitHub
   - Abrir PR desde tu rama

### Corregir un Bug

1. **Crear rama de fix**
   ```bash
   git checkout -b fix/descripcion-del-bug
   ```

2. **Hacer corrección**
   - Identificar y corregir el problema
   - Probar que funcione

3. **Commit y push**
   ```bash
   git add .
   git commit -m "fix: corregir [descripción]"
   git push origin fix/descripcion-del-bug
   ```

4. **Abrir PR**

## 🐛 Debugging Rápido

### La aplicación no inicia

```bash
# Limpiar y reinstalar
rm -rf node_modules .next
npm install
npm run dev
```

### Error de TypeScript

```bash
# Verificar errores
npx tsc --noEmit

# Ver detalles específicos
npm run build
```

### Base de datos no conecta

```bash
# Verificar variable
echo $DATABASE_URL

# Probar conexión
node scripts/test-connection.js

# Si falla, la app usará datos mock automáticamente
```

### ESLint falla

```bash
# Ver errores
npm run lint

# Auto-fix
npm run lint -- --fix
```

## 📚 Recursos Útiles

- [Documentación Completa](DOCUMENTATION.md)
- [Guía de Contribución](CONTRIBUTING.md)
- [README Principal](README.md)
- [Next.js Docs](https://nextjs.org/docs)
- [React Docs](https://react.dev)
- [TypeScript Docs](https://www.typescriptlang.org/docs)

## 💡 Tips Rápidos

1. **Hot Reload**: Los cambios se reflejan automáticamente en `npm run dev`
2. **Mock Data**: Sin `DATABASE_URL`, la app usa datos de ejemplo
3. **Tipos**: TypeScript ayuda a prevenir errores - úsalo
4. **Componentes**: Reutiliza componentes de `components/ui/`
5. **API**: Todos los endpoints están en `app/api/`
6. **Estilos**: Usa Tailwind CSS classes

## 🚦 Checklist Antes de Commit

- [ ] El código compila: `npm run build`
- [ ] Pasa el linter: `npm run lint`
- [ ] Funciona en el navegador
- [ ] Commit message descriptivo
- [ ] Cambios mínimos necesarios

## 🎓 Próximos Pasos

1. Lee el [README.md](README.md) completo
2. Explora la [DOCUMENTATION.md](DOCUMENTATION.md)
3. Revisa [CONTRIBUTING.md](CONTRIBUTING.md) si vas a contribuir
4. Experimenta con la aplicación
5. Revisa el código existente para entender patrones

## 🆘 Ayuda

¿Atascado? 

- 🐛 [Abrir un Issue](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/issues)
- 💬 [Iniciar una Discusión](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/discussions)
- 📖 Leer la documentación completa

---

**¡Feliz desarrollo! 🎉**
