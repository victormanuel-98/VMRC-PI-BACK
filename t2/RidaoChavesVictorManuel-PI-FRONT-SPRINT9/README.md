# FitFood - Aplicación de Recetas Saludables

## 📋 Descripción del Proyecto

FitFood es una aplicación web fullstack para crear, gestionar y descubrir recetas saludables. Permite a los usuarios crear recetas con información nutricional detallada, gestionar su perfil, y explorar recetas por categorías.

---

## 🚀 Características Implementadas

### Sprint 9 - Integración Backend y Funcionalidades Avanzadas

#### ✅ Sistema de Autenticación
- Registro e inicio de sesión con JWT
- Persistencia de sesión en localStorage
- Rutas protegidas con middleware de autenticación
- Contexto global de autenticación (AuthContext)

#### ✅ Gestión de Recetas
- **Crear Recetas**: Formulario completo con búsqueda inteligente de ingredientes
  - Autocompletado de ingredientes (70+ ingredientes en base de datos)
  - Validación de ingredientes (requiere selección de ID)
  - Cálculo automático de calorías
  - Subida de imágenes a Cloudinary
  - Categorías: desayuno, almuerzo, cena, merienda
  
- **Ver Recetas**: Vista detallada con información completa
  - Carga dinámica desde API con parámetro ID
  - Tabla nutricional detallada
  - Sistema de valoraciones (estrellas 1-5)
  - Funcionalidad de favoritos
  - Ingredientes con cantidades en gramos

- **Mis Recetas**: Listado personal de recetas creadas
  - Carga automática de recetas del usuario
  - Vista en tarjetas con información resumida
  - Botones de ver detalle y eliminar
  - Estados de carga, error y vacío

- **Recetas por Categoría**: Exploración de recetas filtradas
  - BreakfastRecipes: Filtrado automático por categoría "desayuno"
  - Estados de carga, error y vacío
  - Badges de calorías y dificultad
  - Navegación dinámica a detalle

#### ✅ Gestión de Perfil de Usuario
- **Obtener Perfil**: Carga automática de datos del usuario
- **Actualizar Perfil**: Edición completa de información personal
  - Campos: nombre, apellidos, usuario, email, teléfono, notificaciones
  - Cambio de contraseña con verificación de contraseña actual
  - Validación de contraseña fuerte (8+ caracteres, mayúsculas, números, símbolos)
  - Subida de foto de perfil a Cloudinary
  - Validación de unicidad de email
  - Autorización: usuario solo puede editar su propio perfil (excepto admin)

#### ✅ Backend - API REST
- **Node.js + Express**: Servidor HTTP con rutas RESTful
- **MongoDB + Mongoose**: Base de datos NoSQL con modelos definidos
- **Cloudinary**: Almacenamiento de imágenes
- **JWT**: Autenticación basada en tokens
- **Bcrypt**: Encriptación de contraseñas
- **CORS**: Configurado para desarrollo local

**Endpoints implementados:**
```
POST   /api/auth/registro          - Registro de usuario
POST   /api/auth/login             - Inicio de sesión
GET    /api/auth/verificar         - Verificar token

GET    /api/usuarios/:id           - Obtener perfil
PUT    /api/usuarios/:id           - Actualizar perfil

GET    /api/recetas                - Listar recetas (filtros: categoria, dificultad)
GET    /api/recetas/:id            - Obtener receta por ID
POST   /api/recetas                - Crear receta
PUT    /api/recetas/:id            - Actualizar receta
DELETE /api/recetas/:id            - Eliminar receta

GET    /api/ingredientes           - Buscar ingredientes

POST   /api/favoritos              - Agregar favorito
GET    /api/favoritos              - Listar favoritos
DELETE /api/favoritos/:id          - Eliminar favorito

POST   /api/valoraciones           - Crear valoración
GET    /api/valoraciones/:id       - Obtener valoraciones de receta

POST   /api/upload/receta          - Subir imagen de receta
POST   /api/upload/perfil          - Subir imagen de perfil
```

