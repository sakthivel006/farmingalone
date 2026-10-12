# 🌱 Farmer Produce Marketing Platform (PT1)

A full-stack web application that simulates a **Farmer Produce Marketplace** with an integrated **AWS Cloud Architecture Simulator**. Farmers can list produce, buyers can place orders, and admins can manage the platform — all while viewing simulated AWS infrastructure telemetry.

---

## 📁 Project Structure

```
PT1/
├── index.js                   # Root entry point (delegates to server)
├── package.json               # Project config & dependencies
│
├── public/                    # Frontend (served as static files)
│   ├── index.html             # Single Page Application (SPA) shell
│   ├── css/                   # Stylesheets
│   ├── js/
│   │   ├── app.js             # Main frontend app logic & routing
│   │   ├── api.js             # API helper functions (fetch wrapper)
│   │   ├── state.js           # Global frontend state management
│   │   ├── marketplace.js     # Marketplace / buyer view
│   │   ├── farmer.js          # Farmer dashboard view
│   │   ├── admin.js           # Admin panel view
│   │   └── aws.js             # AWS Cloud Simulator view
│   └── assets/                # Images and static assets
│
└── server/                    # Backend (Node.js + Express)
    ├── server.js              # Express server setup & middleware
    ├── data/
    │   ├── db.json            # JSON flat-file database (auto-generated)
    │   ├── store.js           # DB read/write helpers
    │   └── fap.js             # Seed/initial data
    └── routes/
        ├── auth.js            # Authentication routes (register/login/JWT)
        ├── products.js        # Product listing CRUD routes
        ├── orders.js          # Order management routes
        ├── admin.js           # Admin-only routes
        └── aws.js             # AWS simulation telemetry routes
```

---

## 🚀 How to Run the Project Manually

### ✅ Prerequisites

Make sure you have the following installed:

| Tool    | Version  | Download Link                |
|---------|----------|------------------------------|
| Node.js | v18+     | https://nodejs.org/          |
| npm     | v9+      | (comes bundled with Node.js) |

To check if they are installed, open a terminal and run:
```bash
node -v
npm -v
```

---

### 📦 Step 1: Install Dependencies

Open a terminal (PowerShell or CMD) in the project folder and run:

```bash
cd "C:\Users\sakth\OneDrive\Desktop\PT1"
npm install
```

This will install all required packages listed in `package.json`:
- `express` — web server framework
- `cors` — cross-origin request support
- `bcryptjs` — password hashing
- `jsonwebtoken` — JWT auth tokens
- `multer` — file upload handling

---

### ▶️ Step 2: Start the Server

**Option A — Using npm start (recommended):**
```bash
npm start
```

**Option B — Using npm dev:**
```bash
npm run dev
```

**Option C — Directly with Node:**
```bash
node server/server.js
```

**Option D — Via the root entry point:**
```bash
node index.js
```

All of the above do the same thing — they start `server/server.js`.

---

### 🌐 Step 3: Open the App in Your Browser

Once the server is running, you'll see this in the terminal:

```
====================================================
🌱 Farmer Produce Marketing Platform is running!
🚀 Local URL: http://localhost:5000
☁️  AWS Cloud Architecture Simulator: http://localhost:5000/#aws
====================================================
```

Open your browser and go to:

| Page                       | URL                               |
|----------------------------|-----------------------------------|
| 🏠 Main App / Marketplace  | http://localhost:5000             |
| ☁️ AWS Simulator Dashboard | http://localhost:5000/#aws        |
| 🩺 Health Check (API)      | http://localhost:5000/api/health  |

---

## 🔑 API Endpoints

| Method | Endpoint              | Description                   | Auth Required |
|--------|-----------------------|-------------------------------|---------------|
| GET    | `/api/health`         | Server health check           | ❌            |
| POST   | `/api/auth/register`  | Register a new user           | ❌            |
| POST   | `/api/auth/login`     | Login and get JWT token       | ❌            |
| GET    | `/api/products`       | List all products             | ❌            |
| POST   | `/api/products`       | Create a new product          | ✅ (Farmer)   |
| PUT    | `/api/products/:id`   | Update a product              | ✅ (Farmer)   |
| DELETE | `/api/products/:id`   | Delete a product              | ✅ (Farmer)   |
| GET    | `/api/orders`         | Get orders for logged-in user | ✅            |
| POST   | `/api/orders`         | Place a new order             | ✅ (Buyer)    |
| GET    | `/api/admin/users`    | List all users                | ✅ (Admin)    |
| GET    | `/api/aws/telemetry`  | Get AWS simulation telemetry  | ❌            |

---

## 🗄️ Database

This project uses a **JSON flat-file database** (`server/data/db.json`).

- It is **auto-created** on first run using seed data from `fap.js`.
- No external database (MySQL, MongoDB, etc.) is needed.
- All data is stored locally in `db.json`.

> ⚠️ **Note:** If you delete `db.json`, it will be recreated with fresh seed data on next server start.

---

## 🛠️ Environment Variables (Optional)

The server uses these environment variables (with defaults):

| Variable | Default | Description             |
|----------|---------|-------------------------|
| `PORT`   | `5000`  | Port the server runs on |

To use a custom port, run:
```powershell
# PowerShell
$env:PORT=3000; node server/server.js
```
```cmd
# CMD
set PORT=3000 && node server/server.js
```

---

## 🧪 Troubleshooting

### ❌ `npm install` fails
- Make sure you're in the correct folder: `C:\Users\sakth\OneDrive\Desktop\PT1`
- Try deleting `node_modules` and `package-lock.json`, then run `npm install` again

### ❌ Port 5000 is already in use
- Change the port using the `PORT` environment variable (see above)
- Or find and kill the process using port 5000:
  ```powershell
  netstat -ano | findstr :5000
  taskkill /PID <PID_NUMBER> /F
  ```

### ❌ Cannot find module error
- Run `npm install` again to ensure all dependencies are installed

### ❌ The page loads but shows a blank screen
- Open browser DevTools (F12) → Console tab for error details
- Make sure the server is running and you're visiting `http://localhost:5000`

---

## 👤 User Roles

| Role       | Capabilities                                              |
|------------|-----------------------------------------------------------|
| **Buyer**  | Browse products, place orders, view order history         |
| **Farmer** | List produce, manage own products, view incoming orders   |
| **Admin**  | Manage all users, products, orders; view AWS telemetry    |

---

## ☁️ AWS Cloud Simulator

The app includes a simulated AWS architecture dashboard at `http://localhost:5000/#aws` that visualizes:

- **EC2** — Simulated compute instance metrics
- **App Runner / Elastic Beanstalk** — Deployment status
- **CloudWatch** — Real-time request log stream (actual API calls are logged here)
- **ALB (Application Load Balancer)** — Request routing simulation
- **RDS** — Simulated database metrics
- **S3** — Simulated storage bucket info

> This is a simulation for educational/demo purposes — no real AWS credentials are needed.

---

## 📜 License

ISC — Free to use and modify.
