# BitPass Identity Protocol

A **Bitcoin-native decentralized identity and authentication system** built on the Stacks blockchain.
BitPass enables users to create, manage, and verify **self-sovereign digital identities** with Bitcoin-backed security and without relying on centralized authorities.

---

## 🌐 System Overview

BitPass provides a **trustless identity layer** for Bitcoin-powered applications. Users can:

* **Register unique identities** with immutable username ownership.
* **Manage personal data** (email, profile image) under full self-custody.
* **Authenticate across applications** without relying on centralized identity providers.
* **Ensure tamper-proof records** by anchoring identity state into Bitcoin through Stacks consensus.

### Key Features

* **Self-Sovereign Identity (SSI):** Users own and control their identity data.
* **Global Uniqueness:** Prevents duplicate usernames across the protocol.
* **Cross-App Authentication:** Applications can query BitPass for secure, verifiable identity.
* **Bitcoin Security:** Inherits Bitcoin’s immutability and consensus finality via Stacks.

---

## ⚙️ Contract Architecture

The contract is written in **Clarity** and centers around three core components:

### **1. Data Structures**

* `users` → Maps a principal (address) to identity metadata:

  ```clarity
  {
    username: string-ascii (50),
    email: string-ascii (100),
    profile-image: optional string-utf8 (256)
  }
  ```

* `taken-usernames` → Tracks reserved usernames to enforce global uniqueness.
* `user-count` → Maintains total registered user count.

---

### **2. Public Functions**

* **Identity Lifecycle**

  * `register-user` → Registers a new identity (username + email).
  * `update-profile` → Updates username and/or email.
  * `delete-profile` → Permanently deletes identity.

* **Profile Management**

  * `set-profile-image` → Sets or updates a profile image.
  * `clear-profile-image` → Removes the profile image.

---

### **3. Read-Only Queries**

* `get-user-info (user principal)` → Returns full identity record.
* `get-user-count` → Returns total registered users.
* `is-user-registered (user principal)` → Checks if a user is registered.
* `is-username-available (username)` → Validates username availability.

---

## 🔄 Data Flow

1. **User Registration**

   * User calls `register-user` with username + email.
   * Contract validates input (length, format, uniqueness).
   * Record stored in `users` and username reserved in `taken-usernames`.

2. **Profile Update**

   * User modifies email or username via `update-profile`.
   * If username changes, old username is freed, new one reserved.

3. **Authentication / Queries**

   * Applications query `get-user-info` or `is-user-registered` to verify identity.

4. **Deletion**

   * User calls `delete-profile`.
   * Identity record is removed, count decremented, username freed.

---

## 🛡️ Security Model

* **Username Uniqueness:** Enforced at registration and updates via `taken-usernames`.
* **Tamper-Proof Records:** State anchored to Bitcoin via Stacks consensus.
* **Input Validation:** Strict constraints on username, email, and image URLs.
* **Self-Sovereignty:** Only `tx-sender` (identity owner) can modify their profile.

---

## 📦 Deployment & Integration

### Deployment

* Deploy the Clarity contract on Stacks mainnet or testnet.
* Initialize without additional configuration.

### Integration

Applications can integrate by:

* Querying `get-user-info` for authentication.
* Using `is-username-available` to check identity creation.
* Verifying self-sovereign ownership by matching `tx-sender` with stored principal.

---

## 🚀 Use Cases

* **Decentralized Applications (dApps):** Enable login/verification via BitPass.
* **Social Protocols:** Ensure globally unique, user-owned handles.
* **Payments & Commerce:** Associate Bitcoin addresses with verified identities.
* **Reputation Systems:** Build trust layers on top of immutable identity records.
