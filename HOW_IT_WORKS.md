# 🔄 How The System Works

## Hawassa University Student Union Election System
### Complete Technical Workflow Documentation

---

## 📋 Table of Contents

1. [System Architecture Overview](#system-architecture-overview)
2. [Technology Stack](#technology-stack)
3. [Authentication Flow](#authentication-flow)
4. [Election Management Flow](#election-management-flow)
5. [Voting Process Flow](#voting-process-flow)
6. [Data Flow Between Components](#data-flow-between-components)
7. [Smart Contract Mechanics](#smart-contract-mechanics)
8. [Security Mechanisms](#security-mechanisms)

---

## 🏗️ System Architecture Overview

The system follows a **5-layer architecture**:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                            │
│         (HTML/CSS/JavaScript - Browser Interface)                │
├─────────────────────────────────────────────────────────────────┤
│                    APPLICATION LAYER                             │
│    (Express.js Server - Routing & Authentication Middleware)     │
├─────────────────────────────────────────────────────────────────┤
│                    BUSINESS LOGIC LAYER                          │
│         (FastAPI - Student Management & Validation)              │
├─────────────────────────────────────────────────────────────────┤
│                    BLOCKCHAIN LAYER                              │
│    (Ethereum/Ganache - Smart Contracts & Vote Recording)         │
├─────────────────────────────────────────────────────────────────┤
│                    DATA LAYER                                    │
│      (MySQL Database - Off-chain Student Data Storage)           │
└─────────────────────────────────────────────────────────────────┘
```

### Component Interaction Diagram

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Browser    │────▶│ Express.js   │────▶│  FastAPI     │
│  (Frontend)  │     │  (Port 8080) │     │ (Port 8000)  │
└──────┬───────┘     └──────────────┘     └──────┬───────┘
       │                                         │
       │                                         ▼
       │                                  ┌──────────────┐
       │                                  │    MySQL     │
       │                                  │   Database   │
       │                                  └──────────────┘
       │
       ▼
┌──────────────┐     ┌──────────────┐
│   MetaMask   │────▶│   Ganache    │
│   Wallet     │     │  (Port 7545) │
└──────────────┘     └──────┬───────┘
                            │
                            ▼
                     ┌──────────────┐
                     │    Smart     │
                     │   Contract   │
                     └──────────────┘
```

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| Frontend | HTML5, CSS3, JavaScript | User interface |
| UI Framework | jQuery | DOM manipulation |
| Styling | Custom CSS with Glassmorphism | Modern UI design |
| Bundler | Webpack | JavaScript bundling |
| Web Server | Express.js (Node.js) | Static file serving, routing |
| API Server | FastAPI (Python) | RESTful API for database |
| Database | MySQL | Student data storage |
| Blockchain | Ethereum (Ganache) | Decentralized vote storage |
| Smart Contract | Solidity 0.8.0 | Voting logic |
| Contract Framework | Truffle | Development & deployment |
| Wallet | MetaMask | Transaction signing |
| Blockchain Library | Web3.js | JavaScript-Ethereum bridge |

---

## 🔐 Authentication Flow

### Step-by-Step Login Process

```
┌────────────────────────────────────────────────────────────────────┐
│                        LOGIN FLOW                                   │
└────────────────────────────────────────────────────────────────────┘

1. User visits http://localhost:8080
         │
         ▼
2. Express serves login.html
         │
         ▼
3. User enters Student ID + Password
         │
         ▼
4. login.js sends GET request to FastAPI
   ┌─────────────────────────────────────────────────────────────┐
   │ GET http://127.0.0.1:8000/login?student_id=X&password=Y     │
   │ Headers: Authorization: Bearer {student_id}                  │
   └─────────────────────────────────────────────────────────────┘
         │
         ▼
5. FastAPI queries MySQL database
   ┌─────────────────────────────────────────────────────────────┐
   │ SELECT role, full_name, department, wallet_address          │
   │ FROM students WHERE student_id = ? AND password = ?         │
   └─────────────────────────────────────────────────────────────┘
         │
         ▼
6. If credentials valid:
   - Generate JWT token
   - Return: { token, role, full_name, department, wallet_address }
         │
         ▼
7. Frontend stores token in localStorage
         │
         ▼
8. Redirect based on role:
   - admin  → /admin.html?Authorization=Bearer {token}
   - voter  → /index.html?Authorization=Bearer {token}
         │
         ▼
9. Express middleware validates JWT on protected routes
```

### JWT Token Structure

```json
{
  "student_id": "ADMIN001",
  "role": "admin",
  "full_name": "System Administrator",
  "department": "IT Department",
  "wallet_address": "0x..."
}
```

---

## 🗳️ Election Management Flow

### Admin Actions

#### 1. Creating an Election

```
Admin Dashboard                Smart Contract
      │                              │
      │ Fill form:                   │
      │ - Title                      │
      │ - Start Date                 │
      │ - End Date                   │
      │                              │
      │────── Click Create ─────────▶│
      │                              │
      │                    createElection(title, start, end)
      │                              │
      │         MetaMask Prompt      │
      │◀────────────────────────────│
      │                              │
      │────── Confirm TX ──────────▶│
      │                              │
      │                    Store in mapping:
      │                    elections[electionId] = Election(...)
      │                              │
      │                    Emit ElectionCreated event
      │                              │
      │◀───── TX Receipt ──────────│
```

#### 2. Adding Candidates

```solidity
// Smart Contract Function
function addCandidate(string memory _name, string memory _department) 
    public onlyAdmin 
{
    candidatesCount++;
    candidates[candidatesCount] = Candidate(
        candidatesCount, 
        _name, 
        _department, 
        0  // Initial vote count
    );
    emit CandidateAdded(candidatesCount, _name, _department);
}
```

#### 3. Registering Voters (On-Chain)

```
Admin enters wallet address
         │
         ▼
registerVoter(0x...) called
         │
         ▼
Contract checks: !registeredVoters[address]
         │
         ├── Already registered → Revert
         │
         └── Not registered → 
               registeredVoters[address] = true
               emit VoterRegistered(address)
```

---

## 🗳️ Voting Process Flow

### Complete Voting Sequence

```
┌────────────────────────────────────────────────────────────────────┐
│                        VOTING FLOW                                  │
└────────────────────────────────────────────────────────────────────┘

1. Student logs in → Redirected to index.html
         │
         ▼
2. app.js initializes:
   - Connect MetaMask (eth_requestAccounts)
   - Set contract provider
   - Load candidates from blockchain
   - Check if user has already voted
         │
         ▼
3. Page displays:
   - Election dates
   - Candidate list with current vote counts
   - Vote button (disabled if already voted)
         │
         ▼
4. User selects candidate (radio button)
         │
         ▼
5. User clicks "Cast My Vote"
         │
         ▼
6. Check vote eligibility (modifiers):
   
   ┌─────────────────────────────────────────────────────────────┐
   │ modifier onlyRegisteredVoter() {                            │
   │     require(registeredVoters[msg.sender]);                  │
   │ }                                                           │
   │                                                             │
   │ modifier hasNotVoted() {                                    │
   │     require(!hasVoted[msg.sender]);                         │
   │ }                                                           │
   │                                                             │
   │ modifier electionIsActive() {                               │
   │     require(block.timestamp >= startDate);                  │
   │     require(block.timestamp <= endDate);                    │
   │     require(election.isActive);                             │
   │ }                                                           │
   └─────────────────────────────────────────────────────────────┘
         │
         ├── Failed → Error message displayed
         │
         └── Passed → Continue
                │
                ▼
7. MetaMask prompts for transaction signature
         │
         ▼
8. Transaction sent to Ganache blockchain
         │
         ▼
9. Contract executes:
   
   hasVoted[msg.sender] = true;
   candidates[_candidateId].voteCount++;
   emit VoteCast(msg.sender, _candidateId);
         │
         ▼
10. Transaction mined, receipt returned
         │
         ▼
11. Frontend displays success message
         │
         ▼
12. Page reloads to show updated vote counts
```

### Vote Validation Checks

| Check | Contract Code | Error Message |
|-------|---------------|---------------|
| Valid candidate | `_candidateId > 0 && _candidateId <= candidatesCount` | "Invalid candidate ID" |
| Not already voted | `!hasVoted[msg.sender]` | "You have already voted" |
| Is registered | `registeredVoters[msg.sender]` | "You must be a registered voter" |
| Election active | `currentElection.isActive` | "Election is not active" |
| Within time window | `block.timestamp >= startDate && block.timestamp <= endDate` | "Election has not started" or "Election has ended" |

---

## 🔄 Data Flow Between Components

### Read Operations (View Data)

```
┌──────────┐      ┌──────────┐      ┌──────────┐      ┌──────────┐
│ Browser  │ ──▶  │ app.js   │ ──▶  │ Web3.js  │ ──▶  │ Ganache  │
│          │      │          │      │          │      │          │
│ Display  │ ◀──  │ Process  │ ◀──  │ Decode   │ ◀──  │ Contract │
│ Results  │      │ Data     │      │ Response │      │ State    │
└──────────┘      └──────────┘      └──────────┘      └──────────┘

Example: Loading Candidates
─────────────────────────────
1. app.js calls instance.getCandidatesCount()
2. Web3 sends eth_call to Ganache
3. Contract returns candidatesCount
4. Loop: for each candidate, call getCandidate(i)
5. Build HTML table with results
```

### Write Operations (Modify Data)

```
┌──────────┐      ┌──────────┐      ┌──────────┐      ┌──────────┐
│ User     │ ──▶  │ MetaMask │ ──▶  │ Ganache  │ ──▶  │ Contract │
│ Action   │      │ Signs TX │      │ Mines TX │      │ Updates  │
│          │      │          │      │          │      │ State    │
└──────────┘      └──────────┘      └──────────┘      └──────────┘
     │
     │ Example: Casting Vote
     │ ─────────────────────
     │ 1. User clicks vote button
     │ 2. app.js calls instance.vote(candidateId, {from: account})
     │ 3. MetaMask shows transaction popup
     │ 4. User confirms in MetaMask
     │ 5. Transaction sent to Ganache
     │ 6. Contract updates state
     │ 7. Event emitted: VoteCast(voter, candidateId)
     │ 8. Transaction receipt returned to app.js
     │ 9. UI updated with success message
```

---

## ⛓️ Smart Contract Mechanics

### State Variables

```solidity
address public admin;                          // Contract deployer
uint public candidatesCount;                   // Total candidates
uint public electionsCount;                    // Total elections

mapping(uint => Candidate) public candidates;  // Candidate data
mapping(address => bool) public hasVoted;      // Vote tracking
mapping(uint => Election) public elections;     // Election data
mapping(address => bool) public registeredVoters; // Voter whitelist
```

### Access Control

```
┌────────────────────────────────────────────────────────────────────┐
│                    FUNCTION ACCESS MATRIX                          │
├────────────────────────────────┬──────────┬──────────┬─────────────┤
│ Function                       │  Admin   │  Voter   │   Anyone    │
├────────────────────────────────┼──────────┼──────────┼─────────────┤
│ addCandidate()                 │    ✓     │    ✗     │      ✗      │
│ createElection()               │    ✓     │    ✗     │      ✗      │
│ registerVoter()                │    ✓     │    ✗     │      ✗      │
│ endElection()                  │    ✓     │    ✗     │      ✗      │
│ vote()                         │    ✗     │    ✓     │      ✗      │
│ getCandidate()                 │    ✓     │    ✓     │      ✓      │
│ getCandidatesCount()           │    ✓     │    ✓     │      ✓      │
│ getCurrentElection()           │    ✓     │    ✓     │      ✓      │
│ checkVote()                    │    ✓     │    ✓     │      ✓      │
│ getWinner()                    │    ✓     │    ✓     │      ✓      │
└────────────────────────────────┴──────────┴──────────┴─────────────┘
```

### Event Emissions

Events provide an audit trail on the blockchain:

| Event | Data | When Triggered |
|-------|------|----------------|
| `CandidateAdded` | candidateId, name, department | Admin adds candidate |
| `ElectionCreated` | electionId, title, startDate, endDate | Admin creates election |
| `VoterRegistered` | voterAddress | Admin registers voter |
| `VoteCast` | voterAddress, candidateId | Voter casts vote |

---

## 🔒 Security Mechanisms

### 1. Smart Contract Security

```
┌─────────────────────────────────────────────────────────────────┐
│ SECURITY FEATURE          │ IMPLEMENTATION                      │
├───────────────────────────┼─────────────────────────────────────┤
│ Admin-only functions      │ onlyAdmin modifier                  │
│ Double voting prevention  │ hasVoted mapping + hasNotVoted mod  │
│ Voter whitelist           │ registeredVoters mapping            │
│ Time-bound elections      │ electionIsActive modifier           │
│ Input validation          │ require() statements                │
│ Immutable vote records    │ Blockchain permanence               │
└─────────────────────────────────────────────────────────────────┘
```

### 2. Authentication Security

```
┌─────────────────────────────────────────────────────────────────┐
│ LAYER                     │ PROTECTION                          │
├───────────────────────────┼─────────────────────────────────────┤
│ API Authentication        │ JWT tokens with HS256 signature     │
│ Route Protection          │ Express middleware validates JWT    │
│ Password Storage          │ Database-stored credentials         │
│ CORS                      │ Restricted to localhost origins     │
│ Blockchain Auth           │ MetaMask signature verification     │
└─────────────────────────────────────────────────────────────────┘
```

### 3. Data Integrity

- **Votes are immutable**: Once recorded on blockchain, cannot be altered
- **Transparent counting**: Anyone can verify vote counts on-chain
- **Audit trail**: All actions logged in audit_logs table
- **No central authority**: Decentralized validation

---

## 📊 Result Computation

### Winner Determination

```solidity
function getWinner() public view returns (
    uint winnerId,
    string memory winnerName,
    string memory winnerDepartment,
    uint winnerVotes
) {
    uint highestVotes = 0;
    uint winningId = 0;
    
    for (uint i = 1; i <= candidatesCount; i++) {
        if (candidates[i].voteCount > highestVotes) {
            highestVotes = candidates[i].voteCount;
            winningId = i;
        }
    }
    
    return (
        candidates[winningId].id,
        candidates[winningId].name,
        candidates[winningId].department,
        candidates[winningId].voteCount
    );
}
```

### Real-time Updates

- Vote counts update immediately after transaction confirmation
- No manual counting required
- Results automatically computed from blockchain state

---

*Document Version: 1.0*
*Last Updated: December 2024*
*Project: Hawassa University Student Union Election System*
