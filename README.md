# meloeché

Este proyecto es una plataforma web que permite a los usuarios gestionar sus semestres académicos, asignaturas y calificaciones con autenticación y almacenamiento de datos seguro.

## **Arquitectura del Proyecto**

### **1. Tecnologías Utilizadas**

#### **Frontend (Next.js + TypeScript)**
- Next.js (SSR e ISR para optimización de carga de datos)
- TypeScript (tipado fuerte para mayor seguridad)
- Tailwind CSS y Shadcn/UI (diseño rápido y modular)
- React Context (gestión de estado global)
- NextAuth.js (gestión de autenticación con JWT y cookies seguras)

#### **Backend (Node.js + Express con TypeScript)**
- Node.js con Express (framework ligero y eficiente para APIs)
- Zod (validación de entrada de datos)
- bcrypt (encriptación de contraseñas)
- jsonwebtoken (autenticación con JWT)

#### **Base de Datos (MongoDB con Mongoose)**
- MongoDB (NoSQL, flexible y escalable)
- Mongoose (ODM para definir esquemas y relaciones entre datos)

#### **Infraestructura y Despliegue**
- VPS con Ubuntu 24.04 LTS
- Nginx (como proxy inverso)
- PM2 (para gestionar procesos de Node.js)
- MongoDB autohospedado en la VPS

## **2. Estructura del Proyecto**

```
/ (Raíz del proyecto)
├── backend/                 # API en Node.js con Express
│   ├── controllers/         # Lógica de negocio
│   ├── models/              # Modelos de datos con Mongoose
│   ├── routes/              # Endpoints de la API
│   ├── middleware/          # Middleware (autenticación, validaciones, etc.)
│   ├── config/              # Configuración de la base de datos y variables de entorno
│   ├── index.ts             # Punto de entrada del backend
│
├── frontend/                # Aplicación en Next.js
│   ├── components/          # Componentes reutilizables
│   ├── pages/               # Páginas de la aplicación
│   ├── context/             # Gestión de estado global (Zustand o Context API)
│   ├── styles/              # Estilos globales
│   ├── utils/               # Funciones auxiliares
│   ├── next.config.js       # Configuración de Next.js
│
├── .env                     # Variables de entorno
├── README.md                # Documentación del proyecto
```

## **3. Modelos de Base de Datos (MongoDB)**

```ts
const SemesterSchema = new Schema({
  userId: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  name: String,
  subjects: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Subject' }]
});

const SubjectSchema = new Schema({
  semesterId: { type: mongoose.Schema.Types.ObjectId, ref: 'Semester' },
  name: String,
  grades: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Grade' }]
});

const GradeSchema = new Schema({
  subjectId: { type: mongoose.Schema.Types.ObjectId, ref: 'Subject' },
  name: String,
  weight: Number,
  value: Number
});
```

## **4. Endpoints de la API**

| Método  | Endpoint             | Descripción                       |
|---------|----------------------|-----------------------------------|
| POST    | /auth/register       | Registrar usuario                 |
| POST    | /auth/login          | Iniciar sesión                    |
| GET     | /semesters           | Obtener todos los semestres       |
| POST    | /semesters           | Crear un nuevo semestre           |
| GET     | /semesters/:id       | Obtener un semestre y asignaturas |
| POST    | /subjects            | Crear una nueva asignatura        |
| GET     | /subjects/:id        | Obtener una asignatura            |
| POST    | /grades              | Agregar una calificación          |
