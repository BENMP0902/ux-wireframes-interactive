# 📋 FLOWS.md - Documentación Técnica de Flujos de Usuario

> Especificación completa de los tres flujos implementados en el proyecto de wireframes interactivos.

---

## 📚 Índice

1. [Introducción](#introducción)
2. [Opción A: Solicitud de Cita Médica](#opción-a-solicitud-de-cita-médica)
3. [Opción B: Actualización de Dirección](#opción-b-actualización-de-dirección)
4. [Opción C: Generación de Factura](#opción-c-generación-de-factura)
5. [Matriz de Validaciones](#matriz-de-validaciones)
6. [Patrones de Diseño](#patrones-de-diseño)

---

## 🎯 Introducción

Este documento detalla la lógica de negocio, validaciones y flujos de error para los tres casos de uso implementados. Cada flujo contiene **6 pasos** documentados exhaustivamente.

### Propósito del Documento

- ✅ Guía de implementación para desarrolladores
- ✅ Referencia de validaciones y reglas de negocio
- ✅ Catálogo de mensajes de error y retroalimentación
- ✅ Documentación de decisiones de flujo

### Convenciones

| Elemento | Descripción |
|----------|-------------|
| **Campo obligatorio** | Marcado con `*` |
| `código` | Nombres de variables/funciones |
| [Botón] | Elementos interactivos |
| → | Flujo de navegación |

---

## 🏥 OPCIÓN A: Solicitud de Cita Médica

### Resumen del Flujo

**Objetivo**: Permitir al usuario agendar una cita médica de forma rápida y sin ambigüedades.

**Pasos**: 6  
**Tiempo estimado**: 3-5 minutos  
**Complejidad**: Media

**Diagrama de flujo simplificado:**
```
Login → Especialidad → Médico/Fecha → Horario → Motivo → Confirmación
```

---

### 📍 Paso 1: Autenticación

**Campos**:
- Email/Usuario *
- Contraseña *
- [Checkbox] Recordar sesión

**Validaciones**:
- Email: formato válido (@dominio.extensión)
- Contraseña: mínimo 8 caracteres
- Límite: 3 intentos fallidos → bloqueo 15 min

**Errores**:
- "Usuario o contraseña incorrectos. Te quedan X intentos"
- "Tu cuenta ha sido temporalmente bloqueada por seguridad. Intenta en 15 minutos"
- "Este campo es obligatorio"

**Decisión**: ¿Usuario autenticado? → Sí: Paso 2 | No: Registro

---

### 📍 Paso 2: Selección de Especialidad

**Elementos**:
- Buscador con autocompletado
- Lista categorizada de especialidades
- Filtros: "Solo disponibles hoy", "Alfabético"

**Validaciones**:
- Selección única (1 especialidad)
- Verificar disponibilidad en 30 días
- Búsqueda mínimo 2 caracteres

**Errores**:
- "Lo sentimos, no hay citas disponibles para Cardiología en los próximos 30 días. Puedes agregarte a la lista de espera"
- "No encontramos esa especialidad. ¿Quisiste decir...?"
- "No pudimos cargar las especialidades. Verifica tu conexión" + [Reintentar]

**Decisión**: ¿Disponibilidad? → Sí: Paso 3 | No: Lista de espera

---

### 📍 Paso 3: Médico y Fecha

**Elementos**:
- Tarjetas de médicos (foto, nombre, rating, reseñas)
- Opción "Primer médico disponible"
- Calendario 30 días con código de colores:
  - 🟢 Verde: 5+ horarios
  - 🟡 Amarillo: 2-4 horarios
  - 🔴 Rojo: 1 horario
  - ⚪ Gris: Sin disponibilidad

**Validaciones**:
- Solo fechas futuras
- Máximo 30 días desde hoy
- Si médico específico → filtrar calendario

**Errores**:
- "No puedes seleccionar fechas pasadas. Elige desde hoy en adelante"
- "Dr. García no tiene citas disponibles en los próximos 30 días. ¿Deseas ver otros médicos?"
- "No pudimos cargar las fechas disponibles" + [Reintentar]

**Decisión**: Médico primero → Filtra calendario | Fecha primero → Muestra médicos disponibles

---

### 📍 Paso 4: Selección de Horario

**Elementos**:
- Horarios agrupados (Mañana/Tarde/Noche)
- Cada horario: hora, duración, médico, ubicación
- Indicador "Más solicitado"
- Tipo: Presencial / Telemedicina

**Validaciones**:
- Selección única
- Verificación en tiempo real
- Bloqueo temporal 5 minutos

**Errores**:
- "Este horario acaba de ser reservado por otro usuario. Por favor elige otro"
- "Todos los horarios se han ocupado para esta fecha. ¿Deseas elegir otra fecha?"
- "Tu sesión ha expirado por inactividad. Inicia sesión nuevamente para continuar"

**Decisión**: ¿Disponible? → Sí: Bloqueo + Paso 5 | No: Seleccionar otro

---

### 📍 Paso 5: Motivo de Consulta

**Elementos**:
- Resumen no editable (con [Modificar])
- Textarea: Motivo * (10-500 caracteres)
- Contador de caracteres en tiempo real
- [Checkbox] ¿Primera vez con el médico?
- [Checkbox] Métodos de contacto (SMS/Email/WhatsApp)
- Info: Políticas de cancelación

**Validaciones**:
- Motivo: mín 10, máx 500 caracteres
- Sin caracteres especiales: <, >, {, }
- Detección de keywords de emergencia
- Al menos 1 método de contacto

**Errores**:
- "Por favor describe con más detalle (mínimo 10 caracteres). Esto ayudará al médico a preparar mejor tu consulta"
- "No se permiten caracteres especiales como <, >, {, }"
- "⚠️ Detectamos que mencionas una emergencia. Si requieres atención inmediata, llama al 911" + [Llamar] + [Continuar]

**Decisión**: ¿Emergencia? → Sí: Banner + opciones | No: Paso 6

---

### 📍 Paso 6: Confirmación

**Elementos**:
- Número de cita: #CIT-YYYYMMDD-####
- Resumen completo (especialidad, médico, fecha, hora, ubicación, motivo)
- Costo y método de pago
- [Checkbox obligatorio] Acepto políticas *
- [Confirmar cita] [Modificar] [Cancelar]
- Opciones post-confirmación: Agregar a calendario

**Validaciones**:
- Checkbox políticas debe estar marcado
- Verificación final de disponibilidad
- Bloqueo temporal no expirado

**Errores**:
- "El tiempo para completar tu reserva ha expirado (5 min). El horario se liberó. ¿Deseas intentar reservarlo nuevamente?"
- "Lo sentimos, otro usuario reservó este horario mientras completabas el proceso. Por favor selecciona otro horario"
- "Debes aceptar las políticas de cancelación para continuar"

**Flujo exitoso**:
1. Guarda en BD
2. Pantalla éxito: "✓ ¡Tu cita ha sido confirmada!"
3. Envío email PDF
4. Envío SMS/WhatsApp
5. Opción descargar PDF
6. [Agregar a calendario]
7. [Ver todas mis citas]

**Post-acción automática**:
- Recordatorios: 24h antes + 2h antes
- Libera bloqueo temporal

---

## 📦 OPCIÓN B: Actualización de Dirección de Envío

### Resumen del Flujo

**Objetivo**: Permitir al usuario gestionar direcciones de envío con validación geográfica.

**Pasos**: 6  
**Tiempo estimado**: 2-4 minutos  
**Complejidad**: Media-Alta

---

### 📍 Paso 1: Acceso a Configuración

**Elementos**:
- Header con foto de perfil
- Menú: Información personal, Direcciones, Métodos de pago, Seguridad
- Badge: "2 direcciones guardadas" o "⚠️ Agrega una dirección"

**Validaciones**:
- Sesión activa
- Tiempo de sesión > 5 min

**Errores**:
- "Tu sesión ha expirado por seguridad. Inicia sesión nuevamente"
- "No pudimos cargar tu información. Verifica tu conexión" + [Reintentar]

**Decisión**: Editar existente → Carga datos | Nueva → Formulario vacío

---

### 📍 Paso 2: Visualización de Direcciones

**Elementos**:
- Tarjetas por dirección:
  - Etiqueta (Casa, Trabajo, etc.)
  - Dirección resumida
  - Badge "Predeterminada"
  - [Editar] [Eliminar] [★ Predeterminada]
- [+ Agregar nueva dirección]
- Contador: "X/10 direcciones"

**Validaciones**:
- Máximo 10 direcciones
- Mínimo 1 dirección
- No eliminar predeterminada sin designar otra

**Errores**:
- "Has alcanzado el límite de 10 direcciones. Elimina una dirección para agregar una nueva"
- "Debes tener al menos una dirección guardada. Agrega otra antes de eliminar esta"
- "No puedes eliminar tu dirección predeterminada. Primero establece otra dirección como predeterminada"
- "Esta dirección está asociada a pedidos en proceso. ¿Estás seguro de eliminarla?" + [Modal]

**Decisión**: Editar → Paso 3 con datos | Nueva → Paso 3 vacío | Eliminar → Modal | Predeterminada → Actualización inmediata

---

### 📍 Paso 3: Formulario de Dirección

**Campos**:
- Etiqueta * (Casa/Trabajo/Otro)
- Nombre de quien recibe *
- Teléfono * (+52 999 999 9999)
- Código Postal * (5 dígitos) → auto-completa Estado/Municipio
- Colonia * (dropdown según CP)
- Calle *
- Número exterior *
- Número interior (opcional)
- Referencias (opcional, máx 200)
- [Checkbox] Dirección predeterminada
- [Usar ubicación actual] (GPS)

**Validaciones en tiempo real**:
- CP: 5 dígitos numéricos
- Teléfono: 10 dígitos
- Calle: mín 3, máx 100, sin números al inicio
- Núm exterior: números y letras (123-A)
- Referencias: máx 200 caracteres

**Errores**:
- "El código postal ingresado no existe. Verifica e intenta nuevamente"
- "Lo sentimos, aún no tenemos cobertura en este código postal"
- "Ingresa un teléfono válido de 10 dígitos"
- "Este campo es obligatorio"
- "No pudimos detectar tu ubicación. Verifica permisos de ubicación en tu navegador"

**Decisión**: GPS → Auto-completa | Manual → Usuario completa

---

### 📍 Paso 4: Validación con Mapa

**Elementos**:
- Mapa interactivo (Google Maps)
- Pin movible en ubicación detectada
- Dirección formateada arriba
- Controles zoom (+/-)
- [Ajustar manualmente]
- Panel lateral:
  - Coordenadas (lat, long)
  - Distancia a referencia
  - "✓ Entregas en 24-48 hrs"
- Toggle: Vista satélite/mapa

**Validaciones**:
- Coordenadas dentro de cobertura
- No en zona restringida
- Pin en ubicación válida

**Errores**:
- "No encontramos esta dirección en el mapa. Verifica los datos ingresados o ajusta manualmente el pin"
- "Esta dirección está fuera de nuestra zona de cobertura. Por ahora solo entregamos en [zonas]"
- "Encontramos varias ubicaciones similares. Ajusta el pin a la ubicación correcta"
- "No pudimos cargar el mapa. Puedes continuar sin validación visual" + [Continuar sin mapa]

**Decisión**: ¿Ubicación correcta? → Sí: Paso 5 | No: Ajustar pin o volver

---

### 📍 Paso 5: Revisión Final

**Elementos**:
- Panel dividido (solo en edición):
  - Izquierda: "Dirección anterior" (gris)
  - Derecha: "Dirección nueva" (blanco)
  - Campos cambiados en amarillo
- Dirección completa formateada
- Miniatura del mapa
- Badge "Predeterminada" si aplica
- [Confirmar y guardar] [Editar] [Cancelar]

**Validaciones**:
- Campos obligatorios completos
- No modificación concurrente
- Sesión activa

**Errores**:
- "Alguien más modificó esta dirección mientras editabas. Por favor revisa los cambios actuales" + [Sobrescribir] [Cancelar]
- "Algunos datos no son válidos. Por favor revisa: [campos]"
- "Tu sesión expiró. Inicia sesión nuevamente. Tus cambios se guardarán temporalmente"

**Decisión**: Confirmar → Paso 6 | Editar → Paso 3 | Cancelar → Lista

---

### 📍 Paso 6: Confirmación Exitosa

**Elementos**:
- ✓ verde animado
- "¡Dirección actualizada exitosamente!"
- "Ahora tus pedidos se enviarán a: [dirección]"
- Timestamp
- Panel de dirección guardada
- Mensaje si es predeterminada
- Acciones rápidas:
  - [Ver todas mis direcciones]
  - [Agregar otra]
  - [Actualizar pago]
  - [Continuar comprando]
- Notificación: Email enviado

**Validaciones**:
- Escritura en BD confirmada
- Email enviado
- Log de auditoría

**Errores**:
- "Tu dirección se guardó correctamente, pero no pudimos enviar el email de confirmación"
- "ℹ️ Tienes 2 pedidos en camino a tu dirección anterior. Este cambio aplicará solo para nuevos pedidos"

**Comportamiento adicional**:
- Auto-redirección en 10s (con contador)
- Si desde checkout: "¿Deseas usar esta dirección para tu pedido actual?"

---

## 🧾 OPCIÓN C: Generación de Factura de Compra

### Resumen del Flujo

**Objetivo**: Generar factura electrónica (CFDI 4.0) cumpliendo normativa SAT de México.

**Pasos**: 6  
**Tiempo estimado**: 4-7 minutos  
**Complejidad**: Alta

---

### 📍 Paso 1: Selección de Compra

**Elementos**:
- Breadcrumb: Mi cuenta > Pedidos > Solicitar factura
- Filtros:
  - Rango fechas (7/30/90 días, personalizado)
  - Estado (Facturables/Facturados/Expirados)
  - Monto (asc/desc)
  - Método de pago
- Lista de pedidos:
  - Número: #PED-YYYYMMDD-####
  - Fecha: "Comprado el DD de MMM, YYYY"
  - Productos: "iPhone 15 + 2 más" + [Ver detalle]
  - Monto: $XXX.XX MXN (IVA incluido)
  - Badge: Pendiente/Facturado/No facturable
  - [Facturar] (habilitado/deshabilitado)
  - Fecha límite: "Hasta: [fecha]" (rojo si <3 días)

**Validaciones**:
- Pedido completado/entregado
- Dentro de 30 días (México)
- No facturado previamente
- Monto > 0

**Errores**:
- "Ya facturado el [fecha]" + [Descargar existente]
- "El plazo para facturar expiró el [fecha]. Por ley solo podemos facturar compras de los últimos 30 días"
- "Este pedido fue cancelado y no es facturable"
- "No tienes compras pendientes de facturar"
- "No pudimos cargar tu historial" + [Reintentar]

**Decisión**: ¿Tiene datos fiscales? → Sí: Pre-llena Paso 3 | No: Paso 2

---

### 📍 Paso 2: Datos Fiscales

**Sin datos previos**:
- RFC * (XXXX######XXX o XXXX######XX#)
  - Validador tiempo real con SAT
  - [¿No sabes tu RFC?] → consulta SAT
- Tipo de persona * (Física/Moral)
- Razón Social / Nombre * (auto-completa si RFC válido)
- Régimen Fiscal * (dropdown filtrado por tipo)
  - Formato: "601 - General de Ley Personas Morales"
- CP Fiscal * (5 dígitos)
- Email factura * (pre-llena cuenta)
- [Checkbox] Guardar para futuras facturas

**Con datos existentes**:
- Lista de perfiles (máx 5):
  - RFC (parcialmente oculto)
  - Razón Social
  - Régimen
  - Badge "Predeterminado"
  - [Seleccionar]
- [+ Agregar nuevo perfil]

**Validaciones**:
- RFC: formato + checksum
- Validación SAT (si disponible)
- Email: formato válido
- CP: 5 dígitos en catálogo SAT
- Régimen compatible con tipo persona
- Razón Social: mín 3 caracteres

**Errores**:
- "El RFC ingresado no tiene un formato válido. Verifica e intenta nuevamente"
- "Este RFC no existe en el padrón del SAT. Verifica tu RFC" + [Consultar SAT]
- "Este RFC no está activo ante el SAT. No podemos emitir facturas con este RFC"
- "No pudimos validar tu RFC con el SAT. Podemos continuar, pero verifica que tu RFC sea correcto"
- "Este régimen fiscal no es válido para Persona Física. Selecciona uno diferente"
- "Has alcanzado el límite de 5 perfiles fiscales. Elimina uno para agregar otro"

**Decisión**: ¿Válido? → Sí: Paso 3 | No: Corregir

---

### 📍 Paso 3: Uso de CFDI

**Elementos**:
- Título: "Selecciona el uso que le darás a esta factura"
- ℹ️ "El uso de CFDI es requerido por el SAT y debe coincidir con la naturaleza de tu compra"
- Dropdown con búsqueda:
  - Lista completa catálogo SAT
  - Formato: Código + Descripción
  - Comunes destacados:
    - G01 - Adquisición de mercancías
    - G03 - Gastos en general
    - I04 - Equipo de cómputo
    - D01 - Honorarios médicos
    - P01 - Por definir
- Buscador: "Escribe para buscar..."
- [¿No sabes cuál elegir?] → Modal:
  - Explicación usos comunes
  - Recomendación según productos
  - "Consulta con tu contador"
  - [Ver catálogo SAT]

**Validaciones**:
- Selección obligatoria
- Código vigente en catálogo
- Compatible con tipo persona

**Errores**:
- "Debes seleccionar el uso de CFDI para continuar"
- "Este uso de CFDI no es compatible con tu tipo de persona"
- "Este uso de CFDI ya no está vigente. Selecciona uno actualizado"
- "No pudimos cargar el catálogo de usos de CFDI" + [Reintentar]

**Funcionalidad inteligente**:
- Sugerencia basada en productos
- Advertencia P01: "⚠️ P01 no permite deducciones fiscales. ¿Estás seguro?"

**Decisión**: ¿Apropiado? → Sí: Paso 4 | Incierto: Ayuda

---

### 📍 Paso 4: Revisión y Desglose

**Secciones**:

**1. Datos Fiscales** (editable):
- RFC, Razón Social, Régimen, Uso CFDI, CP, Email
- [✏️ Editar] → regresa Paso 2

**2. Datos del Pedido**:
- Número, Fecha, Método pago

**3. Desglose de Productos** (tabla):
| Cant | Descripción (SKU) | Precio Unit | Subtotal |
|------|-------------------|-------------|----------|
| 1 | iPhone 15 Pro (IP15P) | $20,000.00 | $20,000.00 |

**4. Resumen Financiero**:
- Subtotal: $XX,XXX.XX
- Descuentos: -$XXX.XX
- Subtotal final: $XX,XXX.XX
- **IVA (16%)**: $X,XXX.XX
- IEPS (si aplica): $XXX.XX
- **Total**: $XX,XXX.XX MXN
- Cantidad en letra

**5. Información Legal**:
- Forma de pago (catálogo SAT)
- Método: PUE/PPD
- Tipo: I - Ingreso
- Moneda: MXN

**Elementos adicionales**:
- [Vista previa PDF] (temporal, sin timbre)
- [Checkbox obligatorio] "He revisado y confirmo que todos los datos son correctos"
- ⚠️ "Una vez generada, NO se podrá modificar ni cancelar durante 24 horas"
- [Generar factura] (deshabilitado hasta checkbox)
- [Editar datos] [Cancelar]

**Validaciones**:
- Integridad datos fiscales
- Cálculo impuestos correcto
- Total coincide con pedido

**Errores**:
- "Detectamos una inconsistencia en los montos. Por favor contacta a soporte"
- "Algunos productos no son facturables: [lista]. Solo se facturarán los válidos" + checkbox aceptar
- "Falta información requerida. Por favor completa tus datos fiscales"

**Decisión**: ¿Correcto? → Sí: Paso 5 | No: Editar

---

### 📍 Paso 5: Timbrado

**Elementos**:
- Indicador de progreso:
  1. ✓ Validando datos fiscales...
  2. ⟳ Generando XML de factura...
  3. ⏸ Conectando con el SAT...
  4. ⏸ Timbrando factura...
  5. ⏸ Generando PDF...
- Barra: [=========>........] 45%
- "Esto puede tomar hasta 30 segundos. No cierres esta ventana"
- Spinner animado
- Info técnica (colapsable):
  - UUID: En espera...
  - Serie y folio: Asignando...
  - Fecha timbrado: Pendiente...

**Validaciones backend**:
- Certificados vigentes (CSD)
- Conexión PAC exitosa
- XML válido SAT
- UUID único
- Sello digital aplicado

**Errores críticos**:

**SAT no disponible**:
- "❌ El servicio del SAT no está disponible temporalmente. Esto es común en fines de semana o días festivos"
- "Intenta nuevamente en unos minutos o espera a día hábil"
- [Reintentar] [Notificarme]
- Guardar en cola para procesamiento automático

**RFC bloqueado**:
- "❌ El RFC está en el listado de contribuyentes no localizados o con certificados revocados del SAT"
- "No podemos emitir factura con este RFC. Regulariza tu situación fiscal"
- [Usar otro RFC] → Paso 2

**Certificados vencidos**:
- "❌ Nuestros certificados digitales están actualizándose. Intenta en 10 minutos"
- [Notificarme por email]

**XML rechazado**:
- "❌ El SAT rechazó la factura. Código de error: [código]"
- Detalle técnico + explicación
- [Reintentar con datos corregidos] [Contactar soporte]

**Timeout**:
- "⏱️ La petición tardó demasiado. El proceso puede haber sido exitoso"
- "Verificando estado..."
- Si existe → Paso 6 | No existe → Reintentar

**Manejo de errores**:
- Guardar solicitud para auditoría
- Email con detalles y pasos
- Bloqueo de pedido (no doble facturación)
- [Hablar con soporte] → chat/ticket

**Decisión**: ¿Timbrado exitoso? → Sí: Paso 6 | No: Error específico

---

### 📍 Paso 6: Descarga

**Elementos**:

**Mensaje de éxito**:
- ✓ grande verde
- "¡Tu factura ha sido generada exitosamente!"
- "El XML y PDF se enviaron a tu correo electrónico"

**Panel de información**:
- **UUID**: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX [📋 Copiar]
  - "Este es tu folio fiscal único. Guárdalo para cualquier aclaración"
- Serie y Folio: A-12345
- Fecha emisión: DD de MMM, YYYY - HH:MM:SS
- Fecha timbrado: DD de MMM, YYYY - HH:MM:SS
- Sello SAT: ABC...XYZ
- **Monto total**: $XX,XXX.XX MXN

**Descarga**:
- 📄 [Descargar PDF] (principal)
  - Archivo: Factura_[RFC]_[UUID].pdf
- 📋 [Descargar XML] (secundario)
  - Archivo: Factura_[RFC]_[UUID].xml
- 📦 [Descargar ambos (.zip)] (terciario)
- Contador: "Archivos descargados: 0/2"
- Vista previa PDF (opcional)

**Opciones adicionales**:
- 📧 Reenviar por email:
  - Campo email + [Reenviar]
  - "Correo reenviado a [email]"
- 🔗 Enlace de descarga:
  - URL temporal (7 días)
  - [📋 Copiar enlace]
  - "Este enlace expira el [fecha]"
- ✅ Verificar en SAT:
  - Link: https://verificacfdi.facturaelectronica.sat.gob.mx/
  - Instrucciones breves

**Información colapsable**:
- Emisor
- Receptor
- Desglose productos
- Totales e impuestos
- Método y forma de pago

**Ayuda (FAQ)**:
- "¿Cómo verifico mi factura en el SAT?"
- "¿Puedo cancelar esta factura?"
- "¿Qué hago si hay un error?"
- "¿Cómo hago una aclaración?"
- Políticas de cancelación:
  - No cancelable en 24h
  - Después: solicitar (requiere aceptación si monto alto)

**Navegación**:
- [Ir a mis facturas]
- [Facturar otro pedido]
- [Volver a pedidos]
- [Continuar comprando]

**Validaciones post**:
- Email enviado (estado: enviado/pendiente/error)
- PDF y XML generados
- Registro en BD

**Errores post-generación**:
- "La factura se timbró exitosamente, pero hubo un problema al generar el PDF. Puedes descargarlo más tarde desde 'Mis Facturas'"
- "Tu factura se generó, pero no pudimos enviar el email. Puedes descargar los archivos aquí"

**Automático**:
- Email con:
  - PDF y XML adjuntos (o enlaces)
  - UUID y datos principales
  - Enlaces descarga alternativos
  - Instrucciones verificación SAT
- Registro en historial
- Marca pedido "Facturado"
- Notificación push (si app): "Tu factura está lista"

**Legal**:
- "Esta factura es un CFDI conforme a la legislación del SAT"
- "Conserva el XML para efectos fiscales"
- [Aviso de privacidad]

**Estadísticas (opcional)**:
- "Has generado X facturas este año por $XXX,XXX.XX"
- "Próxima factura disponible: [folio]"

---

## 📊 Matriz de Validaciones

### Validaciones Comunes

| Campo | Tipo | Regla | Mensaje Error |
|-------|------|-------|---------------|
| Email | String | Formato @dominio | "Ingresa un email válido" |
| Teléfono | String | 10 dígitos | "Ingresa un teléfono válido de 10 dígitos" |
| Código Postal | String | 5 dígitos | "El código postal debe tener 5 dígitos" |
| Texto largo | String | 10-500 chars | "Mínimo 10 caracteres" / "Máximo 500 caracteres" |
| RFC | String | Formato XXXX######XXX | "El RFC ingresado no tiene un formato válido" |

---

## 🎨 Patrones de Diseño Aplicados

### Validación Progresiva

**Principio**: Validar en múltiples niveles para mejor UX.

**Niveles**:
1. **Cliente (JavaScript)**: Formato básico, longitud
2. **Visual**: Resaltado en tiempo real
3. **Servidor**: Validación contra BD/APIs
4. **Negocio**: Reglas complejas post-submit

**Ejemplo Implementación**:
```javascript
// Nivel 1: Cliente
input.addEventListener('blur', (e) => {
  if (!validateEmail(e.target.value)) {
    showError('Formato de email inválido');
  }
});

// Nivel 2: Visual
input.addEventListener('input', (e) => {
  if (e.target.value.includes('@')) {
    input.classList.add('valid');
  }
});

// Nivel 3: Servidor
async function submitForm() {
  const response = await api.post('/validate', formData);
  if (!response.valid) {
    showErrors(response.errors);
  }
}
```

---

### Retroalimentación Contextual

**Principio**: Mensajes específicos, no genéricos.

**Mal ejemplo**: ❌ "Error en el formulario"

**Buen ejemplo**: ✅ "El RFC ingresado no tiene un formato válido. Debe ser de 13 caracteres (Ej: GOMJ850212XXX)"

**Componentes**:
- 🎯 **Específico**: Qué está mal
- 💡 **Accionable**: Cómo solucionarlo
- 📘 **Educativo**: Por qué es importante

---

### Manejo de Estados

**Estados posibles**:
```
IDLE → LOADING → SUCCESS
  ↓        ↓         ↓
ERROR ← TIMEOUT ← RETRY
```

**Implementación visual**:
| Estado | Color | Ícono | Mensaje |
|--------|-------|-------|---------|
| Idle | Gris | - | Campo vacío |
| Valid | Verde | ✓ | "Correcto" |
| Invalid | Rojo | ✗ | Mensaje específico |
| Warning | Amarillo | ⚠️ | "Verifica esto" |
| Loading | Azul | ⟳ | "Validando..." |

---

### Prevención de Errores

**Mejor que manejar errores: prevenirlos**

**Técnicas aplicadas**:

1. **Deshabilitar opciones no válidas**
   - Fechas pasadas en calendario
   - Especialidades sin disponibilidad
   - Botones hasta completar campos

2. **Auto-completado inteligente**
   - CP → auto-llena Estado/Municipio
   - RFC → sugiere Razón Social
   - Búsqueda → sugerencias similares

3. **Límites visuales**
   - Contador de caracteres
   - Barra de progreso
   - Temporizador de bloqueo

4. **Validación en tiempo real**
   - Formato de email mientras escribe
   - Disponibilidad de horarios
   - Conflictos de datos

---

### Recuperación de Errores

**Cuando ocurre un error, ofrecer salidas claras**

**Patrón aplicado**:
```
ERROR detectado
  ↓
Mostrar mensaje específico
  ↓
Ofrecer 2-3 opciones:
  1. Reintentar
  2. Alternativa
  3. Contactar soporte
```

**Ejemplo**:
```
❌ "Este horario acaba de ser reservado"
  → [Elegir otro horario]
  → [Volver al calendario]
  → [Hablar con soporte]
```

---

## 🔐 Consideraciones de Seguridad

### Autenticación y Sesión

**Medidas implementadas**:
- ✅ Límite de 3 intentos fallidos
- ✅ Bloqueo temporal (15 min)
- ✅ Tokens de sesión con expiración
- ✅ Validación de sesión en cada paso crítico
- ✅ HTTPS obligatorio (implícito)

### Sanitización de Datos

**Inputs que requieren sanitización**:
```javascript
// Motivo de consulta
function sanitize(input) {
  return input
    .replace(/[<>{}]/g, '')  // Eliminar caracteres peligrosos
    .trim()
    .substring(0, 500);      // Limitar longitud
}

// RFC
function sanitizeRFC(rfc) {
  return rfc
    .toUpperCase()
    .replace(/[^A-Z0-9]/g, '')
    .substring(0, 13);
}
```

### Validación de Datos Fiscales

**Verificaciones contra SAT**:
1. RFC existe en padrón
2. RFC está activo
3. No en lista negra (contribuyentes no localizados)
4. Certificados vigentes

---

## 📱 Responsive Design

### Breakpoints Recomendados

```css
/* Mobile First */
.container { width: 100%; }

/* Tablet */
@media (min-width: 768px) {
  .container { width: 750px; }
}

/* Desktop */
@media (min-width: 1024px) {
  .container { width: 970px; }
}

/* Large Desktop */
@media (min-width: 1280px) {
  .container { width: 1170px; }
}
```

### Adaptaciones por Dispositivo

**Mobile**:
- Formularios de 1 columna
- Botones de altura táctil (44px min)
- Menús colapsables
- Inputs con teclado apropiado (numeric, email, tel)

**Tablet**:
- Formularios de 2 columnas
- Sidebar visible
- Calendario más espaciado

**Desktop**:
- Todos los controles visibles
- Tooltips en hover
- Atajos de teclado

---

## 🌐 Internacionalización (i18n)

### Preparación para Múltiples Idiomas

**Estructura recomendada**:
```javascript
const messages = {
  es: {
    login: {
      title: "Iniciar sesión",
      email: "Correo electrónico",
      password: "Contraseña",
      submit: "Entrar"
    },
    errors: {
      required: "Este campo es obligatorio",
      invalidEmail: "Ingresa un email válido"
    }
  },
  en: {
    login: {
      title: "Log in",
      email: "Email address",
      password: "Password",
      submit: "Sign in"
    },
    errors: {
      required: "This field is required",
      invalidEmail: "Please enter a valid email"
    }
  }
};
```

### Formato de Fechas y Monedas

**Fechas**:
- España: DD/MM/YYYY
- México: DD de MMM, YYYY
- USA: MM/DD/YYYY

**Moneda**:
- México: $1,234.56 MXN
- USA: $1,234.56 USD
- Europa: €1.234,56 EUR

---

## ♿ Accesibilidad (WCAG 2.1)

### Principios Aplicados

**1. Perceptible**
- ✅ Contraste mínimo 4.5:1
- ✅ Texto alternativo en imágenes
- ✅ Etiquetas en todos los campos

**2. Operable**
- ✅ Navegación por teclado (Tab)
- ✅ Área táctil mínima 44x44px
- ✅ Sin límites de tiempo restrictivos (excepto seguridad)

**3. Comprensible**
- ✅ Mensajes de error claros
- ✅ Instrucciones visibles
- ✅ Navegación consistente

**4. Robusto**
- ✅ HTML semántico
- ✅ ARIA labels donde necesario
- ✅ Compatible con lectores de pantalla

### Atributos ARIA Recomendados

```html
<!-- Campo con error -->
<input 
  type="email" 
  id="email"
  aria-label="Correo electrónico"
  aria-describedby="email-error"
  aria-invalid="true"
  required
/>
<span id="email-error" role="alert">
  Ingresa un email válido
</span>

<!-- Botón con estado de carga -->
<button 
  type="submit"
  aria-busy="true"
  aria-label="Generando factura, por favor espera">
  <span class="spinner"></span>
  Generando...
</button>

<!-- Sección colapsable -->
<button 
  aria-expanded="false"
  aria-controls="faq-content">
  Preguntas frecuentes
</button>
<div id="faq-content" hidden>
  ...
</div>
```

---

## 🧪 Testing y Quality Assurance

### Casos de Prueba Críticos

#### Opción A: Citas Médicas

| Test ID | Descripción | Pasos | Resultado Esperado |
|---------|-------------|-------|-------------------|
| A-001 | Login exitoso | 1. Ingresar credenciales válidas<br>2. Click "Iniciar sesión" | Redirige a dashboard |
| A-002 | Login fallido 3x | 1. Ingresar 3 veces credenciales incorrectas | Cuenta bloqueada 15 min |
| A-003 | Selección de especialidad sin disponibilidad | 1. Click en especialidad gris | Mensaje "Sin citas disponibles" + opción lista de espera |
| A-004 | Bloqueo temporal de horario | 1. Seleccionar horario<br>2. Esperar 5 min sin completar | Horario se libera automáticamente |
| A-005 | Detección de emergencia | 1. Escribir "infarto" en motivo | Banner amarillo con opción llamar 911 |

#### Opción B: Direcciones

| Test ID | Descripción | Pasos | Resultado Esperado |
|---------|-------------|-------|-------------------|
| B-001 | Auto-completado por CP | 1. Ingresar CP válido (76000) | Estado/Municipio se auto-llenan |
| B-002 | CP sin cobertura | 1. Ingresar CP fuera de zona | Mensaje "Sin cobertura en este CP" |
| B-003 | Geolocalización GPS | 1. Click "Usar ubicación actual" | Campos se auto-llenan con ubicación |
| B-004 | Eliminar dirección predeterminada | 1. Click "Eliminar" en predeterminada | Mensaje "No puedes eliminar predeterminada" |
| B-005 | Límite 10 direcciones | 1. Intentar agregar 11ª dirección | Mensaje "Has alcanzado el límite de 10" |

#### Opción C: Facturas

| Test ID | Descripción | Pasos | Resultado Esperado |
|---------|-------------|-------|-------------------|
| C-001 | RFC válido | 1. Ingresar RFC formato correcto | Validación verde + auto-completa datos |
| C-002 | RFC bloqueado | 1. Ingresar RFC en lista negra SAT | Error "RFC no activo ante el SAT" |
| C-003 | Selección uso CFDI | 1. Seleccionar P01 | Advertencia "No permite deducciones" |
| C-004 | Timbrado exitoso | 1. Completar todos los pasos<br>2. Click "Generar" | Factura timbrada + UUID generado |
| C-005 | SAT no disponible | 1. Generar factura cuando SAT caído | Mensaje "SAT temporalmente no disponible" + opción notificar |

---

## 📈 Métricas de Éxito

### KPIs por Flujo

**Opción A - Citas**:
- Tasa de completación: >85%
- Tiempo promedio: <4 minutos
- Errores de validación: <2 por sesión
- Abandono en paso 4: <10%

**Opción B - Direcciones**:
- Uso de GPS: >60%
- Direcciones con error de geolocalización: <5%
- Ediciones post-guardado: <15%

**Opción C - Facturas**:
- Facturas generadas primer intento: >90%
- Tiempo de timbrado: <30 segundos
- Errores de RFC: <8%
- Descargas de XML+PDF: >95%

---

## 🔄 Flujos Alternativos

### Usuarios No Registrados

**Opción A**:
```
Landing → [Solicitar cita]
  ↓
¿Registrado?
  ├─ NO → Registro rápido (email + password)
  └─ SÍ → Login → Flujo normal
```

**Opción B**:
```
Checkout → [Agregar dirección]
  ↓
¿Registrado?
  ├─ NO → Guest checkout (dirección sin guardar)
  └─ SÍ → Flujo normal con guardado
```

---

## 🛠️ Stack Tecnológico Recomendado

### Frontend
- **HTML5**: Estructura semántica
- **CSS3 / Tailwind CSS**: Estilos utility-first
- **JavaScript (ES6+)**: Lógica de negocio
- **React / Vue** (opcional): Para apps complejas

### Backend
- **Node.js / Python**: API REST
- **PostgreSQL / MySQL**: Base de datos
- **Redis**: Cache y bloqueos temporales
- **WebSocket**: Actualización real-time

### Integraciones
- **Google Maps API**: Geolocalización
- **SAT Web Services**: Validación RFC y timbrado
- **SendGrid / Mailgun**: Envío de emails
- **Twilio**: SMS y WhatsApp

### Infraestructura
- **AWS / Azure / GCP**: Hosting
- **CDN**: Assets estáticos
- **Load Balancer**: Distribución de carga
- **Monitoring**: Sentry, DataDog

---

## 📚 Glosario Técnico

| Término | Definición |
|---------|------------|
| **AJAX** | Asynchronous JavaScript And XML - Peticiones asíncronas sin recargar página |
| **CFDI** | Comprobante Fiscal Digital por Internet - Factura electrónica en México |
| **CSD** | Certificado de Sello Digital - Firma electrónica para facturación |
| **Geolocalización** | Determinar ubicación geográfica mediante coordenadas GPS |
| **PAC** | Proveedor Autorizado de Certificación - Intermediario para timbrado SAT |
| **RFC** | Registro Federal de Contribuyentes - Identificador fiscal en México |
| **SAT** | Servicio de Administración Tributaria - Autoridad fiscal de México |
| **UUID** | Universally Unique IDentifier - Folio fiscal único de 36 caracteres |
| **Validación en tiempo real** | Verificación de datos mientras el usuario escribe |
| **Wireframe** | Prototipo de baja fidelidad que muestra estructura sin diseño final |

---

## 🔗 Referencias y Recursos

### Documentación Oficial

**México - SAT**:
- [Portal SAT](https://www.sat.gob.mx/)
- [Catálogo CFDI 4.0](http://omawww.sat.gob.mx/tramitesyservicios/Paginas/documentos/Catalogo_CFDI.xls)
- [Consulta RFC](https://www.sat.gob.mx/aplicacion/31274/consulta-tu-clave-de-rfc-mediante-curp)
- [Verificación de CFDI](https://verificacfdi.facturaelectronica.sat.gob.mx/)

**UX/UI Best Practices**:
- [Nielsen Norman Group](https://www.nngroup.com/)
- [Material Design Guidelines](https://material.io/design)
- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)

**Accesibilidad**:
- [WCAG 2.1](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM](https://webaim.org/)
- [A11y Project](https://www.a11yproject.com/)

---

## 📝 Notas Finales

### Mantenimiento del Documento

Este documento debe actualizarse cuando:
- ✅ Se agreguen nuevos pasos o flujos
- ✅ Cambien validaciones o reglas de negocio
- ✅ Se modifiquen mensajes de error
- ✅ Actualicen regulaciones (ej: catálogo SAT)

### Versionado

- **v1.0.0** (2025-12-24): Versión inicial con 3 flujos completos
- **v1.1.0** (Pendiente): Agregar flujos de pago y checkout
- **v2.0.0** (Futuro): Integración con app móvil

---

<div align="center">

**Documentación creada para el proyecto:**  
**[ux-wireframes-interactive](https://github.com/BENMP0902/ux-wireframes-interactive)**

Última actualización: Diciembre 24, 2025

</div>