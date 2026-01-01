# 🗳️ Hawassa University Student Union Election System

## Blockchain-based Voting System for Student Union Presidential Election

[![Solidity](https://img.shields.io/badge/Solidity-0.8.0-363636?logo=solidity)](https://soliditylang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?logo=node.js)](https://nodejs.org/)
[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?logo=python)](https://python.org/)
[![Ethereum](https://img.shields.io/badge/Ethereum-Ganache-3C3C3D?logo=ethereum)](https://trufflesuite.com/ganache/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

<p align="center">
  <img src="https://img.shields.io/badge/Faculty-Informatics-00bcd4" alt="Faculty"/>
  <img src="https://img.shields.io/badge/Department-Computer%20Science-00bcd4" alt="Department"/>
  <img src="https://img.shields.io/badge/Institution-Hawassa%20University-00bcd4" alt="University"/>
</p>

---

## 📋 Overview

A decentralized voting system designed for Hawassa University's Student Union Presidential Elections. This system leverages **Ethereum blockchain technology** to ensure transparent, secure, and tamper-proof elections.

### 🎯 Key Features

| Feature | Description |
|---------|-------------|
| 🔐 **Secure Authentication** | JWT-based login with role-based access control |
| ⛓️ **Blockchain Voting** | Votes recorded immutably on Ethereum blockchain |
| 👤 **Voter Registration** | On-chain voter whitelist managed by admin |
| 📊 **Real-time Results** | Instant vote counting computed from blockchain |
| 🗓️ **Election Management** | Admin can create elections with time windows |
| 📝 **Audit Trail** | Complete logging of all system activities |
| 🔒 **Double-vote Prevention** | Smart contract enforces one vote per wallet |
| 👁️ **Transparent Verification** | Anyone can verify vote counts on-chain |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     PRESENTATION LAYER                          │
│              HTML/CSS/JavaScript (Browser)                       │
├─────────────────────────────────────────────────────────────────┤
│                     APPLICATION LAYER                           │
│                 Express.js (Port 8080)                          │
├─────────────────────────────────────────────────────────────────┤
│                   BUSINESS LOGIC LAYER                          │
│              FastAPI Python (Port 8000)                         │
├─────────────────────────────────────────────────────────────────┤
│                     BLOCKCHAIN LAYER                            │
│             Ethereum/Ganache (Port 7545)                        │
├─────────────────────────────────────────────────────────────────┤
│                       DATA LAYER                                │
│                   MySQL Database                                │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

| Category | Technology |
|----------|------------|
| **Frontend** | HTML5, CSS3, JavaScript, jQuery |
| **Backend (Web)** | Node.js, Express.js |
| **Backend (API)** | Python, FastAPI |
| **Database** | MySQL 8.0+ |
| **Blockchain** | Ethereum (Ganache for development) |
| **Smart Contracts** | Solidity 0.8.0 |
| **Contract Framework** | Truffle Suite |
| **Wallet** | MetaMask |
| **Bundler** | Webpack |

---

## 📦 Quick Start

### Prerequisites

- Node.js v18+
- Python 3.9+
- MySQL 8.0+
- Ganache
- MetaMask browser extension

### Installation

```bash
# 1. Clone the repository
git clone <repository-url>
cd newC

# 2. Install Node.js dependencies
npm install

# 3. Install Python dependencies
cd Database_API
pip install fastapi mysql-connector-python uvicorn python-dotenv PyJWT
cd ..

# 4. Setup database
mysql -u root -p < database_schema.sql

# 5. Configure environment variables
# Edit Database_API/.env with your MySQL credentials

# 6. Start Ganache and configure MetaMask

# 7. Deploy smart contracts
truffle compile
truffle migrate

# 8. Bundle frontend
npx webpack

# 9. Start servers
node index.js                                    # Terminal 1
cd Database_API && uvicorn main:app --reload     # Terminal 2

# 10. Open http://localhost:8080
```

### Default Credentials

| Role | Student ID | Password |
|------|------------|----------|
| Admin | `ADMIN001` | `admin123` |
| Student | `2838/13` | `student123` |

---

## 📂 Project Structure

```
newC/
├── contracts/              # Solidity smart contracts
│   ├── StudentElection.sol # Main election contract
│   └── Migrations.sol      # Truffle migrations
├── Database_API/           # Python FastAPI backend
│   ├── main.py             # API endpoints
│   └── .env                # Database credentials
├── src/
│   ├── html/               # HTML pages
│   ├── css/                # Stylesheets
│   ├── js/                 # JavaScript source
│   └── dist/               # Bundled JS files
├── migrations/             # Truffle migrations
├── build/                  # Compiled contracts
├── index.js                # Express server
├── truffle-config.js       # Truffle configuration
├── webpack.config.js       # Webpack configuration
├── database_schema.sql     # MySQL schema
└── package.json            # Node.js dependencies
```

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [📁 Project Structure](docs/PROJECT_STRUCTURE.md) | Detailed file organization and purpose |
| [🔄 How It Works](docs/HOW_IT_WORKS.md) | Complete technical workflow |
| [🛠️ Setup Guide](docs/SETUP_GUIDE.md) | Step-by-step installation |
| [🔧 Troubleshooting](docs/TROUBLESHOOTING.md) | Common errors and solutions |

---

## 🎮 Usage

### Admin Workflow

1. **Login** as `ADMIN001`
2. **Create Election** - Set title and voting dates
3. **Add Candidates** - Enter candidate name and department
4. **Register Voters** - Add student wallet addresses to whitelist
5. **Monitor** - View real-time vote counts

### Voter Workflow

1. **Register** - Create account with Student ID and wallet address
2. **Login** - Authenticate with credentials
3. **Connect MetaMask** - Link your registered wallet
4. **Vote** - Select candidate and confirm transaction
5. **Verify** - View results on blockchain

---

## 🔒 Security Features

- ✅ **Smart Contract Modifiers** - Admin-only functions protected
- ✅ **Double Voting Prevention** - Each wallet can vote once
- ✅ **Time-bound Elections** - Voting only during defined period
- ✅ **JWT Authentication** - Secure token-based login
- ✅ **On-chain Voter Registry** - Only whitelisted wallets can vote
- ✅ **Immutable Records** - Votes cannot be altered after casting
- ✅ **Event Logging** - All actions recorded on blockchain

---

## 📊 Smart Contract

The `StudentElection.sol` contract manages:

```solidity
// Key structures
struct Candidate { uint id; string name; string department; uint voteCount; }
struct Election { uint id; string title; uint256 startDate; uint256 endDate; bool isActive; }

// Key functions
function addCandidate(name, department) public onlyAdmin
function createElection(title, startDate, endDate) public onlyAdmin
function registerVoter(voterAddress) public onlyAdmin
function vote(candidateId) public onlyRegisteredVoter
function getWinner() public view returns (winner details)
```

---

## 🤝 Contributing

This project was developed for Hawassa University's Computer Science department.

### Project Team

- **Abel Abera** - ID: 2838/13
- **Behailu Bekele** - ID: 0019/13
- **John Dawit** - ID: 0044/13
- **Mintesnot Abebe** - ID: 0056/13
- **Serawit Yoseph** - ID: 0069/13

**Project Advisor:** Mr. Amsalu D.

---

## 📄 License

This project is developed for academic purposes at Hawassa University Institute of Technology, Faculty of Informatics, Department of Computer Science.

---

## 🙏 Acknowledgements

- Hawassa University for academic support
- Truffle Suite for development tools
- OpenZeppelin for smart contract security patterns
- The Ethereum community for blockchain technology

---

<p align="center">
  <strong>Hawassa University Institute of Technology</strong><br>
  Faculty of Informatics | Department of Computer Science<br>
  <em>September 2025</em>
</p>

---

<p align="center">
  Made with ❤️ for transparent student elections
</p>
