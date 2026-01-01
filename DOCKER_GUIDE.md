# 🐳 Docker Installation & Quick Start Guide

## Hawassa University Student Election System
### Blockchain-based Voting System - Docker Version

---

## Part 1: Installing Docker Desktop

### Windows Installation

1. **Download Docker Desktop**
   - Go to: https://www.docker.com/products/docker-desktop/
   - Click **"Download for Windows"**

2. **Run the Installer**
   - Double-click `Docker Desktop Installer.exe`
   - Check ✅ "Use WSL 2 instead of Hyper-V" (recommended)
   - Click **Install**

3. **Enable WSL 2 (if prompted)**
   - Open **PowerShell as Administrator** and run:
   ```powershell
   wsl --install
   ```
   - Restart your computer

4. **Start Docker Desktop**
   - After restart, launch **Docker Desktop** from the Start menu
   - Wait for the whale icon in the system tray to show "Docker Desktop is running"
   - This may take 2-3 minutes on first launch

5. **Verify Installation**
   - Open PowerShell and run:
   ```powershell
   docker --version
   docker-compose --version
   ```
   - You should see version numbers (e.g., `Docker version 24.x.x`)

### Troubleshooting Docker Installation

| Issue | Solution |
|-------|----------|
| "WSL 2 installation is incomplete" | Run `wsl --install` in PowerShell as Admin, then restart |
| Docker Desktop won't start | Enable virtualization in BIOS (VT-x or AMD-V) |
| "Access denied" errors | Run Docker Desktop as Administrator |

---

## Part 2: Running the Voting System

### Step 1: Extract the Project

1. Extract `Blockchain-Voting-System.zip` to a folder (e.g., `C:\Blockchain-Voting-System\`)

### Step 2: Start All Services

1. **Open PowerShell** and navigate to the project folder:
   ```powershell
   cd C:\Blockchain-Voting-System
   ```

2. **Run Docker Compose** (one command!):
   ```powershell
   docker-compose up --build
   ```

3. **Wait for startup** - You'll see logs like:
   ```
   voting_db       | ready for connections
   voting_ganache  | Listening on 0.0.0.0:8545
   voting_api      | Uvicorn running on http://0.0.0.0:5000
   voting_web      | 🗳️  HAWASSA UNIVERSITY STUDENT ELECTION SYSTEM
   voting_web      | Server running at http://localhost:8080
   ```

4. **Access the application**: Open your browser to **http://localhost:8080**

---

## Part 3: MetaMask Setup

### Install MetaMask

1. Go to https://metamask.io/
2. Click **"Download"** and add the Chrome/Firefox extension
3. Create a new wallet or import existing

### Connect to Local Blockchain

1. Open MetaMask → Click the network dropdown (top-left)
2. Click **"Add Network"** → **"Add a network manually"**
3. Enter these settings:

| Field | Value |
|-------|-------|
| Network Name | `Ganache Local` |
| RPC URL | `http://127.0.0.1:8545` |
| Chain ID | `1337` |
| Currency Symbol | `ETH` |

4. Click **Save**

### Import Test Account

1. In MetaMask, click your account icon → **"Import Account"**
2. Select **"Private Key"**
3. Paste this private key:
   ```
   0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d
   ```
4. Click **Import**
5. You now have 1000 ETH for testing!

---

## Part 4: Using the System

### Access URLs

| Service | URL | Description |
|---------|-----|-------------|
| **Main App** | http://localhost:8080 | Login & voting portal |
| **API Health** | http://localhost:5000/health | Check API status |
| **Blockchain** | http://localhost:8545 | Ganache RPC endpoint |

### Default Login Credentials

#### Administrator:
- **Student ID:** `ADMIN001`
- **Password:** `admin123`

#### Test Voter Accounts:
| Student ID | Password |
|------------|----------|
| `2838/13` | `student123` |
| `0019/13` | `student123` |
| `0044/13` | `student123` |
| `0056/13` | `student123` |
| `0069/13` | `student123` |

---

## Part 5: Managing the System

### Stop the Application
Press `Ctrl+C` in the PowerShell window, then:
```powershell
docker-compose down
```

### Restart the Application
```powershell
docker-compose up
```
(No `--build` needed after first run unless you change files)

### View Logs
```powershell
# All services
docker-compose logs

# Specific service
docker-compose logs web
docker-compose logs api
docker-compose logs db
docker-compose logs ganache
```

### Reset Everything (Fresh Start)
```powershell
docker-compose down -v
docker-compose up --build
```

---

## Troubleshooting

### "Port already in use"
Another application is using port 8080, 5000, 8545, or 3306.
```powershell
# Stop all Docker containers
docker-compose down

# Check what's using the port (example: 8080)
netstat -ano | findstr :8080

# Kill the process (replace PID with actual number)
taskkill /PID <PID> /F
```

### "Cannot connect to Docker daemon"
Docker Desktop is not running. Start it from the Start menu.

### "Image build failed"
```powershell
# Clean up and rebuild
docker-compose down -v
docker system prune -f
docker-compose up --build
```

### Database not initialized
```powershell
# Remove old data and restart
docker-compose down -v
docker-compose up --build
```

---

## System Requirements

- **Operating System:** Windows 10 (version 2004+) or Windows 11
- **RAM:** 8 GB minimum (16 GB recommended)
- **Disk Space:** 10 GB free
- **CPU:** Must support virtualization (check BIOS if issues)
- **Browser:** Chrome, Firefox, or Edge (latest version)

---

## Support Contacts

If you encounter issues:
1. Check the troubleshooting section above
2. View Docker logs for error messages
3. Contact the development team

---

**Version:** 2.0 (Docker Edition)  
**Last Updated:** January 2026
