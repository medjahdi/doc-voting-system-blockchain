# 🚀 Quick Start Guide - Running After PC Restart

## How to Start the Blockchain Voting System

After restarting your PC, follow these steps to run the project:

---

## Step 1: Start Docker Desktop

1. Open **Docker Desktop** from the Start Menu
2. Wait until Docker shows "Docker Desktop is running" (green icon in system tray)
3. This may take 1-2 minutes

---

## Step 2: Fix Windows Network (Important!)

⚠️ **Before starting Docker containers**, run these commands in **PowerShell as Administrator**:

```powershell
net stop winnat
net start winnat
```

This fixes a Windows port reservation issue that can block Docker.

---

## Step 3: Start the Project

1. Open **PowerShell** 
2. Navigate to the project folder:
   ```powershell
   cd "C:\Users\Dr Dawit Asnake\OneDrive\Desktop\Blockchain-Voting-System-v2.2-Docker"
   ```
3. Start all services:
   ```powershell
   docker-compose up
   ```
4. Wait until you see:
   ```
   voting_web      | Server running at http://localhost:8080
   voting_api      | Uvicorn running on http://0.0.0.0:5000
   voting_ganache  | RPC Listening on 0.0.0.0:8545
   ```

---

## Step 4: Deploy Smart Contract

⚠️ **After every Docker restart**, the blockchain resets. Deploy the contract:

Open a **new PowerShell window** and run:
```powershell
cd "C:\Users\Dr Dawit Asnake\OneDrive\Desktop\Blockchain-Voting-System-v2.2-Docker"
truffle migrate --network docker
```

---

## Step 5: Open the Application

1. Open your browser
2. Go to: **http://localhost:8080**
3. Login with:
   - **Admin:** ADMIN001 / admin123
   - **Student:** 2838/13 / student123

---

## 📋 Quick Command Reference

| Task | Command |
|------|---------|
| Fix Windows network | `net stop winnat` then `net start winnat` (run as Admin) |
| Start project | `docker-compose up` |
| Start in background | `docker-compose up -d` |
| Stop project | `docker-compose down` |
| Deploy contract | `truffle migrate --network docker` |
| View running containers | `docker ps` |

---

## ✅ Complete Restart Checklist

1. [ ] Start Docker Desktop and wait for green icon
2. [ ] Run `net stop winnat` then `net start winnat` (as Admin)
3. [ ] Run `docker-compose up`
4. [ ] Wait for all services to start
5. [ ] Run `truffle migrate --network docker` (new window)
6. [ ] Open http://localhost:8080

---

## 🔧 Troubleshooting

### "Port not available" error
Run as Administrator:
```powershell
net stop winnat
net start winnat
```

### Docker not starting?
- Make sure Docker Desktop is running (green icon)
- Try restarting Docker Desktop

### MetaMask shows wrong balance?
- Make sure you're on "Ganache Local" network (port 8545)
- Import this private key:
  ```
  0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d
  ```

### "MetaMask not connected" error?
- Run `truffle migrate --network docker` to deploy the contract
- Refresh the browser page

---

## 🎉 That's it!

Your Blockchain Voting System is ready to use!
