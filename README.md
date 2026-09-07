# 💸 Peer-to-Peer Payment Application

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Bun](https://img.shields.io/badge/Bun-000000?style=for-the-badge&logo=bun&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
 
A high-performance, secure Peer-to-Peer (P2P) payment platform and digital wallet system. This application enables users to instantly transfer funds to peers using simplified unique identifiers, manage a virtual wallet balance, and execute secure transactions backed by robust multi-factor authentication.

## ✨ Key Features

- **Instant P2P Transfers**: Send and receive money in real-time without processing delays.
- **Flexible Identifiers**: Look up peers and route payments securely using a mobile number, email address, or custom username.
- **Digital Wallet Management**: Integrated virtual wallet to store user funds natively. Allows users to hold a balance and make lightning-fast payments without repeatedly querying external bank APIs.
- **Enterprise-Grade Security**: 
  - Multi-Factor Authentication (MFA) including Biometrics, PIN codes, and OTPs (One-Time Passwords) for transaction signing.
  - End-to-end SSL encryption for all data in transit.
- **Fiat On/Off Ramping**: Integrated with Stripe to allow users to add funds from their bank/card into the digital wallet or withdraw back to their bank.

## 🛠️ Technology Stack

### Frontend
* **React & TypeScript**: For a highly interactive, type-safe, and scalable user interface.
* **State Management**: Context API / Custom Hooks for managing wallet state.

### Backend & Infrastructure
* **Bun & Node.js / Express.js**: High-speed JavaScript runtime environments handling the core REST API and business logic.
* **PostgreSQL**: Primary relational database ensuring strict ACID compliance for all financial ledgers and transaction records.
* **Prisma ORM**: Type-safe database access, schema migrations, and relational queries.
* **Redis**: In-memory data store used for rate-limiting, session management, and caching frequent user queries.
* **Stripe API**: Payment gateway integration for external funding.

## 🏗️ Architecture Overview

The system is built on a service-oriented architecture. The Express.js backend handles client requests and enforces validation schemas. 
Financial ledgers are maintained in PostgreSQL using atomic transactions (`prisma.$transaction`) to prevent race conditions during concurrent transfers (e.g., preventing a double-spend scenario). 
Redis acts as a speed layer for OTP verification and rate-limiting brute-force login attempts. 

## 🚀 Getting Started

### Prerequisites
* [Bun](https://bun.sh/) (v1.0+)
* [Node.js](https://nodejs.org/) (v20+)
* [PostgreSQL](https://www.postgresql.org/) (Running locally or via Docker)
* [Redis](https://redis.io/)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Himanshu-022001/p2p-payment-app.git
   cd p2p-payment-app
   ```

2. **Install dependencies**
   ```bash
   bun install
   ```

3. **Set up Environment Variables**
   Create a `.env` file in the root directory based on the `.env.example`:
   ```env
   # Database Connections
   DATABASE_URL="postgresql://user:password@localhost:5432/p2p_wallet"
   REDIS_URL="redis://localhost:6379"
   
   # Security
   JWT_SECRET="your_super_secret_jwt_key"
   SESSION_SECRET="your_session_secret"
   
   # Third Party API Keys
   STRIPE_SECRET_KEY="sk_test_..."
   STRIPE_WEBHOOK_SECRET="whsec_..."
   ```

4. **Initialize the Database**
   Run Prisma migrations to set up the SQL schema:
   ```bash
   bunx prisma migrate dev --name init
   bunx prisma generate
   ```

5. **Start the Development Server**
   ```bash
   # Run backend
   bun run dev:server
   
   # Run frontend (in a separate terminal)
   bun run dev:client
   ```

## 🔒 Security Practices Implemented
* **ACID Transactions**: All database writes involving money movement are wrapped in strict SQL transactions. If a receiver's account update fails, the sender's debit rolls back automatically.
* **Concurrency Control**: Implemented row-level locking during balance updates to prevent race conditions.
* **Input Sanitization & Validation**: Zod/Joi schemas are utilized to validate incoming payloads and prevent injection attacks.
* **Token Expiration**: JWTs are short-lived, with a secure HTTP-only refresh token mechanism.
