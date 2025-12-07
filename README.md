# Silverthorne Holdings - API REST Backend

Sistema completo de e-commerce para **Silverthorne Holdings**, empresa dedicada a la prestación de servicios y venta de insumos informáticos. Incluye gestión de usuarios, productos, carritos y órdenes con autenticación JWT, roles de usuario, gestión de stock y panel de administración.

## Características

- **Gestión de Usuarios**: Registro, login, verificación por email, perfiles, recuperación de contraseña
- **Roles de Usuario**: Usuario normal y Administrador con permisos granulares
- **Catálogo de Productos**: CRUD completo con imágenes, categorías, búsqueda y filtros
- **Carrito de Compras**: Agregar, actualizar, eliminar productos con validación de stock
- **Sistema de Órdenes**: Crear órdenes desde el carrito con tracking completo
- **Gestión de Stock**: Actualización automática al realizar compras y cancelaciones
- **Panel de Administración**: Gestión de productos, órdenes, estadísticas y usuarios
- **Documentación Swagger**: API completamente documentada e interactiva
- **Subida de Imágenes**: Gestión de imágenes de productos con Multer
- **Integración Transbank**: Pagos con Webpay Plus (opcional)
- **Notificaciones por Email**: Verificación de cuenta y notificaciones de órdenes

## Requisitos Previos

### Software Requerido

- **Node.js**: Versión 14.0.0 o superior (recomendado: v18 LTS o v20 LTS)
- **npm**: Versión 6.0.0 o superior (incluido con Node.js)
- **Git**: Versión 2.20.0 o superior

### Servicios Externos

- **Supabase** (OBLIGATORIO): Base de datos PostgreSQL y almacenamiento
  - Cuenta gratuita disponible en: https://supabase.com/
  - Plan gratuito incluye: 500 MB de base de datos, 1 GB de almacenamiento
  
- **Vercel** (OBLIGATORIO para producción): Hosting del backend
  - Cuenta gratuita disponible en: https://vercel.com/

### Servicios Opcionales

- **Transbank**: Para integración de pagos (Webpay Plus)
- **Gmail/SMTP**: Para envío de emails de notificación

## Instalación Local

### Paso 1: Clonar el Repositorio

```bash
git clone <url-del-repositorio>
cd ProyectoDeTitulo/Backend
```

### Paso 2: Instalar Dependencias

```bash
npm install
```

### Paso 3: Configurar Supabase

