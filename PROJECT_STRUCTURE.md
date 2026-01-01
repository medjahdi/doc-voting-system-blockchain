# 📁 Project Structure Documentation

## Hawassa University Student Union Election System
### Blockchain-based Voting System - File Structure & Purpose

---

## 📂 Complete Project Tree

```
newC/
│
├── 📄 .env                          # Node.js environment variables
├── 📄 .gitignore                    # Git ignore file
├── 📄 index.js                      # Express.js server entry point
├── 📄 package.json                  # Node.js dependencies
├── 📄 package-lock.json             # Locked dependency versions
├── 📄 truffle-config.js             # Truffle framework configuration
├── 📄 webpack.config.js             # Webpack bundler configuration
├── 📄 empty-module.js               # Webpack module placeholder
├── 📄 database_schema.sql           # MySQL database schema
│
├── 📁 contracts/                    # Solidity smart contracts
│   ├── 📄 StudentElection.sol       # Main election contract
│   ├── 📄 Migrations.sol            # Truffle migrations contract
│   └── 📄 2_deploy_contracts.js     # Contract deployment script
│
├── 📁 migrations/                   # Truffle migration scripts
│   └── 📄 1_initial_migration.js    # Initial deployment migration
│
├── 📁 build/                        # Compiled contract artifacts
│   └── 📁 contracts/
│       ├── 📄 StudentElection.json  # Compiled election contract ABI
│       └── 📄 Migrations.json       # Compiled migrations contract ABI
│
├── 📁 Database_API/                 # Python FastAPI backend
│   ├── 📄 .env                      # Database environment variables
│   ├── 📄 .gitignore                # Python cache ignore
│   ├── 📄 main.py                   # FastAPI application
│   └── 📁 __pycache__/              # Python compiled files
│
├── 📁 src/                          # Frontend source code
│   ├── 📁 html/                     # HTML pages
│   │   ├── 📄 login.html            # Student login page
│   │   ├── 📄 register.html         # Student registration page
│   │   ├── 📄 index.html            # Voter dashboard
│   │   └── 📄 admin.html            # Admin dashboard
│   │
│   ├── 📁 css/                      # Stylesheets
│   │   ├── 📄 login.css             # Login page styles
│   │   ├── 📄 index.css             # Voter dashboard styles
│   │   └── 📄 admin.css             # Admin dashboard styles
│   │
│   ├── 📁 js/                       # JavaScript source files
│   │   ├── 📄 app.js                # Main application logic
│   │   └── 📄 login.js              # Login handler
│   │
│   ├── 📁 dist/                     # Compiled JavaScript bundles
│   │   ├── 📄 app.bundle.js         # Bundled main application
│   │   └── 📄 login.bundle.js       # Bundled login script
│   │
│   └── 📁 assets/                   # Static assets (images, etc.)
│
├── 📁 public/                       # Public static files
│   └── 📄 favicon.ico               # Website favicon
│
└── 📁 node_modules/                 # Node.js dependencies
```

---

## 📋 Detailed File Descriptions

### 🔧 Root Configuration Files

| File | Purpose | Technology |
|------|---------|------------|
| `index.js` | Main Express.js server. Handles routing, authentication middleware, and serves static files. | Node.js, Express |
| `package.json` | Defines project dependencies, scripts, and metadata. | NPM |
| `truffle-config.js` | Configures Truffle framework: network settings (Ganache), Solidity compiler version. | Truffle |
| `webpack.config.js` | Bundles frontend JavaScript with polyfills for browser compatibility. | Webpack |
| `.env` | Stores environment variables (SECRET_KEY for JWT). | dotenv |
| `database_schema.sql` | Complete MySQL database schema with all tables and sample data. | MySQL |
| `empty-module.js` | Placeholder module for Webpack to ignore unnecessary dependencies. | JavaScript |

---

### ⛓️ Smart Contracts (`/contracts`)

| File | Purpose | Key Functions |
|------|---------|---------------|
| `StudentElection.sol` | **Core smart contract** implementing the voting logic. Manages candidates, elections, voter registration, and voting. | `addCandidate()`, `createElection()`, `vote()`, `registerVoter()`, `getWinner()` |
| `Migrations.sol` | Truffle's migration tracking contract. Records which migrations have been deployed. | `setCompleted()` |
| `2_deploy_contracts.js` | Deployment script that tells Truffle how to deploy StudentElection. | N/A |

