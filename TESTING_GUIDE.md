# 📋 Blockchain Voting System - Testing Guide

## How to Test the Application After Setup

This guide will walk you through testing all features of the Hawassa University Student Election System.

---

## 📌 Before You Start

Make sure:
- ✅ Docker Desktop is running
- ✅ All containers are running (`docker-compose up`)
- ✅ You see these messages in the terminal:
  ```
  voting_web      | Server running at http://localhost:8080
  voting_api      | Uvicorn running on http://0.0.0.0:5000
  voting_ganache  | RPC Listening on 0.0.0.0:8545
  ```
- ✅ MetaMask is installed and connected to Ganache network

---

## 🔐 Test 1: Admin Login

### Steps:

1. **Open your browser** and go to:
   ```
   http://localhost:8080
   ```

2. **Enter Admin Credentials:**
   - **Student ID:** `ADMIN001`
   - **Password:** `admin123`

3. **Click "Login"**

4. **Expected Result:** 
   - You should be redirected to the Admin Dashboard (`/admin.html`)
   - The dashboard shows election management options

### What Admin Can Do:
- ✅ Create new elections
- ✅ Add candidates
- ✅ Start/End elections
- ✅ View voting results
- ✅ View audit logs

---

## 👤 Test 2: Student Voter Login

### Steps:

1. **Go to:** `http://localhost:8080`

2. **Enter Voter Credentials (any of these):**

   | Student ID | Password | Name |
   |------------|----------|------|
   | `2838/13` | `student123` | Abel Abera |
   | `0019/13` | `student123` | Behailu Bekele |
   | `0044/13` | `student123` | John Dawit |
   | `0056/13` | `student123` | Mintesnot Abebe |
   | `0069/13` | `student123` | Serawit Yoseph |

3. **Click "Login"**

4. **Expected Result:**
   - You should be redirected to the Voting page (`/index.html`)
   - The page shows available elections and candidates

---

## 📝 Test 3: Register a New Student

### Steps:

1. **Go to:** `http://localhost:8080`

2. **Click "Register here"** link below the login form

3. **Fill in the registration form:**
   - **Student ID:** `TEST001` (or any unique ID)
   - **Full Name:** `Test Student`
   - **Department:** `Computer Science`
   - **Wallet Address:** Copy from MetaMask (click account → copy address)
   - **Password:** `test123`

4. **Click "Register"**

5. **Expected Result:**
   - Success message: "Student registered successfully"
   - You can now login with the new account

### How to Get Your MetaMask Wallet Address:
1. Open MetaMask extension
2. Click on your account name (shows a short address like `0x90F8...`)
3. Click "Copy address to clipboard"
4. Paste in the Wallet Address field

---

## 🗳️ Test 4: Cast a Vote

### Prerequisites:
- Login as a voter (e.g., `2838/13` / `student123`)
- MetaMask must be connected to Ganache network
- An election must be active (created by admin)

### Steps:

1. **Login as a voter**

2. **On the voting page, you will see:**
   - List of active elections
   - Candidates for each election

3. **Select a candidate** by clicking on their card

4. **Click "Vote" button**

5. **MetaMask will pop up** asking to confirm the transaction:
   - Click **"Confirm"** in MetaMask

6. **Wait for transaction confirmation** (a few seconds)

7. **Expected Result:**
   - Success message: "Vote cast successfully!"
   - Transaction hash is displayed
   - You cannot vote again in the same election

---

## 🦊 Test 5: MetaMask Connection

### Steps to Set Up MetaMask:

1. **Open MetaMask** browser extension

2. **Add Ganache Network:**
   - Click the network dropdown (top-left)
   - Click "Add network" → "Add a network manually"
   - Enter:
     - **Network Name:** `Ganache Local`
     - **RPC URL:** `http://127.0.0.1:8545`
     - **Chain ID:** `1337`
     - **Currency Symbol:** `ETH`
   - Click "Save"

3. **Import Test Account:**
   - Click account icon → "Import account"
   - Select "Private Key"
   - Paste: `0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d`
   - Click "Import"

4. **Expected Result:**
   - Account shows `1000 ETH` balance
   - Network shows "Ganache Local"

---

## 📊 Test 6: View Election Results (Admin)

### Steps:

1. **Login as Admin** (`ADMIN001` / `admin123`)

2. **Go to Election Results** section

3. **Select an election** to view results

4. **Expected Result:**
   - Displays vote count for each candidate
   - Shows percentage of votes
   - Shows transaction hashes for verification

---

## ✅ Test 7: API Health Check

### Steps:

1. **Open browser** and go to:
   ```
   http://127.0.0.1:5000/health
   ```

2. **Expected Result:**
   ```json
   {
     "status": "healthy",
     "database": "connected",
     "timestamp": "2026-01-02 12:00:00.000000"
   }
   ```

If you see `"database": "disconnected"`, the MySQL container may not be running.

---

## 🔍 Troubleshooting Common Issues

### Issue: "Failed to fetch" on login
**Solution:**
1. Check if all Docker containers are running: `docker ps`
2. Check API health: `http://127.0.0.1:5000/health`
3. Clear browser cache: `Ctrl+Shift+Delete`

### Issue: MetaMask shows "Could not fetch chain ID"
**Solution:**
1. Make sure Ganache container is running
2. Use `http://127.0.0.1:8545` (NOT `localhost`)
3. Wait 30 seconds after starting Docker, then retry

### Issue: "Transaction failed" when voting
**Solution:**
1. Check MetaMask is on "Ganache Local" network
2. Make sure account has ETH balance
3. Try importing a new account from Ganache

### Issue: Cannot register - "Wallet address already registered"
**Solution:**
Each wallet address can only be registered once. Import a different account from Ganache.

---

## 📱 Quick Reference

| Task | URL | Credentials |
|------|-----|-------------|
| Login Page | http://localhost:8080 | See below |
| Admin Login | http://localhost:8080 | ADMIN001 / admin123 |
| Voter Login | http://localhost:8080 | 2838/13 / student123 |
| API Health | http://127.0.0.1:5000/health | - |
| Ganache RPC | http://127.0.0.1:8545 | - |

---

## 🎉 Success Checklist

After testing, you should be able to:

- [ ] Login as Admin
- [ ] Login as Voter
- [ ] Register new student account
- [ ] Connect MetaMask to Ganache
- [ ] Cast a vote (with MetaMask confirmation)
- [ ] View election results
- [ ] See API health check response

---

**Congratulations!** If all tests pass, your Blockchain Voting System is working correctly! 🗳️
