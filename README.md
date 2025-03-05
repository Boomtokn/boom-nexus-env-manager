# **Boom Nexus Env Manager**  

## **📌 Overview**  
The **Boom Nexus Env Manager** is a secure and scalable environment variable management system for Boom Nexus applications. It enables administrators to **store, manage, and retrieve** environment variables efficiently while maintaining **high security, role-based access control (RBAC), and seamless API integration**.  

---

## **🚀 Features**  

✅ **Secure Storage** – Encrypts environment variables using AES encryption in PostgreSQL.  
✅ **Role-Based Access Control (RBAC)** – Implements JWT authentication for user-specific access control.  
✅ **Admin Dashboard** – User-friendly interface for managing variables effortlessly (React-based).  
✅ **RESTful API** – Exposes secure endpoints for programmatic access and automation.  
✅ **Logging & Auditing** – Tracks changes, access logs, and permissions for security compliance.  
✅ **Multi-Environment Support** – Manages variables across development, staging, and production environments.  

---

## **📂 Project Structure**  
boom-nexus-env-manager/
│── backend/                # Node.js & Express API
│   ├── config/             # Configuration files
│   ├── controllers/        # API Controllers
│   ├── models/             # Database Models
│   ├── routes/             # API Routes
│   ├── middleware/         # Auth & Security Middleware
│   ├── utils/              # Utility Functions
│   ├── server.js           # Main Server Entry
│   └── .env.example        # Example environment variables
│
│── frontend/               # React Admin Dashboard
│   ├── src/
│   │   ├── components/     # UI Components
│   │   ├── pages/          # Dashboard Pages
│   │   ├── services/       # API Integration
│   │   ├── App.js          # Main App Entry
│   │   ├── index.js        # React DOM Renderer
│   └── public/             # Static Files
│
│── database/               # PostgreSQL Database Scripts
│── docs/                   # Documentation & API Reference
│── README.md               # Project Documentation
│── LICENSE                 # MIT License
│── package.json            # Backend Dependencies
│── frontend/package.json   # Frontend Dependencies

---

## **🛠️ Installation & Setup**  

### **1️⃣ Clone the Repository**  
```sh
git clone https://github.com/Boomtoknlab/boom-nexus-env-manager.git
cd boom-nexus-env-manager

2️⃣ Backend Setup
cd backend
cp .env.example .env   # Configure environment variables
npm install            # Install dependencies
npm run dev            # Start backend in development mode

3️⃣ Frontend Setup
cd ../frontend
npm install            # Install dependencies
npm start              # Run React dashboard

4️⃣ Database Setup (PostgreSQL)
# Open PostgreSQL and create the database
CREATE DATABASE boom_env_manager;

# Run migrations
npx sequelize-cli db:migrate

🔑 API Authentication

Boom Nexus Env Manager uses JWT authentication. You need to include a token in the request header:
Authorization: Bearer YOUR_JWT_TOKEN

🔹 Example API Calls

1️⃣ Get All Environment Variables
GET /api/env-variables

2️⃣ Add a New Environment Variable
POST /api/env-variables
Content-Type: application/json
Authorization: Bearer YOUR_JWT_TOKEN

{
  "key": "DATABASE_URL",
  "value": "postgres://user:password@localhost:5432/dbname"
}

4️⃣ Delete an Environment Variable
DELETE /api/env-variables/:id
Authorization: Bearer YOUR_JWT_TOKEN

🔧 Backend Code Examples

server.js (Backend Entry Point)
import express from 'express';
import dotenv from 'dotenv';
import cors from 'cors';
import envRoutes from './routes/envRoutes.js';
import authMiddleware from './middleware/auth.js';

dotenv.config();
const app = express();

app.use(cors());
app.use(express.json());

app.use('/api/env-variables', authMiddleware, envRoutes);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));

models/EnvVariable.js (Database Model for Environment Variables)
import express from 'express';
import { getEnvVariables, addEnvVariable, updateEnvVariable, deleteEnvVariable } from '../controllers/envController.js';

const router = express.Router();

router.get('/', getEnvVariables);
router.post('/', addEnvVariable);
router.put('/:id', updateEnvVariable);
router.delete('/:id', deleteEnvVariable);

export default router;

controllers/envController.js (Controller Functions for Environment Variables)
import EnvVariable from '../models/EnvVariable.js';

export const getEnvVariables = async (req, res) => {
  try {
    const variables = await EnvVariable.findAll();
    res.json(variables);
  } catch (error) {
    res.status(500).json({ error: 'Failed to fetch environment variables' });
  }
};

export const addEnvVariable = async (req, res) => {
  try {
    const { key, value } = req.body;
    const variable = await EnvVariable.create({ key, value });
    res.status(201).json(variable);
  } catch (error) {
    res.status(500).json({ error: 'Failed to add environment variable' });
  }
};

export const updateEnvVariable = async (req, res) => {
  try {
    const { id } = req.params;
    const { value } = req.body;
    await EnvVariable.update({ value }, { where: { id } });
    res.json({ message: 'Updated successfully' });
  } catch (error) {
    res.status(500).json({ error: 'Failed to update environment variable' });
  }
};

export const deleteEnvVariable = async (req, res) => {
  try {
    const { id } = req.params;
    await EnvVariable.destroy({ where: { id } });
    res.json({ message: 'Deleted successfully' });
  } catch (error) {
    res.status(500).json({ error: 'Failed to delete environment variable' });
  }
};

🌍 Frontend (React Dashboard) Example

services/api.js (API Service for Environment Variables)
import axios from 'axios';

const API_URL = 'http://localhost:5000/api/env-variables';

export const getEnvVariables = async () => {
  const response = await axios.get(API_URL);
  return response.data;
};

export const addEnvVariable = async (data) => {
  const response = await axios.post(API_URL, data);
  return response.data;
};

export const updateEnvVariable = async (id, data) => {
  const response = await axios.put(`${API_URL}/${id}`, data);
  return response.data;
};

export const deleteEnvVariable = async (id) => {
  const response = await axios.delete(`${API_URL}/${id}`);
  return response.data;
};

📜 License

This project is open-source and available under the MIT License.

📩 Contact

For inquiries, reach out via GitHub Issues.
