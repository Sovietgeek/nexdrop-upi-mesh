# 🚀 NexDrop UPI: Mesh-Routed Offline Payments

**NexDrop UPI** ek cutting-edge Spring Boot solution hai jo financial inclusion ko extreme conditions (zero connectivity) mein reality banata hai. Ye system **Bluetooth Mesh Network** aur **Deferred Settlement** ka use karke payments ko un areas mein pahunchata hai jahan internet ka namo-nishan nahi hai.

---

## 🌟 Key Innovations

* **P2P Mesh Propagation:** Transaction packets ek phone se dusre phone par hop karte hain jab tak wo "Internet Zone" mein na pahunch jayein.
* **Cryptographic Vault:** AES-256-GCM aur RSA-2048 ka use karke payment ko "Tamper-Proof" banaya gaya hai.
* **Scalable Persistence:** Backend ab **MySQL 8.0** par based hai, jo real-world banking ledger ko mimic karta hai aur data ko permanent store karta hai.
* **Atomic Idempotency:** Duplicate delivery se bachne ke liye logic-level aur database-level dono jagah atomic checks hain.

---

## 🛠️ Deep Technical Stack

| Component | Technology | Role |
| :--- | :--- | :--- |
| **Language** | Java 17 (LTS) | High-performance backend logic |
| **Framework** | Spring Boot 3.3.5 | API Orchestration & Dependency Management |
| **Database** | MySQL 8.0 | ACID-compliant storage for Accounts & Ledger |
| **Security** | JCE (Java Cryptography) | Implementation of RSA-OAEP & AES-GCM |
| **UI/Frontend** | Thymeleaf + Tailwind | Real-time Dashboard with Mesh Simulation |

---

## 🔒 Security Architecture (The Three Pillars)

### 1. Hybrid Encryption Flow (End-to-End)
Sender ka phone payment ko encrypt karta hai jise raste mein koi read nahi kar sakta:
* **AES-256-GCM:** Payload (Amount/PIN) ko encrypt karta hai aur "Authentication Tag" generate karta hai.
* **RSA-OAEP:** Server ki Public Key se AES key ko wrap karta hai.
* **Integrity Check:** Agar intermediate node 1-bit bhi change karega, toh decryption fail ho jayegi (GCM Tag Mismatch).

### 2. Double-Spending & Replay Prevention
* **Nonce-based Hashing:** Har transaction ka ek unique UUID hota hai.
* **Idempotency Layer:** Server par `SHA-256(ciphertext)` ka use karke ek registry maintain hoti hai. Same hash do baar settle nahi ho sakta.

### 3. Database Integrity
MySQL table mein `packet_hash` par **Unique Constraint** lagayi gayi hai, jo system ka last line of defense hai.

---

## 📝 Demo Flow (Step-by-Step)

1.  **Compose:** Dashboard se Alice ke account se payment initiate karein.
2.  **Inject:** Packet `phone-alice` (Offline Node) mein inject hota hai.
3.  **Mesh Gossip:** `Gossip` button dabane par packet `stranger` nodes se hota hua `phone-bridge` tak pahunchta hai.
4.  **Gateway Sync:** `Sync` button dabane par `phone-bridge` packet ko MySQL Backend par POST karta hai.
5.  **Settlement:** Backend decrypt karke MySQL ledger mein balance update kar deta hai.

---

## 🚀 Future Roadmap & Scalability

Ise production-level product banane ke liye hum ye advanced features integrate karenge:

* **🔹 Real BLE Stack Integration:** Abhi ye virtual simulator hai. Future mein Android ke **Nearby Connections API** ka use karke real Bluetooth Low Energy mesh banaya jayega, jisse phones background mein automatically data exchange karenge.
* **🔹 Distributed Cache (Redis Implementation):** Abhi idempotency `ConcurrentHashMap` mein hai. Ise **Redis** se replace karenge taaki multiple server instances hone par bhi duplicate payment detect ho sake.
* **🔹 Zero-Knowledge Proofs (ZKP):** Security ko next level par le jaane ke liye ZKP use karenge, jisse bina transaction details reveal kiye ye prove kiya ja sake ki sender ke paas balance hai.
* **🔹 Offline Wallet (UPI Lite Style):** Device par ek pre-funded, hardware-secured wallet create karenge jisse real-time offline verification ho sake (Balance check offline).
* **🔹 Mesh Routing Optimization:** *Dijkstra's Algorithm* ya *AODV (Ad hoc On-Demand Distance Vector)* routing ka use karke sabse efficient path choose karna taaki battery consumption kam ho.

---

## ⚙️ Installation & Local Setup

### Step 1: Database Setup
```sql
CREATE DATABASE upimesh;
Step 2: Configure Properties
src/main/resources/application.properties mein:

Properties
spring.datasource.url=jdbc:mysql://localhost:3306/upimesh
spring.datasource.username=your_user
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
Step 3: Run the Engine
Bash
./mvnw spring-boot:run
👤 Author
Utkarsh Yadav Computer Science Engineering | Java Full Stack Developer

GitHub Profile | LinkedIn Profile
