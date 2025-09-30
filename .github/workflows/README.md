# 🚀 CI/CD Pipeline - Guía de Uso

Este directorio contiene el workflow de integración continua (CI/CD) del proyecto.

## 📋 Workflow: ci.yml

### ¿Qué hace?

El workflow automatiza la validación de código en cada push y pull request:

1. **Instala dependencias** del proyecto
2. **Ejecuta el linter** para verificar calidad de código
3. **Compila la aplicación** para detectar errores
4. **Audita seguridad** en dependencias

### ¿Cuándo se ejecuta?

- ✅ Cuando haces `git push` a las ramas `main` o `develop`
- ✅ Cuando abres o actualizas un Pull Request a `main` o `develop`

### Jobs Configurados

#### 1. Build and Test 🔨

Matrix testing en Node.js 18.x y 20.x

```yaml
- Checkout code
- Setup Node.js (18.x o 20.x)
- Install dependencies (npm ci)
- Run lint
- Build application
- Check build output
```

**Duración aproximada**: 2-3 minutos

#### 2. Code Quality 🔍

Verificación de calidad de código

```yaml
- Checkout code
- Setup Node.js 20.x
- Install dependencies
- Run ESLint
- Check TypeScript types
```

**Duración aproximada**: 1-2 minutos

#### 3. Security 🔒

Auditoría de seguridad

```yaml
- Checkout code
- Setup Node.js 20.x
- Install dependencies
- Run security audit
- Check outdated packages
```

**Duración aproximada**: 1 minuto

## 🎯 Ver Resultados

### En GitHub

1. Ve a tu repositorio en GitHub
2. Click en la pestaña **Actions**
3. Verás la lista de workflows ejecutados
4. Click en cualquier workflow para ver detalles

### En Pull Requests

Los resultados aparecen automáticamente:

- ✅ **Verde**: Todo pasó correctamente
- ❌ **Rojo**: Hay errores que corregir
- 🟡 **Amarillo**: El workflow está en progreso

### Badge en README

El estado del workflow se muestra en el README:

```markdown
[![Build Status](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/workflows/CI/badge.svg)](...)
```

## 🔧 Ejecutar Localmente

Puedes simular el workflow en tu máquina local:

```bash
# Instalar como en CI
npm ci

# Ejecutar linter
npm run lint

# Compilar
npm run build

# Auditoría de seguridad
npm audit --audit-level=high
```

## 📝 Modificar el Workflow

### Añadir más versiones de Node.js

Edita la matriz en `ci.yml`:

```yaml
strategy:
  matrix:
    node-version: [18.x, 20.x, 21.x]  # Añadir 21.x
```

### Añadir tests

Si añades tests al proyecto:

```yaml
- name: Run tests
  run: npm test
```

### Cambiar branches trigger

Para ejecutar en otras ramas:

```yaml
on:
  push:
    branches: [ main, develop, staging ]  # Añadir staging
```

## 🐛 Troubleshooting

### El workflow falla en "Install dependencies"

**Problema**: Conflicto en package-lock.json

**Solución**:
```bash
rm package-lock.json
npm install
git add package-lock.json
git commit -m "fix: update package-lock.json"
```

### El workflow falla en "Run lint"

**Problema**: Errores de ESLint

**Solución**:
```bash
npm run lint -- --fix  # Auto-fix de errores
# O corregir manualmente
```

### El workflow falla en "Build application"

**Problema**: Errores de TypeScript o compilación

**Solución**:
```bash
npm run build  # Ver errores localmente
# Corregir errores mostrados
```

### El workflow es muy lento

**Optimizaciones posibles**:

1. Usar caché de npm (ya configurado)
2. Reducir versiones en matrix
3. Ejecutar jobs en paralelo (ya configurado)

## 📚 Recursos

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Next.js CI/CD Best Practices](https://nextjs.org/docs/deployment#continuous-integration-ci)

## 🔄 Mantenimiento

### Actualizar versiones de Actions

Periódicamente actualiza las versiones:

```yaml
# De:
uses: actions/checkout@v4
uses: actions/setup-node@v4

# A (cuando hay nueva versión):
uses: actions/checkout@v5
uses: actions/setup-node@v5
```

### Revisar logs

Revisa los logs regularmente para:
- Detectar warnings que puedan convertirse en errores
- Identificar paquetes desactualizados
- Encontrar vulnerabilidades de seguridad

## ✅ Checklist: Antes de Merge

Antes de hacer merge de un PR, verifica que:

- [ ] ✅ Todos los jobs pasaron exitosamente
- [ ] 📝 No hay warnings críticos
- [ ] 🔒 No hay vulnerabilidades de alta severidad
- [ ] 📊 El código está revisado
- [ ] 🧪 Has probado localmente

## 🎓 Aprende Más

Para aprender más sobre CI/CD:

1. Lee la [DOCUMENTATION.md](../DOCUMENTATION.md)
2. Revisa [CONTRIBUTING.md](../CONTRIBUTING.md)
3. Experimenta con el workflow
4. Revisa otros proyectos open-source

---

**Última actualización**: Enero 2025  
**Mantenedor**: [@ManuhCardoso1501](https://github.com/ManuhCardoso1501)
