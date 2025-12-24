# 🎨 Wireframes Interactivos - Análisis de Flujos de Usuario

> **Prototipos de baja fidelidad** para visualizar y documentar flujos de usuario en sistemas web. Proyecto educativo enfocado en UX/UI y análisis de requerimientos funcionales.

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/es/docs/Web/HTML)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)](https://developer.mozilla.org/es/docs/Web/JavaScript)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Características](#-características)
- [Casos de Uso](#-casos-de-uso)
- [Demostración](#-demostración)
- [Instalación](#-instalación)
- [Uso](#-uso)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Metodología de Diseño](#-metodología-de-diseño)
- [Tecnologías](#-tecnologías)
- [Contribuir](#-contribuir)
- [Roadmap](#-roadmap)
- [Licencia](#-licencia)
- [Contacto](#-contacto)

---

## 📖 Descripción

Este proyecto presenta **wireframes interactivos** para tres flujos de usuario críticos en aplicaciones web modernas. Cada flujo está documentado con **6 pasos detallados** que incluyen:

- ✅ Validaciones en tiempo real
- ⚠️ Manejo de errores y retroalimentación
- 🔀 Decisiones clave y ramificaciones
- 🎯 Mejores prácticas de UX/UI

El objetivo es proporcionar una **guía visual y funcional** que pueda ser utilizada por diseñadores, desarrolladores y product managers para entender la lógica de negocio y la experiencia del usuario antes de la implementación.

---

## ✨ Características

### 🖼️ Visualización Interactiva
- Navegación entre 3 casos de uso diferentes
- 6 pasos por cada flujo con transiciones suaves
- Interfaz tipo browser con header simulado
- Diseño responsive y adaptable

### 📝 Documentación Detallada
- Descripción técnica de cada paso
- Validaciones y reglas de negocio
- Mensajes de error contextuales
- Indicadores visuales de progreso

### 🎨 Diseño Clean
- Wireframes minimalistas (baja fidelidad)
- Sistema de colores consistente
- Tipografía legible (Inter font)
- Componentes reutilizables

### ⚡ Zero Dependencies
- Sin frameworks pesados
- Vanilla JavaScript puro
- Tailwind CSS via CDN
- Carga instantánea

---

## 🎯 Casos de Uso

### **Opción A: Solicitud de Cita Médica en Línea** 🏥

Sistema de agendamiento médico que permite a los usuarios:
- Autenticarse de forma segura
- Buscar y filtrar especialidades médicas
- Seleccionar médico, fecha y horario
- Ingresar motivo de consulta
- Confirmar cita y recibir comprobante

**Complejidad**: Media | **Pasos**: 6 | **Validaciones**: Alta

---

### **Opción B: Actualización de Dirección de Envío** 📍

Gestión de direcciones para e-commerce con:
- Múltiples direcciones por usuario (hasta 10)
- Validación de código postal automática
- Geolocalización en mapa interactivo
- Comparativa antes/después de cambios
- Impacto en pedidos activos

**Complejidad**: Media-Alta | **Pasos**: 6 | **Validaciones**: Alta

---

### **Opción C: Generación de Factura de Compra** 🧾

Facturación electrónica (CFDI 4.0 - México) que incluye:
- Selección de pedidos facturables
- Validación de RFC con el SAT
- Selección de uso de CFDI
- Desglose de impuestos (IVA, IEPS)
- Timbrado y descarga (XML + PDF)

**Complejidad**: Alta | **Pasos**: 6 | **Validaciones**: Crítica

---

## 🎬 Demostración

### Vista Previa

```
┌─────────────────────────────────────────┐
│  ●  ●  ●     [URL Bar]                  │
├─────────────────────────────────────────┤
│                                         │
│   [Wireframe del paso actual]           │
│                                         │
│   • Elementos interactivos              │
│   • Validaciones visuales               │
│   • Mensajes de error                   │
│                                         │
└─────────────────────────────────────────┘
   [← Anterior]  Paso X/6  [Siguiente →]
```

### Capturas de Pantalla

**Paso 1 - Autenticación (Caso A)**
```
┌──────────────────┐
│    [Logo]        │
│                  │
│  Email: ____     │
│  Password: ____  │
│                  │
│  [INICIAR]       │
└──────────────────┘
```

**Paso 4 - Validación Mapa (Caso B)**
```
┌──────────────────┐
│  [MAPA]          │
│     📍           │
│  Confirmar       │
│  ubicación       │
└──────────────────┘
```

---

## 🚀 Instalación

### Opción 1: Clonar Repositorio

```bash
# Clonar el proyecto
git clone https://github.com/BENMP0902/ux-wireframes-interactive.git

# Navegar al directorio
cd ux-wireframes-interactive

# Abrir en el navegador
open index.html
# O en Windows:
start index.html
# O en Linux:
xdg-open index.html
```

### Opción 2: Descarga Directa

1. Ve a [Releases](https://github.com/BENMP0902/ux-wireframes-interactive/releases)
2. Descarga la última versión
3. Descomprime el archivo
4. Abre `index.html` en tu navegador

### Opción 3: GitHub Pages

Visita la demo en vivo: **[https://BENMP0902.github.io/ux-wireframes-interactive](https://BENMP0902.github.io/ux-wireframes-interactive)**

---

## 💻 Uso

### Navegación Básica

```javascript
// Cambiar entre casos de uso
changeCase('A') // Citas médicas
changeCase('B') // Direcciones
changeCase('C') // Facturas

// Navegar entre pasos
nextStep()      // Siguiente paso
prevStep()      // Paso anterior
goToStep(3)     // Ir al paso 3
```

### Estructura de Datos

Cada caso de uso sigue esta estructura:

```javascript
{
  title: "Nombre del Flujo",
  steps: [
    {
      title: "Nombre del Paso",
      desc: "Descripción técnica detallada",
      render: `<!-- HTML del wireframe -->`
    },
    // ... 6 pasos en total
  ]
}
```

### Personalización

Para agregar un nuevo caso de uso:

```javascript
data['D'] = {
  title: "Mi Nuevo Flujo",
  steps: [
    {
      title: "Paso 1",
      desc: "Descripción del paso",
      render: `
        <div class="tu-wireframe-aqui">
          <!-- Contenido -->
        </div>
      `
    },
    // ... más pasos
  ]
};
```

---

## 📁 Estructura del Proyecto

```
ux-wireframes-interactive/
│
├── index.html              # Archivo principal (todo-en-uno)
│
├── README.md               # Este archivo
│
├── LICENSE                 # Licencia MIT
│
├── docs/                   # Documentación adicional(futuro)
│   ├── FLOWS.md           # Análisis detallado de flujos
│   ├── VALIDATIONS.md     # Reglas de validación
│   └── ERRORS.md          # Catálogo de errores
│
├── assets/                 # Recursos (futuro)
│   ├── screenshots/       # Capturas de pantalla
│   └── diagrams/          # Diagramas de flujo
│
└── examples/               # Ejemplos adicionales(futuro)
    ├── variant-a.html
    ├── variant-b.html
    └── variant-c.html
```

---

## 🎨 Metodología de Diseño

### Principios Aplicados

1. **Claridad sobre Creatividad**: Wireframes de baja fidelidad enfocados en funcionalidad
2. **Feedback Inmediato**: Validaciones y errores visibles en cada paso
3. **Progreso Visible**: Indicadores claros de dónde está el usuario
4. **Recuperación de Errores**: Siempre hay una salida clara ante problemas
5. **Consistencia**: Patrones de diseño repetidos en los 3 casos

### Niveles de Validación

| Nivel | Momento | Ejemplo |
|-------|---------|---------|
| **Cliente** | Tiempo real | Formato de email, longitud de campos |
| **Visual** | Al perder foco | Resaltado de campos incorrectos |
| **Servidor** | Al enviar | RFC válido ante el SAT |
| **Negocio** | Post-proceso | Disponibilidad de horarios médicos |

### Convenciones de Color

```css
/* Sistema de colores semántico */
Azul (#2563EB)    → Acción principal / Seleccionado
Verde (#10B981)   → Éxito / Confirmación
Rojo (#EF4444)    → Error / Atención requerida
Amarillo (#F59E0B) → Advertencia / Información
Gris (#6B7280)    → Neutral / Deshabilitado
```

---

## 🛠️ Tecnologías

### Core
- **HTML5**: Estructura semántica
- **CSS3**: Estilos personalizados
- **JavaScript (ES6+)**: Lógica de navegación y renderizado

### Frameworks/Librerías
- **Tailwind CSS 3.x**: Sistema de diseño utility-first
- **Google Fonts**: Tipografía Inter

### Herramientas de Desarrollo
- **Git**: Control de versiones
- **GitHub Pages**: Hosting estático
- **VS Code**: Editor recomendado

### Compatibilidad

| Navegador | Versión Mínima |
|-----------|----------------|
| Chrome    | 90+            |
| Firefox   | 88+            |
| Safari    | 14+            |
| Edge      | 90+            |

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Sigue estos pasos:

### 1. Fork del Proyecto

```bash
# Desde GitHub, haz clic en "Fork"
```

### 2. Crear Rama de Feature

```bash
git checkout -b feature/nueva-funcionalidad
```

### 3. Commit de Cambios

```bash
git commit -m "✨ Agrega nueva funcionalidad X"
```

Usa conventional commits:
- `✨ feat:` Nueva funcionalidad
- `🐛 fix:` Corrección de bug
- `📝 docs:` Documentación
- `💄 style:` Cambios de estilo
- `♻️ refactor:` Refactorización

### 4. Push a la Rama

```bash
git push origin feature/nueva-funcionalidad
```

### 5. Abrir Pull Request

Ve a GitHub y crea un Pull Request con:
- Descripción clara del cambio
- Screenshots (si aplica)
- Referencia al issue relacionado

### Guías de Contribución

- Mantén los wireframes simples (baja fidelidad)
- Documenta cada paso con descripción técnica
- Incluye manejo de errores en cada flujo
- Sigue las convenciones de código existentes
- Prueba en múltiples navegadores

---

## 🗺️ Roadmap

### Versión 1.0 (Actual)
- [x] 3 casos de uso completos
- [x] Navegación entre pasos
- [x] Wireframes de baja fidelidad
- [x] Documentación básica

### Versión 1.1 (En progreso)
- [ ] Modo oscuro
- [ ] Exportar flujos a PDF
- [ ] Anotaciones editables
- [ ] Historial de navegación

### Versión 2.0 (Futuro)
- [ ] Editor de wireframes integrado
- [ ] Colaboración en tiempo real
- [ ] Integración con Figma
- [ ] Sistema de comentarios
- [ ] Versionado de flujos
- [ ] API REST para integración

### Mejoras Propuestas
- [ ] Casos de uso adicionales (checkout, registro, etc.)
- [ ] Animaciones de transición suaves
- [ ] Temas personalizables
- [ ] Soporte para mobile-first
- [ ] Accesibilidad (WCAG 2.1 AA)

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

```
MIT License

Copyright (c) 2025 [Tu Nombre]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---

## 🙏 Agradecimientos

- [Tailwind CSS](https://tailwindcss.com/) por el excelente framework CSS
- [Google Fonts](https://fonts.google.com/) por la tipografía Inter
- [Heroicons](https://heroicons.com/) por los íconos SVG
- Comunidad de UX/UI en GitHub por la inspiración
- [Laws of UX](https://lawsofux.com/) por los principios de diseño

---

## 📚 Recursos Adicionales

### Documentación Relacionada(futuro)
- [Análisis Completo de Flujos](docs/FLOWS.md) - Desglose paso a paso
- [Reglas de Validación](docs/VALIDATIONS.md) - Todas las validaciones
- [Catálogo de Errores](docs/ERRORS.md) - Mensajes y soluciones

### Referencias Externas
- [Nielsen Norman Group](https://www.nngroup.com/) - Investigación UX
- [Material Design](https://material.io/design) - Guías de diseño
- [UX Collective](https://uxdesign.cc/) - Artículos y casos de estudio

---

## ⭐ Apóyanos

Si este proyecto te resulta útil:

1. Dale una ⭐ en GitHub
2. Compártelo en redes sociales
3. Considera [comprarme un café](https://ko-fi.com/BENMP0902) ☕
4. Contribuye con mejoras

---

<div align="center">

**Hecho con ❤️ y mucho ☕ por [BENJAMIN :)]**

[⬆ Volver arriba](#-wireframes-interactivos---análisis-de-flujos-de-usuario)

</div>