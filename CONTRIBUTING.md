# Guía de Contribución

¡Gracias por tu interés en contribuir a Finanzas Personales! Este documento proporciona guías y mejores prácticas para contribuir al proyecto.

## 📋 Tabla de Contenidos

- [Código de Conducta](#código-de-conducta)
- [¿Cómo Puedo Contribuir?](#cómo-puedo-contribuir)
- [Configuración del Entorno](#configuración-del-entorno)
- [Proceso de Desarrollo](#proceso-de-desarrollo)
- [Guías de Estilo](#guías-de-estilo)
- [Proceso de Pull Request](#proceso-de-pull-request)
- [Reportar Bugs](#reportar-bugs)
- [Solicitar Características](#solicitar-características)

## 📜 Código de Conducta

Este proyecto se adhiere a un Código de Conducta. Al participar, se espera que mantengas este código. Por favor reporta comportamiento inaceptable a través de los issues de GitHub.

### Nuestras Promesas

- Usar un lenguaje acogedor e inclusivo
- Ser respetuoso con diferentes puntos de vista y experiencias
- Aceptar críticas constructivas de manera profesional
- Enfocarse en lo que es mejor para la comunidad
- Mostrar empatía hacia otros miembros de la comunidad

## 🤝 ¿Cómo Puedo Contribuir?

### Tipos de Contribuciones

1. **Reportar Bugs**: Ayuda a identificar problemas
2. **Sugerir Mejoras**: Propón nuevas características o mejoras
3. **Escribir Código**: Implementa nuevas funcionalidades o corrige bugs
4. **Mejorar Documentación**: Actualiza o mejora la documentación
5. **Revisar Pull Requests**: Ayuda a revisar código de otros contribuyentes
6. **Traducir**: Ayuda a internacionalizar la aplicación

## 🔧 Configuración del Entorno

### Prerrequisitos

- Node.js 18.x o superior
- npm 9.x o superior
- Git
- Un editor de código (recomendamos VS Code)
- PostgreSQL (opcional, para desarrollo con base de datos)

### Instalación

1. **Fork el repositorio**
   
   Haz click en el botón "Fork" en la página principal del repositorio.

2. **Clona tu fork**

   ```bash
   git clone https://github.com/TU-USUARIO/FinanzasPersonales-PyI-II.git
   cd FinanzasPersonales-PyI-II
   ```

3. **Añade el repositorio original como upstream**

   ```bash
   git remote add upstream https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II.git
   ```

4. **Instala las dependencias**

   ```bash
   npm install
   ```

5. **Crea el archivo de configuración**

   ```bash
   cp .env.example .env.local  # Si existe
   # O crea un nuevo .env.local con las variables necesarias
   ```

6. **Inicia el servidor de desarrollo**

   ```bash
   npm run dev
   ```

### Configuración Recomendada de VS Code

Extensiones recomendadas:
- ESLint
- Prettier
- TypeScript and JavaScript Language Features
- Tailwind CSS IntelliSense
- GitLens

## 🔄 Proceso de Desarrollo

### 1. Crea una Rama

Antes de comenzar a trabajar, crea una rama descriptiva:

```bash
git checkout -b tipo/descripcion-corta
```

Tipos de rama:
- `feature/` - Nueva funcionalidad
- `fix/` - Corrección de bug
- `docs/` - Cambios en documentación
- `refactor/` - Refactorización de código
- `test/` - Añadir o actualizar tests
- `style/` - Cambios de estilo (formato, espacios, etc.)

Ejemplos:
```bash
git checkout -b feature/añadir-modo-oscuro
git checkout -b fix/corregir-calculo-balance
git checkout -b docs/actualizar-readme
```

### 2. Realiza tus Cambios

- Escribe código limpio y legible
- Sigue las convenciones de estilo del proyecto
- Comenta código complejo cuando sea necesario
- Actualiza la documentación si es relevante

### 3. Prueba tus Cambios

```bash
# Verifica que el código compile
npm run build

# Ejecuta el linter
npm run lint

# Prueba manualmente en el navegador
npm run dev
```

### 4. Commit tus Cambios

Usa mensajes de commit descriptivos siguiendo el formato:

```
tipo(ámbito): descripción breve

Descripción más detallada si es necesario.

Fixes #123
```

Tipos de commit:
- `feat`: Nueva funcionalidad
- `fix`: Corrección de bug
- `docs`: Cambios en documentación
- `style`: Cambios de formato (no afectan el código)
- `refactor`: Refactorización de código
- `test`: Añadir o actualizar tests
- `chore`: Tareas de mantenimiento

Ejemplos:
```bash
git commit -m "feat(dashboard): añadir gráfico de gastos mensuales"
git commit -m "fix(transactions): corregir cálculo de balance"
git commit -m "docs(readme): actualizar instrucciones de instalación"
```

### 5. Mantén tu Rama Actualizada

```bash
git fetch upstream
git rebase upstream/main
```

### 6. Push a tu Fork

```bash
git push origin tipo/descripcion-corta
```

## 🎨 Guías de Estilo

### TypeScript

- Usa TypeScript para todo el código nuevo
- Define tipos explícitos para props y estados
- Evita usar `any` - usa `unknown` si es necesario
- Usa interfaces para objetos y types para uniones/intersecciones

```typescript
// ✅ Bien
interface UserProps {
  name: string
  email: string
  age?: number
}

// ❌ Mal
const user: any = { name: "John" }
```

### React

- Usa componentes funcionales con hooks
- Mantén componentes pequeños y reutilizables
- Usa nombres descriptivos para componentes y props
- Extrae lógica compleja a custom hooks

```typescript
// ✅ Bien
export function TransactionList({ transactions }: TransactionListProps) {
  return (
    <div>
      {transactions.map(transaction => (
        <TransactionItem key={transaction.id} transaction={transaction} />
      ))}
    </div>
  )
}

// ❌ Mal
export function TL({ data }: any) {
  // ...
}
```

### Naming Conventions

- **Componentes**: PascalCase (`TransactionList`)
- **Archivos de componentes**: PascalCase (`TransactionList.tsx`)
- **Hooks**: camelCase con prefijo `use` (`useTransactions`)
- **Funciones**: camelCase (`calculateBalance`)
- **Constantes**: SCREAMING_SNAKE_CASE (`MAX_TRANSACTIONS`)
- **Variables**: camelCase (`totalAmount`)

### Estructura de Archivos

```typescript
// Orden de imports
import React from 'react'                    // React
import { useState } from 'react'             // React hooks
import type { NextPage } from 'next'         // Next.js

import { Button } from '@/components/ui'     // Componentes locales
import { formatCurrency } from '@/lib/utils' // Utilidades locales

import type { Transaction } from '@/lib/types' // Tipos locales

// Orden del contenido del componente
// 1. Tipos e interfaces
// 2. Definición del componente
// 3. Hooks
// 4. Handlers
// 5. Render
```

### CSS y Estilos

- Usa Tailwind CSS para estilos
- Prefiere utility classes sobre CSS custom
- Usa el sistema de design de Radix UI
- Mantén consistencia con los colores y espaciado existentes

```tsx
// ✅ Bien
<div className="flex items-center gap-4 p-4 rounded-lg bg-card">
  <Button variant="default" size="sm">
    Save
  </Button>
</div>

// ❌ Mal
<div style={{ display: 'flex', padding: '16px' }}>
  <button className="my-custom-button">Save</button>
</div>
```

### Comentarios

- Escribe comentarios solo cuando el código no sea auto-explicativo
- Usa comentarios JSDoc para funciones públicas
- Mantén comentarios actualizados con el código

```typescript
/**
 * Calcula el balance total sumando todos los ingresos y restando los gastos
 * @param transactions - Array de transacciones a procesar
 * @param startDate - Fecha de inicio del período (opcional)
 * @param endDate - Fecha de fin del período (opcional)
 * @returns El balance total calculado
 */
export function calculateBalance(
  transactions: Transaction[],
  startDate?: string,
  endDate?: string
): number {
  // Implementación
}
```

## 🔀 Proceso de Pull Request

### Antes de Crear el PR

1. ✅ El código compila sin errores: `npm run build`
2. ✅ El linter pasa: `npm run lint`
3. ✅ Has probado tus cambios manualmente
4. ✅ La rama está actualizada con `main`
5. ✅ Los commits siguen las convenciones
6. ✅ La documentación está actualizada

### Crear el Pull Request

1. **Título Descriptivo**
   ```
   feat: Añadir gráfico de gastos mensuales al dashboard
   fix: Corregir cálculo de balance en transacciones
   docs: Actualizar documentación de API
   ```

2. **Descripción Completa**

   Usa esta plantilla:

   ```markdown
   ## Descripción
   Breve descripción de los cambios realizados.

   ## Tipo de Cambio
   - [ ] Bug fix (cambio que corrige un problema)
   - [ ] Nueva funcionalidad (cambio que añade funcionalidad)
   - [ ] Breaking change (cambio que rompe compatibilidad)
   - [ ] Documentación (cambios solo en documentación)

   ## ¿Cómo se ha probado?
   Describe las pruebas realizadas.

   ## Checklist
   - [ ] Mi código sigue las guías de estilo del proyecto
   - [ ] He realizado una auto-revisión de mi código
   - [ ] He comentado el código en áreas complejas
   - [ ] He actualizado la documentación correspondiente
   - [ ] Mis cambios no generan nuevos warnings
   - [ ] El build pasa exitosamente
   - [ ] El linter pasa exitosamente

   ## Screenshots (si aplica)
   Añade capturas de pantalla para cambios visuales.

   ## Contexto Adicional
   Información adicional relevante.
   ```

3. **Asigna Revisores** (si tienes permisos)

4. **Añade Labels** apropiados

### Durante la Revisión

- Responde a los comentarios de manera profesional
- Realiza cambios solicitados en commits separados
- No hagas force push después de que la revisión haya comenzado
- Solicita re-revisión después de hacer cambios

### Después de la Aprobación

- El maintainer hará merge del PR
- Puedes eliminar tu rama después del merge
- Actualiza tu fork con los últimos cambios

## 🐛 Reportar Bugs

### Antes de Reportar

1. Verifica que estás usando la última versión
2. Busca en issues existentes para evitar duplicados
3. Intenta reproducir el bug de manera consistente

### Crear un Reporte de Bug

Usa esta plantilla:

```markdown
## Descripción del Bug
Una descripción clara y concisa del bug.

## Pasos para Reproducir
1. Ve a '...'
2. Haz click en '...'
3. Scroll hasta '...'
4. Observa el error

## Comportamiento Esperado
Descripción clara de lo que esperabas que sucediera.

## Comportamiento Actual
Descripción de lo que realmente sucede.

## Screenshots
Si aplica, añade screenshots para ayudar a explicar el problema.

## Entorno
- OS: [e.g., Windows 10, macOS 13, Ubuntu 22.04]
- Browser: [e.g., Chrome 120, Firefox 121, Safari 17]
- Node Version: [e.g., 18.17.0]
- Versión del Proyecto: [e.g., 0.1.0]

## Información Adicional
Cualquier otra información relevante sobre el problema.

## Posible Solución (opcional)
Si tienes ideas sobre cómo solucionarlo.
```

## 💡 Solicitar Características

### Antes de Solicitar

1. Verifica que la característica no exista ya
2. Busca en issues para ver si alguien ya la solicitó
3. Considera si es apropiada para el proyecto

### Crear una Solicitud de Característica

Usa esta plantilla:

```markdown
## ¿Tu solicitud está relacionada con un problema?
Una descripción clara del problema. Ej: "Siempre me frustra cuando..."

## Describe la Solución que te Gustaría
Una descripción clara y concisa de lo que quieres que suceda.

## Describe Alternativas que has Considerado
Una descripción de soluciones o características alternativas que has considerado.

## Contexto Adicional
Cualquier otro contexto o screenshots sobre la solicitud de característica.

## Beneficios
Explica cómo esta característica beneficiará a los usuarios.

## Implementación Propuesta (opcional)
Si tienes ideas sobre cómo implementarlo.
```

## 📞 Contacto

Si tienes preguntas sobre cómo contribuir:

- 💬 Abre un [Discussion](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/discussions)
- 📧 Contacta a los maintainers a través de GitHub
- 🐛 Revisa los [Issues](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/issues) existentes

## 🙏 Reconocimientos

¡Gracias por contribuir a Finanzas Personales! Cada contribución, sin importar cuán pequeña sea, es valiosa y apreciada.

---

**¿Listo para contribuir? [Crea tu primer Pull Request](https://github.com/ManuhCardoso1501/FinanzasPersonales-PyI-II/pulls)** 🚀
