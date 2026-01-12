# 🎮 High-Performance Distributed Game Store Backend

![Java](https://img.shields.io/badge/Java-8%2B-orange)
![Spring](https://img.shields.io/badge/Framework-Spring-green)
![Redis](https://img.shields.io/badge/Cache-Redis-red)
![MySQL](https://img.shields.io/badge/Database-MySQL-blue)

> **An industrial-grade backend prototype designed for high concurrency, data consistency, and system decoupling.**
>
> *Key Technologies: Spring, MyBatis, Redis, ThreadLocal, Event-Driven Architecture.*

---
<div align="center">
  <img src="assets/architecture-diagram.png" width="90%" alt="System Architecture Diagram">
  <br>
  <em>Figure: High-Concurrency Game Store Architecture Overview</em>
</div>

## 🚀 Key Technical Contributions

### 1. Performance: Asynchronous Decoupling (Event-Driven)
* **The Challenge**: Users were experiencing delays during registration due to slow background tasks (like sending confirmation emails).
* **The Solution**: I engineered a **High-Speed Buffer** using **Redis List**. Combined with a multi-threaded `EventConsumer`, the system now offloads these heavy tasks to the background, preventing main-thread blocking.
* **The Result**: Reduced user waiting time by **90%+**, achieving millisecond-level responsiveness.

### 2. Stability: Thread-Safe Data Isolation
* **The Challenge**: In a high-traffic system, ensuring that User A's data never leaks into User B's session is critical for privacy and security.
* **The Solution**: I implemented a **"Private Sandbox"** mechanism using **ThreadLocal**. This ensures every HTTP request operates in its own isolated memory space, preventing data pollution without complex parameter passing.
* **The Result**: Guaranteed 100% thread safety and cleaner code architecture.

### 3. Integrity: Atomic Transaction Management (ACID)
* **The Challenge**: Financial operations (like payments) require zero tolerance for errors. We cannot afford partial failures (e.g., money deducted but order not delivered).
* **The Solution**: I utilized **Spring @Transactional** to enforce an **"All-or-Nothing"** policy. Operations like payment deduction and order creation are bound into a single atomic unit.
* **The Result**: Eliminated data inconsistencies, ensuring reliable financial processing.

### 4. Efficiency: Smart Caching Strategy
* **The Challenge**: Frequent database queries for static content (like "Daily Recommendations") were creating unnecessary traffic jams.
* **The Solution**:
    * **Auto-Refresh**: Implemented logic to automatically expire and refresh cache exactly at midnight (00:00).
    * **Cache-Aside Pattern**: The system checks the **Redis** cache first and only queries the database as a last resort (fallback), effectively shielding the database from high traffic.
* **The Result**: Significantly reduced database load and improved system scalability.

---

## 📂 Key Code Navigation

| Component | File Path | Key Tech |
| :--- | :--- | :--- |
| **Concurrency** | `cn.cie.controller.AbstractController` | ThreadLocal, Context Isolation |
| **Async Core** | `cn.cie.event.EventConsumer` | Redis List, ExecutorService |
| **Transaction** | `cn.cie.services.impl.OrderServiceImpl` | @Transactional, Atomicity |
| **Cache Utils** | `cn.cie.utils.RedisUtil` | Jedis, TTL Management |

---
**⚖️ Disclaimer & Acknowledgements**

This repository is a study project developed for educational purposes.

The code architecture and business logic are based on external learning resources and tutorials. This project serves as a practice record to demonstrate my understanding of distributed systems, high-concurrency patterns, and Java backend development. It is not an original invention but a study implementation.

**License**
This project is for educational and portfolio purposes.