#### StudentElection.sol Structures:
```solidity
struct Candidate {
    uint id;
    string name;
    string department;
    uint voteCount;
}

struct Election {
    uint id;
    string title;
    uint256 startDate;
    uint256 endDate;
    bool isActive;
}
```

---

### 🐍 Backend API (`/Database_API`)

| File | Purpose | Endpoints |
|------|---------|-----------|
| `main.py` | FastAPI REST API for student authentication and database operations. | `/login`, `/register`, `/student/{id}`, `/validate/{id}`, `/audit-log`, `/health` |
| `.env` | Database credentials (MySQL user, password, host, database name, JWT secret). | N/A |

#### API Endpoints:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | API root - health check |
| GET | `/login` | Authenticate student, return JWT |
| POST | `/register` | Register new student voter |
| GET | `/student/{id}` | Get student details |
| GET | `/validate/{id}` | Validate student eligibility |
| POST | `/audit-log` | Create audit log entry |
| GET | `/audit-logs` | Get all audit logs |
| GET | `/health` | API health status |

---

### 🎨 Frontend (`/src`)

#### HTML Pages (`/src/html`)
| File | Purpose | Access |
|------|---------|--------|
| `login.html` | Student/Admin login portal | Public (`/`) |
| `register.html` | New student registration | Public (`/register.html`) |
| `index.html` | Voter dashboard - view candidates and vote | Protected (requires auth) |
| `admin.html` | Admin dashboard - manage elections | Protected (admin role) |

#### JavaScript (`/src/js`)
| File | Purpose | Key Functions |
|------|---------|---------------|
| `app.js` | Main application logic. Connects to blockchain, handles voting, candidate management. | `eventStart()`, `vote()`, `loadCandidates()`, `setupEventListeners()` |
| `login.js` | Handles login form submission, JWT storage, role-based redirection. | Form submission handler |

#### CSS Stylesheets (`/src/css`)
| File | Purpose | Design Elements |
|------|---------|-----------------|
| `login.css` | Styles for login/register pages | Glassmorphism, gradient background, form styles |
| `index.css` | Voter dashboard styling | Candidate table, vote button, results section |
| `admin.css` | Admin dashboard styling | Statistics cards, form sections, candidate list |

---

### 📦 Build Artifacts

#### `/build/contracts/`
Contains compiled Solidity contracts in JSON format with:
- **ABI (Application Binary Interface)**: Defines how to interact with the contract
- **Bytecode**: Compiled contract code for deployment
- **Networks**: Deployed contract addresses per network

#### `/src/dist/`
Webpack-bundled JavaScript files:
- `app.bundle.js` (~13.5 MB) - Main application with Web3 and contract libraries
- `login.bundle.js` (~2.4 KB) - Login handler bundle

---

## 🔗 File Dependencies

```
Frontend (Browser)
    └── app.bundle.js
        ├── Web3.js (Blockchain interaction)
        ├── @truffle/contract (Contract abstraction)
        └── StudentElection.json (Contract ABI)

Express Server (index.js)
    ├── JWT verification
    ├── Static file serving
    └── Route protection

FastAPI (main.py)
    ├── MySQL database
    ├── JWT token generation
    └── CORS handling

Blockchain (Ganache)
    └── StudentElection contract
        ├── Candidate data
        ├── Voter registrations
        ├── Vote records
        └── Election configuration
```

---

## 📊 Database Tables

| Table | Purpose | Key Columns |
|-------|---------|-------------|
| `students` | Store student/voter information | student_id, full_name, department, wallet_address, role, password |
| `admins` | Administrator accounts | admin_id, full_name, role, password_hash |
| `elections` | Election metadata | election_id, title, start_date, end_date, status |
| `candidates` | Candidate information | candidate_id, election_id, full_name, department |
| `votes` | Off-chain vote backup | vote_id, election_id, candidate_id, txn_hash |
| `results` | Computed results | result_id, election_id, candidate_id, vote_count |
| `audit_logs` | System activity logs | log_id, user_id, action_type, timestamp, details |

---

*Document Version: 1.0*
*Last Updated: December 2024*
*Project: Hawassa University Student Union Election System*