---

## 🏗️ Arquitectura de Navegación

### Layouts Implementados

#### **PublicLayout**
- **Propósito**: Layout minimalista para páginas de autenticación
- **Características**: Sin Header, Navigation ni Footer
- **Rutas asociadas**: Login, Registro, 404, 403

#### **PrivateLayout**
- **Propósito**: Layout completo para usuarios autenticados
- **Características**: Incluye Header, Navigation y Footer
- **Rutas asociadas**: Todas las rutas protegidas (Inicio, Perfil, Recetas, etc.)

### Sistema de Autenticación

**AuthContext** - Contexto global de autenticación
- Gestión de sesión con `useState` y `useEffect`
- Persistencia en `localStorage`
- Métodos: `login()`, `logout()`, `isAuthenticated`, `user`, `loading`

**ProtectedRoute** - Componente de protección de rutas
- Verifica autenticación antes de renderizar
- Muestra pantalla de carga mientras verifica sesión
- Redirige a `/login` si no hay sesión activa

---

## Mapa de Rutas

### Rutas Públicas

| Ruta | Componente | Descripción | Layout | Protegida |
|------|-----------|-------------|---------|-----------|
| `/` | Navigate | Redirección a /login | - | ❌ |
| `/login` | Login | Página de inicio de sesión | Public | ❌ |
| `/registro` | Register | Formulario de registro de usuario | Public | ❌ |
| `/forbidden` | Forbidden | Página 403 - Sin permisos | Public | ❌ |
| `*` | NotFound | Página 404 - No encontrada | Public | ❌ |

### Rutas Privadas (Protegidas)

| Ruta | Componente | Descripción | Layout | API Integrada |
|------|-----------|-------------|---------|---------------|
| `/inicio` | Home | Página principal con hero y carrusel | Private | ❌ |
| `/perfil` | Profile | Perfil de usuario (edición completa) | Private | ✅ |
| `/recetas` | MyRecipes | Listado de recetas personales | Private | ✅ |
| `/receta/:id` | RecipeDetail | Vista detallada de una receta | Private | ✅ |
| `/recetas/crear` | CreateRecipe | Formulario de creación con autocomplete | Private | ✅ |
| `/contacto` | Contact | Formulario de contacto | Private | ❌ |
| `/ajustes` | Settings | Configuración de la aplicación | Private | ❌ |
| `/platos/desayuno` | BreakfastRecipes | Recetas de desayuno | Private | ✅ |
| `/platos/almuerzo` | NotFound | (Por implementar) | Private | ❌ |
| `/platos/cena` | NotFound | (Por implementar) | Private | ❌ |
| `/platos/otros` | NotFound | (Por implementar) | Private | ❌ |

---

## 🛠️ Tecnologías Utilizadas

### Frontend
- **React 19.2.0**: Framework UI con hooks
- **React Router v7.12.0**: Navegación con parámetros dinámicos
- **Vite 7.3.1**: Build tool y dev server
- **CSS3**: Estilos personalizados responsive
- **Context API**: Estado global de autenticación

### Backend
- **Node.js + Express**: Servidor API REST
- **MongoDB + Mongoose**: Base de datos NoSQL
- **JWT (jsonwebtoken)**: Autenticación
- **Bcrypt**: Hash de contraseñas
- **Cloudinary**: Almacenamiento de imágenes
- **Validator.js**: Validación de emails
- **CORS**: Cross-Origin Resource Sharing
- **express-async-errors**: Manejo de errores asíncronos

---

## 📁 Estructura del Proyecto