1. Crea un proyecto en [Supabase](https://supabase.com/)
2. Obtén tus credenciales desde Settings → API:
   - Project URL (SUPABASE_URL)
   - anon public key (SUPABASE_KEY)
   - service_role key (SUPABASE_SERVICE_ROLE_KEY)
3. Ejecuta el script SQL en Supabase SQL Editor:
   - Abre `Backend/database_schema.sql`
   - Copia y pega el contenido en el editor SQL de Supabase
   - Ejecuta el script para crear todas las tablas necesarias

### Paso 4: Configurar Variables de Entorno

Crea un archivo `.env` en la carpeta `Backend/` con el siguiente contenido:

```env
# Base de datos (Supabase) - OBLIGATORIO
SUPABASE_URL=https://tu-proyecto.supabase.co
SUPABASE_KEY=tu_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=tu_supabase_service_role_key

# JWT - OBLIGATORIO
JWT_SECRET=tu_jwt_secret_muy_seguro_y_largo_minimo_32_caracteres

# Servidor
PORT=4005
NODE_ENV=development

# Frontend URL (para emails y redirects)
FRONTEND_URL=http://localhost:5173

# CORS (URLs permitidas separadas por comas)
ALLOWED_ORIGINS=http://localhost:5173

# Transbank (opcional - para pagos)
TRANSBANK_API_KEY=tu_api_key
TRANSBANK_ENVIRONMENT=integration
TRANSBANK_COMMERCE_CODE=tu_commerce_code

# Email (opcional - para notificaciones)
EMAIL_USER=tu_email@gmail.com
EMAIL_PASS=tu_password_de_aplicacion
```

### Paso 5: Poblar Base de Datos con Productos de Prueba

```bash
npm run seed:products
```

### Paso 6: Iniciar el Servidor

```bash
# Desarrollo (con nodemon para auto-reload)
npm run dev

# Producción
npm start
```

El servidor estará disponible en: `http://localhost:4005`

## Documentación de la API

Una vez iniciado el servidor, accede a la documentación Swagger interactiva en:

```
http://localhost:4005/api-docs
```

La documentación incluye:
- Descripción de todos los endpoints
- Parámetros requeridos y opcionales
- Ejemplos de requests y responses
- Pruebas interactivas de la API

## Endpoints Principales

### Autenticación (`/users`)

- `POST /users/register` - Registrar nuevo usuario
- `POST /users/login` - Iniciar sesión
- `GET /users/verify/:token` - Verificar cuenta por email
- `GET /users/profile` - Obtener perfil del usuario autenticado
- `PUT /users/profile` - Actualizar perfil
- `POST /users/reset-password-request` - Solicitar reset de contraseña
- `POST /users/reset-password/:token` - Restablecer contraseña

### Productos (`/api/products`)

**Público:**
- `GET /api/products` - Listar productos (con paginación y filtros)
- `GET /api/products/:id` - Obtener producto por ID
- `GET /api/products/categories` - Obtener todas las categorías
- `GET /api/products/featured` - Obtener productos destacados
- `GET /api/products/on-sale` - Obtener productos en oferta

**Solo Administrador:**
- `POST /api/products` - Crear nuevo producto (con imagen)
- `PUT /api/products/:id` - Actualizar producto
- `DELETE /api/products/:id` - Eliminar producto (soft delete)
- `PATCH /api/products/:id/stock` - Actualizar stock
- `GET /api/products/admin/all` - Listar todos los productos (incluyendo eliminados)

### Carrito (`/api/cart`)

**Usuarios autenticados:**
- `GET /api/cart` - Obtener carrito completo
- `GET /api/cart/summary` - Resumen del carrito (total, cantidad de items)
- `POST /api/cart/add` - Agregar producto al carrito
- `PUT /api/cart/update` - Actualizar cantidad de un producto
- `DELETE /api/cart/remove/:productId` - Eliminar producto del carrito
- `DELETE /api/cart/clear` - Limpiar carrito completo

### Órdenes (`/api/orders`)

**Usuarios:**
- `POST /api/orders` - Crear orden desde el carrito
- `GET /api/orders/my-orders` - Obtener mis órdenes
- `GET /api/orders/:orderId` - Obtener detalle de una orden
- `PATCH /api/orders/:orderId/cancel` - Cancelar orden pendiente

**Administrador:**
- `GET /api/orders/admin/all` - Obtener todas las órdenes (con filtros)
- `GET /api/orders/admin/stats` - Estadísticas del sistema
- `PATCH /api/orders/admin/:orderId/status` - Actualizar estado de orden

### Contacto (`/api/contact`)

- `POST /api/contact` - Enviar mensaje de contacto

## Autenticación

El sistema utiliza **JWT (JSON Web Tokens)** para la autenticación. Para acceder a endpoints protegidos, incluye el token en el header:

```
Authorization: Bearer <tu_jwt_token>
```

El token se obtiene al hacer login exitoso y tiene una duración configurable.

## Roles de Usuario

### Usuario Normal (`user`)

- Ver productos y categorías
- Buscar y filtrar productos
- Gestionar su carrito personal
- Crear y ver sus órdenes
- Cancelar órdenes pendientes
- Gestionar su perfil personal
- Ver historial de compras

### Administrador (`admin`)

- Todas las funciones de usuario normal
- Gestionar productos (crear, editar, eliminar)
- Gestionar stock de productos
- Ver todas las órdenes del sistema
- Actualizar estado de órdenes
- Ver estadísticas del sistema
- Gestionar usuarios (futuro)

## Estructura del Proyecto

```
Backend/
├── controllers/          # Lógica de negocio
│   ├── userController.js
│   ├── productController.js
│   ├── cartController.js
│   ├── orderController.js
│   └── contactController.js
├── models/              # Modelos de Supabase/PostgreSQL
│   ├── userModel.js
│   ├── productModel.js
│   ├── cartModel.js
│   └── orderModel.js
├── routes/              # Definición de rutas
│   ├── userRouter.js
│   ├── productRoutes.js
│   ├── cartRoutes.js
│   ├── orderRoutes.js
│   └── contactRoutes.js
├── middlewares/         # Middlewares personalizados
│   ├── auth.js
│   ├── authAdmin.js
│   └── authMiddleware.js
├── utils/               # Utilidades
│   ├── seedProducts.js
│   ├── testSystem.js
│   └── logger.js
├── uploads/             # Archivos subidos
│   └── products/        # Imágenes de productos
├── swagger/             # Documentación API
│   ├── swagger.js
│   ├── productRoutes.js
│   ├── cartRoutes.js
│   ├── orderRoutes.js
│   └── userRoutes.js
├── database.js          # Configuración de conexión a Supabase
├── database_schema.sql   # Script SQL para crear tablas
├── server.js            # Configuración del servidor Express
├── index.js             # Punto de entrada de la aplicación
└── package.json         # Dependencias y scripts
```

## Ejemplos de Uso

### 1. Registrar Usuario

```bash
curl -X POST http://localhost:4005/users/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Juan Pérez",
    "email": "juan@example.com",
    "password": "Password123!"
  }'
```

### 2. Iniciar Sesión

```bash
curl -X POST http://localhost:4005/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "juan@example.com",
    "password": "Password123!"
  }'
```

### 3. Agregar Producto al Carrito

```bash
curl -X POST http://localhost:4005/api/cart/add \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <tu_token>" \
  -d '{
    "productId": "product_id_aqui",
    "quantity": 2
  }'
```

### 4. Crear Orden

```bash
curl -X POST http://localhost:4005/api/orders \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <tu_token>" \
  -d '{
    "shippingAddress": {
      "street": "Calle Principal 123",
      "city": "Santiago",
      "state": "Región Metropolitana",
      "zipCode": "12345",
      "country": "Chile"
    },
    "paymentMethod": "cash_on_delivery"
  }'
```

## Scripts Disponibles

- `npm start` - Iniciar servidor en producción
- `npm run dev` - Iniciar servidor en desarrollo (con nodemon)
- `npm run seed:products` - Poblar BD con productos de prueba
- `npm run test:system` - Ejecutar pruebas del sistema (conexión a BD, etc.)
- `npm test` - Ejecutar tests unitarios
- `npm run lint` - Verificar código con ESLint

## Despliegue en Vercel

### Paso 1: Preparar el Repositorio

Asegúrate de que tu código esté en un repositorio de GitHub, GitLab o Bitbucket.

### Paso 2: Conectar con Vercel

1. Ve a [Vercel](https://vercel.com/)
2. Inicia sesión con tu cuenta de GitHub/GitLab/Bitbucket
3. Haz clic en "Add New Project"
4. Selecciona tu repositorio
5. Configura el proyecto:
   - **Framework Preset**: Other
   - **Root Directory**: `Backend`
   - **Build Command**: (dejar vacío)
   - **Output Directory**: (dejar vacío)
   - **Install Command**: `npm install`

### Paso 3: Configurar Variables de Entorno en Vercel

Ve a **Settings → Environment Variables** y agrega las siguientes variables:

**Variables OBLIGATORIAS:**

```env
SUPABASE_URL=https://tu-proyecto.supabase.co
SUPABASE_KEY=tu_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=tu_supabase_service_role_key
JWT_SECRET=tu_jwt_secret_muy_seguro_y_largo_minimo_32_caracteres
FRONTEND_URL=https://tu-frontend.vercel.app
```

**Variables OPCIONALES (recomendadas):**

```env
NODE_ENV=production
ALLOWED_ORIGINS=https://tu-frontend.vercel.app,https://otro-dominio.com
EMAIL_USER=tu_email@gmail.com
EMAIL_PASS=tu_password_de_aplicacion
TRANSBANK_API_KEY=tu_api_key
TRANSBANK_ENVIRONMENT=production
TRANSBANK_COMMERCE_CODE=tu_commerce_code
PORT=4005
```

**Importante:**
- Selecciona el entorno correcto (Production, Preview, Development) para cada variable
- Después de agregar las variables, ve a **Deployments** y haz clic en **"Redeploy"** en el último deployment

### Paso 4: Verificar el Despliegue

Una vez desplegado, Vercel te proporcionará una URL (ej: `https://tu-backend.vercel.app`). 

**Verificaciones:**
1. Visita la URL y deberías ver un JSON con información de la API
2. Visita `https://tu-backend.vercel.app/api-docs` para ver la documentación Swagger
3. Prueba un endpoint público: `https://tu-backend.vercel.app/api/products`

Si ves un error 500, revisa los logs en **Vercel → Deployments → Ver logs**.

## Funcionalidades Destacadas

### Seguridad

- Autenticación JWT robusta con tokens seguros
- Verificación de email para nuevos usuarios
- Roles y permisos granulares (user/admin)
- Validación de datos completa en todos los endpoints
- Rate limiting para prevenir abusos
- Helmet.js para headers de seguridad HTTP
- CORS configurado para permitir solo orígenes autorizados

### Carrito Inteligente

- Validación automática de stock antes de agregar productos
- Cálculo de totales en tiempo real
- Persistencia por usuario en base de datos
- Limpieza automática al crear una orden
- Manejo de productos agotados

### Gestión de Productos

- Categorización automática
- Subida de imágenes con Multer
- Control de stock en tiempo real
- Soft delete para mantener historial
- Búsqueda y filtros avanzados
- Productos destacados y en oferta

### Sistema de Órdenes

- Transacciones atómicas para garantizar consistencia
- Estados de orden configurables (pending, processing, shipped, delivered, cancelled)
- Restauración automática de stock en cancelaciones
- Tracking completo de cada orden
- Integración con Transbank para pagos (opcional)

### Panel Administrativo

- Estadísticas en tiempo real (ventas, productos, usuarios)
- Gestión completa de órdenes con filtros
- Control de inventario
- Reportes de ventas
- Gestión de productos con imágenes

## Solución de Problemas

### Error: "Missing required environment variables"

**Solución:**
- Verifica que todas las variables de entorno obligatorias estén configuradas en Vercel
- Asegúrate de haber hecho "Redeploy" después de agregar las variables
- Verifica que las variables estén en el entorno correcto (Production/Preview/Development)

### Error: "SUPABASE_SERVICE_ROLE_KEY is not set"

**Solución:**
- Agrega la variable `SUPABASE_SERVICE_ROLE_KEY` en Vercel
- Esta clave se encuentra en Supabase → Settings → API → service_role key
- Haz "Redeploy" después de agregarla

### Error: "Cannot connect to Supabase"

**Solución:**
- Verifica que `SUPABASE_URL` y `SUPABASE_KEY` sean correctos
- Verifica que el proyecto de Supabase esté activo
- Revisa los logs en Vercel para más detalles
- Ejecuta `npm run test:system` localmente para probar la conexión

### Frontend no se conecta al Backend

**Solución:**
- Verifica que `ALLOWED_ORIGINS` incluya la URL exacta del frontend
- Asegúrate de que no haya espacios en la lista de URLs permitidas
- Verifica que `FRONTEND_URL` esté configurada correctamente
- Revisa la consola del navegador para ver errores específicos de CORS

### Error 500 en producción

**Solución:**
1. Ve a Vercel → Deployments → Selecciona el deployment fallido
2. Revisa los logs para identificar el error
3. Verifica que todas las variables de entorno estén configuradas
4. Verifica que la base de datos esté accesible
5. Verifica que el script SQL se haya ejecutado correctamente en Supabase

## Recursos Adicionales

### Documentación

- [Vercel Documentation](https://vercel.com/docs)
- [Supabase Documentation](https://supabase.com/docs)
- [Express.js Documentation](https://expressjs.com/)
- [Swagger Documentation](https://swagger.io/docs/)

### Soporte

Si encuentras problemas que no están cubiertos en este README:

1. Revisa los logs en Vercel
2. Revisa la documentación de Swagger en `/api-docs`
3. Consulta el `MANUAL_INSTALACION.txt` en la raíz del proyecto
4. Verifica que todas las variables de entorno estén configuradas correctamente

---

**Silverthorne Holdings** - Sistema de e-commerce para servicios e insumos informáticos
