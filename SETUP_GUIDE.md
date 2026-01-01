# 🛠️ Setup Guide

## Hawassa University Student Union Election System
### Complete Installation & Configuration Guide

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Software Installation](#software-installation)
3. [Database Setup](#database-setup)
4. [Blockchain Setup](#blockchain-setup)
5. [MetaMask Configuration](#metamask-configuration)
6. [Project Installation](#project-installation)
7. [Smart Contract Deployment](#smart-contract-deployment)
8. [Running the Application](#running-the-application)
9. [Verification Steps](#verification-steps)

---

## 📦 Prerequisites

### Required Software

| Software | Minimum Version | Purpose | Download Link |
|----------|-----------------|---------|---------------|
| Node.js | v18.0.0+ | JavaScript runtime | [nodejs.org](https://nodejs.org/) |
| Python | 3.9+ | FastAPI backend | [python.org](https://python.org/) |
| MySQL | 8.0+ | Database server | [mysql.com](https://dev.mysql.com/downloads/) |
| Ganache | 2.7+ | Local blockchain | [trufflesuite.com](https://trufflesuite.com/ganache/) |
| MetaMask | Latest | Browser wallet | [metamask.io](https://metamask.io/) |
| Git | Latest | Version control | [git-scm.com](https://git-scm.com/) |
| VS Code | Latest | Code editor (optional) | [code.visualstudio.com](https://code.visualstudio.com/) |

### System Requirements

- **OS**: Windows 10/11, macOS 10.15+, or Linux
- **RAM**: 8 GB minimum (16 GB recommended)
- **Storage**: 2 GB free space
- **Network**: Stable internet connection

---

## 💻 Software Installation

### Step 1: Install Node.js

1. Download Node.js LTS from [nodejs.org](https://nodejs.org/)
2. Run the installer (include npm)
3. Verify installation:
   ```bash
   node --version    # Should show v18.x.x or higher
   npm --version     # Should show 9.x.x or higher
   ```

### Step 2: Install Python

1. Download Python from [python.org](https://python.org/downloads/)
2. **Important**: Check "Add Python to PATH" during installation
3. Verify installation:
   ```bash
   python --version  # Should show Python 3.9+
   pip --version     # Should show pip version
   ```

### Step 3: Install MySQL

#### Windows:
1. Download MySQL Installer from [mysql.com](https://dev.mysql.com/downloads/installer/)
2. Run installer, select "MySQL Server" and "MySQL Shell"
3. Set root password (remember this!)
4. Complete installation

#### Verify MySQL:
```bash
mysql --version
# or connect to MySQL
mysql -u root -p
```

### Step 4: Install Ganache

1. Download Ganache from [trufflesuite.com/ganache](https://trufflesuite.com/ganache/)
2. Install and run
3. Create a new workspace named "development"
4. **Important**: Note these settings:
   - RPC Server: `http://127.0.0.1:7545`
   - Network ID: `1337`

### Step 5: Install MetaMask Browser Extension

1. Go to [metamask.io](https://metamask.io/)
2. Click "Download" and add to your browser (Chrome/Firefox/Brave)
3. Create a new wallet or import existing
4. **Save your seed phrase securely!**

### Step 6: Install Truffle (Global)

```bash
npm install -g truffle
truffle version  # Verify installation
```

---

## 🗄️ Database Setup

### Step 1: Start MySQL Server

Ensure MySQL service is running:
```bash
# Windows (PowerShell as Admin)
net start mysql

# Or check MySQL Workbench / MySQL Shell
```

### Step 2: Create Database

Open MySQL Shell or command line:

```bash
mysql -u root -p
```

Enter your root password, then run:

```sql
-- Create the database
CREATE DATABASE hawassa_election_db;

-- Switch to the database
USE hawassa_election_db;

-- Exit MySQL
EXIT;
```

### Step 3: Import Database Schema

Navigate to the project folder and run:

```bash
mysql -u root -p hawassa_election_db < database_schema.sql
```

Or use MySQL Shell:
```sql
SOURCE D:/path/to/project/database_schema.sql;
```

### Step 4: Verify Database Tables

```bash
mysql -u root -p
```

```sql
USE hawassa_election_db;
SHOW TABLES;
```

Expected output:
```
+-------------------------------+
| Tables_in_hawassa_election_db |
+-------------------------------+
| admins                        |
| audit_logs                    |
| candidates                    |
| elections                     |
| results                       |
| students                      |
| votes                         |
+-------------------------------+
```

### Step 5: Verify Sample Data

```sql
SELECT student_id, full_name, role FROM students;
```

Expected output:
```
+------------+----------------------+-------+
| student_id | full_name            | role  |
+------------+----------------------+-------+
| ADMIN001   | System Administrator | admin |
| 2838/13    | Abel Abera           | voter |
| 0019/13    | Behailu Bekele       | voter |
| 0044/13    | John Dawit           | voter |
| 0056/13    | Mintesnot Abebe      | voter |
| 0069/13    | Serawit Yoseph       | voter |
+------------+----------------------+-------+
```

---

## ⛓️ Blockchain Setup

### Step 1: Configure Ganache

1. Open Ganache
2. Click "New Workspace" (or "Quickstart")
3. Name it: `Hawassa Election`
4. Go to Settings (⚙️):

   **Workspace Tab:**
   - Add truffle-config.js from project folder

   **Server Tab:**
   - Hostname: `127.0.0.1`
   - Port: `7545`
   - Network ID: `1337`

5. Click "Save and Restart"

### Step 2: Note Account Information

From Ganache main screen:
- **Account 0 Address**: `0x...` (This will be the admin)
- Click 🔑 key icon to reveal **Private Key**
- Copy this for MetaMask import

---

## 🦊 MetaMask Configuration

### Step 1: Add Custom Network

1. Open MetaMask extension
2. Click network dropdown (top center)
3. Click "Add Network" → "Add network manually"
4. Enter:

   | Field | Value |
   |-------|-------|
   | Network Name | `Localhost 7545` |
   | New RPC URL | `http://127.0.0.1:7545` |
   | Chain ID | `1337` |
   | Currency Symbol | `ETH` |
   | Block Explorer URL | (leave blank) |

5. Click "Save"
6. Switch to this network

### Step 2: Import Ganache Account

1. In MetaMask, click account icon (top right)
2. Click "Import Account"
3. Select "Private Key"
4. Paste the private key from Ganache Account 0
5. Click "Import"

**⚠️ Important**: This imported account will be the **admin** for the smart contract.

### Step 3: Verify Connection

- Network should show: `Localhost 7545`
- Account should show: ~100 ETH balance
- Account address matches Ganache Account 0

---

## 📦 Project Installation

### Step 1: Clone or Download Project

```bash
# If using Git
git clone <repository-url>
cd newC

# Or extract downloaded ZIP to desired location
```

### Step 2: Install Node.js Dependencies

```bash
cd D:\Desktop\frelance\newC  # Navigate to project folder
npm install
```

Wait for installation to complete (~2-5 minutes).

### Step 3: Install Python Dependencies

```bash
cd Database_API
pip install fastapi mysql-connector-python pydantic python-dotenv uvicorn PyJWT
```

### Step 4: Configure Environment Variables

#### Main .env file (project root):
```bash
# D:\Desktop\frelance\newC\.env
SECRET_KEY="your-secret-key-here"
```

#### Database API .env file:
```bash
# D:\Desktop\frelance\newC\Database_API\.env
MYSQL_USER="root"
MYSQL_PASSWORD="your-mysql-password"
MYSQL_HOST="localhost"
MYSQL_DB="hawassa_election_db"
SECRET_KEY="your-secret-key-here"
```

**⚠️ Replace `your-mysql-password` with your actual MySQL root password!**

---

## 🚀 Smart Contract Deployment

### Step 1: Compile Contracts

```bash
cd D:\Desktop\frelance\newC
truffle compile
```

Expected output:
```
Compiling your contracts...
===========================
> Compiling .\contracts\Migrations.sol
> Compiling .\contracts\StudentElection.sol
> Artifacts written to .\build\contracts
> Compiled successfully using:
   - solc: 0.8.0
```

### Step 2: Deploy to Ganache

**⚠️ Ensure Ganache is running and MetaMask is connected!**

```bash
truffle migrate
```

For fresh deployment (reset):
```bash
truffle migrate --reset
```

Expected output:
```
Deploying 'StudentElection'
   > transaction hash:    0x...
   > contract address:    0x...
   > account:             0x... (admin)
   > gas used:            ...
   > Deployed successfully!
```

**📝 Note the contract address** - this is stored in `build/contracts/StudentElection.json`

### Step 3: Bundle Frontend JavaScript

```bash
npx webpack
```

Expected output:
```
asset app.bundle.js 13.5 MiB [emitted]
asset login.bundle.js 2.4 KiB [emitted]
webpack compiled successfully
```

---

## ▶️ Running the Application

### Terminal 1: Start Node.js Server

```bash
cd D:\Desktop\frelance\newC
node index.js
```

Expected output:
```
============================================================
  🗳️  HAWASSA UNIVERSITY STUDENT ELECTION SYSTEM
============================================================
  Server running at http://localhost:8080
  Faculty of Informatics - Department of Computer Science
============================================================
```

### Terminal 2: Start Python API

```bash
cd D:\Desktop\frelance\newC\Database_API
uvicorn main:app --reload --host 127.0.0.1
```

Expected output:
```
INFO:     Uvicorn running on http://127.0.0.1:8000
INFO:     Application startup complete.
```

### Open the Application

1. Open browser
2. Go to: [http://localhost:8080](http://localhost:8080)
3. You should see the login page

---

## ✅ Verification Steps

### 1. Test API Health

Open in browser or use curl:
```
http://127.0.0.1:8000/health
```

Expected response:
```json
{
  "status": "healthy",
  "database": "connected",
  "timestamp": "2024-12-31 16:00:00"
}
```

### 2. Test Login

| Credential | Value |
|------------|-------|
| Student ID | `ADMIN001` |
| Password | `admin123` |

Should redirect to Admin Dashboard.

### 3. Test MetaMask Connection

1. On Admin Dashboard, check bottom of page
2. Should show: "🔗 Connected Wallet: 0x..."
3. Should match your MetaMask account

### 4. Test Blockchain Connection

Try adding a candidate:
1. Enter Name: `Test Candidate`
2. Enter Department: `Computer Science`
3. Click "Add Candidate"
4. MetaMask should prompt for transaction
5. Confirm transaction
6. Candidate should appear in list

---

## 🔄 Quick Start Checklist

```
□ Node.js installed (v18+)
□ Python installed (3.9+)
□ MySQL installed and running
□ Ganache installed and running on port 7545
□ MetaMask installed and configured
□ Database created and schema imported
□ Environment variables configured
□ npm install completed
□ pip install completed
□ Contracts compiled (truffle compile)
□ Contracts deployed (truffle migrate)
□ Frontend bundled (npx webpack)
□ Node.js server running (port 8080)
□ Python API running (port 8000)
□ Can access http://localhost:8080
□ Can login as ADMIN001
□ MetaMask connects successfully
```

---

## 📝 Next Steps After Setup

1. **Login as Admin**: `ADMIN001` / `admin123`
2. **Create an Election**: Set title and dates
3. **Add Candidates**: Enter candidate details
4. **Register Voters**: Add wallet addresses to whitelist
5. **Test Voting**: Login as student and cast a vote

---

*Document Version: 1.0*
*Last Updated: December 2024*
*Project: Hawassa University Student Union Election System*