```
RidaoChavesVictorManuel-PI-FRONT-SPRINT9/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   │   ├── authController.js
│   │   │   ├── userController.js          ⭐ NUEVO
│   │   │   ├── recipeController.js
│   │   │   ├── ingredientController.js
│   │   │   ├── favoriteController.js
│   │   │   ├── ratingController.js
│   │   │   └── uploadController.js
│   │   ├── routes/
│   │   │   ├── authRoutes.js
│   │   │   ├── userRoutes.js              ⭐ NUEVO
│   │   │   ├── recipeRoutes.js
│   │   │   ├── ingredientRoutes.js
│   │   │   ├── favoriteRoutes.js
│   │   │   ├── ratingRoutes.js
│   │   │   └── uploadRoutes.js
│   │   ├── models/
│   │   │   ├── User.js
│   │   │   ├── Recipe.js
│   │   │   ├── Ingredient.js
│   │   │   ├── Favorite.js
│   │   │   └── Rating.js
│   │   ├── middleware/
│   │   │   └── auth.js
│   │   ├── config/
│   │   │   ├── db.js
│   │   │   └── cloudinary.js
│   │   ├── utils/
│   │   │   └── seedIngredients.js
│   │   ├── app.js
│   │   └── server.js
│   ├── .env
│   ├── .gitignore
│   └── package.json
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Header.jsx
│   │   │   ├── Navigation.jsx
│   │   │   ├── Footer.jsx
│   │   │   ├── Breadcrumbs.jsx
│   │   │   └── ProtectedRoute.jsx
│   │   ├── context/
│   │   │   └── AuthContext.jsx
│   │   ├── layouts/
│   │   │   ├── PublicLayout.jsx
│   │   │   └── PrivateLayout.jsx
│   │   ├── pages/
│   │   │   ├── Login.jsx
│   │   │   ├── Register.jsx
│   │   │   ├── Home.jsx
│   │   │   ├── Profile.jsx                ⭐ REESCRITO
│   │   │   ├── MyRecipes.jsx              ⭐ REESCRITO
│   │   │   ├── RecipeDetail.jsx           ⭐ REESCRITO
│   │   │   ├── CreateRecipe.jsx           ⭐ REESCRITO
│   │   │   ├── BreakfastRecipes.jsx       ⭐ REESCRITO
│   │   │   ├── Contact.jsx
│   │   │   ├── Settings.jsx
│   │   │   ├── NotFound.jsx
│   │   │   └── Forbidden.jsx
│   │   ├── services/
│   │   │   └── api.js                     ⭐ AMPLIADO
│   │   ├── styles/
│   │   │   └── styles.css
│   │   ├── App.jsx                        ⭐ MODIFICADO
│   │   └── main.jsx
│   ├── public/
│   ├── .gitignore
│   ├── package.json
│   └── vite.config.js
│
├── .gitignore
└── README.md

### 1. **Separación de Layouts**
**Decisión**: Crear dos layouts diferenciados (Public/Private)

**Justificación**:
- Mejora la experiencia de usuario al no mostrar navegación innecesaria en login/registro
- Cumple con el patrón UX de separar flujos públicos y privados
- Facilita mantenimiento al centralizar cambios de UI por tipo de ruta
- Optimiza rendimiento al no cargar componentes innecesarios

### 2. **Context API para Autenticación**
**Decisión**: Usar React Context en lugar de prop drilling

**Justificación**:
- Estado global accesible desde cualquier componente
- Evita pasar props por múltiples niveles
- Facilita escalabilidad (preparado para agregar Redux si es necesario)
- Persistencia con localStorage para mantener sesión

### 3. **ProtectedRoute como Componente Wrapper**
**Decisión**: Componente reutilizable que envuelve rutas privadas

**Justificación**:
- DRY (Don't Repeat Yourself) - evita duplicar lógica de protección
- Centraliza lógica de redirección
- Fácil de mantener y testear
- Muestra estado de carga mientras verifica autenticación

### 4. **Estados de Pantalla (Loading/Empty/Error)**
**Decisión**: Implementar estados explícitos en componentes clave

**Justificación**:
- Mejora UX al informar al usuario del estado de la aplicación
- Cumple con requisitos del Sprint 8
- Prepara la app para integración con API real
- Reduce frustración del usuario con feedback visual

### 5. **Estructura de Carpetas**
```
src/
├── components/        # Componentes reutilizables
│   ├── Header.jsx
│   ├── Navigation.jsx
│   ├── Footer.jsx
│   └── ProtectedRoute.jsx
├── context/          # Contextos de React
│   └── AuthContext.jsx
├── layouts/          # Layouts de página
│   ├── PublicLayout.jsx
│   └── PrivateLayout.jsx
├── pages/            # Páginas/Vistas
│   ├── Login.jsx
│   ├── Register.jsx
│   ├── Home.jsx
│   ├── Profile.jsx
│   ├── MyRecipes.jsx
│   ├── RecipeDetail.jsx
│   ├── CreateRecipe.jsx
│   ├── Contact.jsx
│   ├── Settings.jsx
│   ├── BreakfastRecipes.jsx
│   ├── NotFound.jsx
│   └── Forbidden.jsx
└── App.jsx           # Router principal
```

---

## 🔑 Decisiones Técnicas
### 6. **Búsqueda Inteligente de Ingredientes**
**Decisión**: Implementar autocompletado con búsqueda en tiempo real

**Justificación**:
- UX mejorada: usuario no necesita recordar nombres exactos
- Prevención de errores: solo se pueden seleccionar ingredientes válidos
- Integración con DB: 70+ ingredientes precargados con datos nutricionales
- Validación en frontend: verifica que se haya seleccionado un ID válido

### 7. **Rutas Dinámicas con Parámetros**
**Decisión**: Usar `/receta/:id` en lugar de `/receta` estático

**Justificación**:
- Permite compartir enlaces directos a recetas específicas
- Facilita navegación desde listados
- Preparado para SEO en producción
- useParams() hook de React Router simplifica la extracción del ID

### 8. **Gestión de Imágenes con Cloudinary**
**Decisión**: Usar servicio externo en lugar de almacenamiento local

**Justificación**:
- Escalabilidad: no satura el servidor con archivos
- CDN global: tiempos de carga optimizados
- Transformaciones automáticas: resize, optimización, formatos modernos
- Backup automático y alta disponibilidad

### 9. **Validaciones en Múltiples Capas**
**Decisión**: Validar tanto en frontend como en backend

**Justificación**:
- Frontend: feedback inmediato al usuario (UX)
- Backend: seguridad y consistencia de datos
- Doble validación de contraseñas: actual + nueva
- Validación de unicidad de email en base de datos

---

## 🚦 Instrucciones de Ejecución

### Prerrequisitos
- Node.js v18 o superior
- MongoDB Atlas (cuenta gratuita)
- Cloudinary (cuenta gratuita)

### Configuración del Backend

1. **Instalar dependencias**:
```bash
cd backend
npm install
```

2. **Configurar variables de entorno**:
Crear archivo `.env` en `/backend`:
```env
PORT=5000
MONGO_URI=mongodb+srv://tu-usuario:tu-password@cluster.mongodb.net/fitfood
JWT_SECRET=tu-clave-secreta-super-segura
JWT_EXPIRE=7d
CLOUDINARY_CLOUD_NAME=tu-cloud-name
CLOUDINARY_API_KEY=tu-api-key
CLOUDINARY_API_SECRET=tu-api-secret
CORS_ORIGIN=http://localhost:5173
```

3. **Iniciar servidor**:
```bash
npm run dev
```
El servidor estará en [http://localhost:5000](http://localhost:5000)

### Configuración del Frontend

1. **Instalar dependencias**:
```bash
cd frontend
npm install
```

2. **Configurar variables de entorno** (opcional):
Crear archivo `.env` en `/frontend`:
```env
VITE_API_URL=http://localhost:5000/api
```

3. **Iniciar aplicación**:
```bash
npm run dev
```
La app estará en [http://localhost:5173](http://localhost:5173)

### Build para Producción

**Frontend**:
```bash
cd frontend
npm run build
```

**Backend**:
```bash
cd backend
npm start
```

---

## 👤 Credenciales de Prueba

**Opción 1 - Crear cuenta nueva**:
- Ir a `/registro` y completar el formulario

**Opción 2 - Usar cuenta de prueba** (si existe en tu DB):
- Usuario: `victor_98`
- Contraseña: `Admin123!`

---

## 📊 Modelos de Datos

### User (Usuario)
```javascript
{
  usuario: String (único, requerido),
  email: String (único, requerido),
  contrasena: String (hasheada, requerida),
  nombre: String,
  apellidos: String,
  telefono: String,
  foto: String (URL Cloudinary),
  rol: String (default: 'usuario'),
  notificaciones: Boolean (default: true),
  visibilidad: String (default: 'publica')
}
```

### Recipe (Receta)
```javascript
{
  nombre: String (requerido),
  descripcionCorta: String (requerido),
  descripcionLarga: String,
  imagen: String (URL Cloudinary, requerida),
  categoria: String (requerido),
  dificultad: String (requerido),
  tiempoPreparacion: Number (minutos),
  ingredientes: [{
    ingrediente: ObjectId (ref: Ingredient),
    cantidad: Number (gramos)
  }],
  calorias: Number (calculadas),
  proteinas: Number,
  carbohidratos: Number,
  grasas: Number,
  usuario: ObjectId (ref: User),
  valoracionPromedio: Number (default: 0)
}
```

### Ingredient (Ingrediente)
```javascript
{
  nombre: String (único, requerido),
  categoria: String,
  calorias: Number (por 100g),
  proteinas: Number,
  carbohidratos: Number,
  grasas: Number
}
```

---

## 🎯 Próximos Pasos (Pendientes)

- [ ] Implementar categorías adicionales (Almuerzo, Cena, Merienda)
- [ ] Sistema de historial de consumo diario
- [ ] Dashboard con estadísticas nutricionales
- [ ] Búsqueda avanzada de recetas por ingredientes
- [ ] Sistema de comentarios en recetas
- [ ] Compartir recetas en redes sociales
- [ ] Modo oscuro
- [ ] Notificaciones push
- [ ] PWA (Progressive Web App)
- [ ] Internacionalización (i18n)

---

## 📸 Capturas de Pantalla

## 📸 Capturas de Pantalla

### Login
![Login](./frontend/public/images/login.png)

### Home
![Home](./frontend/public/images/home.png)

### Perfil de Usuario
![Profile](./frontend/public/images/profile.png)

---

## 📝 Notas de Desarrollo

### Sprint 9 - Cambios Principales

1. **CreateRecipe.jsx**: Reescrito completamente con búsqueda de ingredientes
2. **RecipeDetail.jsx**: Integración completa con API usando `useParams()`
3. **Profile.jsx**: Sistema completo de edición de perfil con cambio de contraseña
4. **BreakfastRecipes.jsx**: Migrado de datos estáticos a API con filtros
5. **App.jsx**: Ruta actualizada de `/receta` a `/receta/:id`
6. **api.js**: Agregadas funciones `obtenerPerfilUsuario` y `actualizarPerfilUsuario`
7. **userController.js**: Nuevos endpoints `obtenerPerfil` y `actualizarPerfil`
8. **userRoutes.js**: Nuevo archivo de rutas para usuarios
9. **styles.css**: Agregados estilos para mensajes de error/éxito y badges nutricionales

### Mejoras de UX Implementadas
- Loading states en todas las páginas con datos dinámicos
- Error states con botón de reintentar
- Empty states con call-to-action
- Mensajes de feedback para operaciones CRUD
- Validaciones en tiempo real
- Imágenes con fallback SVG

---

## 👨‍💻 Autor

**Victor Manuel Ridao Chaves**  
Proyecto Integrado - Sprint 9  
Desarrollo de Aplicaciones Web

---

## 📄 Licencia

Este proyecto es parte de un trabajo académico.
