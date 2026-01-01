# 🔧 Troubleshooting Guide

## Hawassa University Student Union Election System
### Common Errors & Solutions

---

## 📋 Table of Contents

1. [Installation Errors](#installation-errors)
2. [Database Errors](#database-errors)
3. [Blockchain Errors](#blockchain-errors)
4. [MetaMask Errors](#metamask-errors)
5. [Smart Contract Errors](#smart-contract-errors)
6. [Frontend Errors](#frontend-errors)
7. [API Errors](#api-errors)
8. [General Debugging Tips](#general-debugging-tips)

---

## 📦 Installation Errors

### Error: `npm install` fails

**Symptoms:**
```
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR! peer dep missing: ...
```

**Solution:**
```bash
npm install --legacy-peer-deps
# or
npm install --force
```

---

### Error: `truffle: command not found`

**Symptoms:**
```
'truffle' is not recognized as an internal or external command
```

**Solution:**
```bash
# Install Truffle globally
npm install -g truffle

# Verify installation
truffle version
```

---

### Error: Python package installation fails

**Symptoms:**
```
ERROR: Could not find a version that satisfies the requirement
```

**Solution:**
```bash
# Upgrade pip
python -m pip install --upgrade pip

# Install packages one by one
pip install fastapi
pip install mysql-connector-python
pip install uvicorn
pip install python-dotenv
pip install PyJWT
```

---

## 🗄️ Database Errors

### Error: `Something is wrong with your user name or password`

**Symptoms:**
- Python API shows this error on startup
- Login fails with "Failed to fetch"

**Solution:**
1. Check Database_API/.env file:
   ```bash
   MYSQL_USER="root"
   MYSQL_PASSWORD="your-actual-password"
   MYSQL_HOST="localhost"
   MYSQL_DB="hawassa_election_db"
   ```

2. Verify MySQL is running:
   ```bash
   # Windows
   net start mysql
   
   # Or check in Services app
   ```

3. Test MySQL connection manually:
   ```bash
   mysql -u root -p
   # Enter your password
   ```

---

### Error: `Database does not exist`

**Symptoms:**
- API cannot connect
- Tables not found

**Solution:**
```sql
-- In MySQL Shell
CREATE DATABASE hawassa_election_db;
USE hawassa_election_db;
SOURCE D:/path/to/project/database_schema.sql;
```

---

### Error: `Access denied for user 'root'@'localhost'`

**Symptoms:**
- Cannot connect to MySQL
- Permission errors

**Solution:**
```sql
-- Reset root password (run as admin)
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
FLUSH PRIVILEGES;
```

Then update `Database_API/.env` with new password.

---

## ⛓️ Blockchain Errors

### Error: `Could not connect to Ganache`

**Symptoms:**
```
Error: CONNECTION ERROR: Couldn't connect to node http://127.0.0.1:7545
```

**Solution:**
1. Ensure Ganache is running
2. Check Ganache settings:
   - Host: `127.0.0.1`
   - Port: `7545`
3. Restart Ganache

---

### Error: `truffle compile` - Compiler version mismatch

**Symptoms:**
```
ParserError: Source file requires different compiler version
```

**Solution:**
Ensure all `.sol` files use the same pragma:
```solidity
pragma solidity ^0.8.0;
```

Check `truffle-config.js`:
```javascript
compilers: {
  solc: {
    version: "0.8.0",
  }
}
```

---

### Error: `truffle migrate` - Network not found

**Symptoms:**
```
Error: Could not find network configuration for development
```

**Solution:**
Check `truffle-config.js` has:
```javascript
networks: {
  development: {
    host: "127.0.0.1",
    port: 7545,
    network_id: "*"
  }
}
```

---

## 🦊 MetaMask Errors

### Error: MetaMask not connecting

**Symptoms:**
- "Please install MetaMask" message
- Account address not showing

**Solution:**
1. Ensure MetaMask extension is installed
2. Refresh the page
3. Click "Connect" when MetaMask prompts
4. Allow the site to connect

---

### Error: Wrong network in MetaMask

**Symptoms:**
- Transactions fail
- Contract not found

**Solution:**
1. Open MetaMask
2. Click network dropdown
3. Select "Localhost 7545"
4. If not listed, add custom network:
   - Network Name: `Localhost 7545`
   - RPC URL: `http://127.0.0.1:7545`
   - Chain ID: `1337`
   - Currency: `ETH`

---

### Error: `Internal JSON-RPC error` on transactions

**Symptoms:**
```
MetaMask - RPC Error: Internal JSON-RPC error
```

**Common Causes & Solutions:**

#### Cause 1: Wrong account (not admin)
- The smart contract admin is the account that deployed it
- **Solution**: Import the first Ganache account (index 0) into MetaMask

#### Cause 2: Account nonce mismatch
- After restarting Ganache, account nonces reset
- **Solution**: In MetaMask:
  1. Settings → Advanced → Clear Activity Tab Data (older versions)
  2. Or: Settings → Advanced → Reset Account

#### Cause 3: Insufficient gas
- **Solution**: The contract defaults should handle this, but you can increase gas limit in MetaMask advanced options

---

### Error: `Only admin can perform this action`

**Symptoms:**
- Cannot add candidates
- Cannot create election

**Solution:**
1. The admin is whoever deployed the contract
2. Check which account deployed:
   - Look at Ganache transactions
   - First transaction = deployment
   - That account = admin

3. Import that account's private key into MetaMask:
   ```
   Ganache → Click 🔑 on Account 0 → Copy Private Key
   MetaMask → Import Account → Paste key
   ```

---

## 📜 Smart Contract Errors

### Error: `Voter already registered`

**Symptoms:**
- Cannot register a voter wallet
- Transaction reverts

**Solution:**
- The wallet is already registered
- Each wallet can only be registered once
- Use a different wallet address

---

### Error: `You have already voted`

**Symptoms:**
- Vote transaction fails
- Already voted message

**Solution:**
- Each wallet can only vote once
- This is by design for security
- To test again, use a different wallet or redeploy:
  ```bash
  truffle migrate --reset
  npx webpack
  ```

---

### Error: `Election has not started yet` / `Election has ended`

**Symptoms:**
- Cannot vote
- Time-related error

**Solution:**
1. Check election dates in admin panel
2. Ensure current time is between start and end dates
3. Use datetime-local format: `YYYY-MM-DDTHH:MM`

---

### Error: `No election exists`

**Symptoms:**
- Voting fails
- Election info not loading

**Solution:**
1. Login as admin
2. Create an election with valid dates
3. Confirm transaction in MetaMask

---

### Error: `You must be a registered voter`

**Symptoms:**
- Cannot vote
- Wallet not on whitelist

**Solution:**
1. Admin must register the voter's wallet address on-chain
2. Go to Admin Dashboard → "Register Voter"
3. Enter the voter's MetaMask wallet address
4. Confirm transaction

---

## 🖥️ Frontend Errors

### Error: `process is not defined`

**Symptoms:**
```
Uncaught ReferenceError: process is not defined
```

**Solution:**
Rebuild with webpack:
```bash
npx webpack
```

Ensure `webpack.config.js` has:
```javascript
new webpack.ProvidePlugin({
  process: 'process/browser',
  Buffer: ['buffer', 'Buffer']
})
```

---

### Error: `Web3 is not a constructor`

**Symptoms:**
```
Uncaught TypeError: Web3 is not a constructor
```

**Solution:**
Check `src/js/app.js` uses:
```javascript
const Web3 = require('web3').default || require('web3');
```

Then rebuild:
```bash
npx webpack
```

---

### Error: Blank page / JavaScript not loading

**Symptoms:**
- Page loads but nothing works
- Console shows 404 for .bundle.js

**Solution:**
1. Check if bundles exist:
   ```
   src/dist/app.bundle.js
   src/dist/login.bundle.js
   ```

2. If missing, rebuild:
   ```bash
   npx webpack
   ```

3. Check Express routes in `index.js`

---

### Error: `Failed to fetch` on login

**Symptoms:**
- Login shows "Failed to fetch"
- Network error

**Solution:**
1. Check Python API is running:
   ```bash
   cd Database_API
   uvicorn main:app --reload --host 127.0.0.1
   ```

2. Check API health:
   ```
   http://127.0.0.1:8000/health
   ```

3. Check CORS settings in `main.py`

---

## 🔌 API Errors

### Error: API not starting

**Symptoms:**
- uvicorn fails to start
- Import errors

**Solution:**
```bash
# Check Python version
python --version  # Should be 3.9+

# Reinstall dependencies
pip install --upgrade fastapi uvicorn mysql-connector-python python-dotenv PyJWT
```

---

### Error: `401 Unauthorized` on login

**Symptoms:**
- Login fails
- Invalid credentials message

**Solution:**
1. Check username and password are correct
2. Verify user exists in database:
   ```sql
   SELECT * FROM students WHERE student_id = 'ADMIN001';
   ```
3. Check password matches

---

### Error: CORS error in browser console

**Symptoms:**
```
Access to fetch has been blocked by CORS policy
```

**Solution:**
Check `Database_API/main.py` has correct origins:
```python
origins = [
    "http://localhost:8080",
    "http://127.0.0.1:8080",
]
```

---

## 🔍 General Debugging Tips

### 1. Check All Services Are Running

| Service | Port | Check Command |
|---------|------|---------------|
| Ganache | 7545 | Open Ganache UI |
| Express | 8080 | `http://localhost:8080` |
| FastAPI | 8000 | `http://127.0.0.1:8000/health` |
| MySQL | 3306 | `mysql -u root -p` |

### 2. Browser Console

- Press `F12` to open Developer Tools
- Check "Console" tab for errors
- Check "Network" tab for failed requests

### 3. Restart Everything

When in doubt, restart in this order:
1. Stop all terminals (Ctrl+C)
2. Restart Ganache
3. Reset MetaMask account (Settings → Advanced → Reset Account)
4. Redeploy contracts: `truffle migrate --reset`
5. Rebuild frontend: `npx webpack`
6. Start Node server: `node index.js`
7. Start Python API: `uvicorn main:app --reload --host 127.0.0.1`
8. Hard refresh browser: Ctrl+Shift+R

### 4. Check Environment Variables

```bash
# Print .env contents (don't share passwords!)
type Database_API\.env   # Windows
cat Database_API/.env    # Mac/Linux
```

### 5. Verify Contract Deployment

```bash
truffle console
> let instance = await StudentElection.deployed()
> instance.address  // Should show contract address
> let admin = await instance.admin()
> admin  // Should show admin wallet address
```

---

## 🆘 Still Having Issues?

1. **Check the error message carefully** - it usually tells you what's wrong
2. **Search the error** - Stack Overflow often has solutions
3. **Check Ganache logs** - Shows transaction details and errors
4. **Review recent changes** - Did you modify any files?
5. **Try a fresh start** - Sometimes starting over is faster

---

*Document Version: 1.0*
*Last Updated: December 2024*
*Project: Hawassa University Student Union Election System*
