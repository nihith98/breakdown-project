# GitHub Repositories

All breakDown repositories with their status and purpose.

## Backend Services

| Repository | Purpose | Status | Tech |
|---|---|---|---|
| **[breakdown-dashboard-svc](https://github.com/YOUR-ORG/breakdown-dashboard-svc)** | Expense system REST API (WildFly deployment) | 🔄 In Progress | Java 21, Spring Boot 3.3, MongoDB |
| **[breakdown-dashboard](https://github.com/YOUR-ORG/breakdown-dashboard)** | Expense service orchestration layer | 🔄 In Progress | Java 21, Spring Boot (library) |
| **[breakdown-model](https://github.com/YOUR-ORG/breakdown-model)** | Shared data models and interfaces | 🔄 In Progress | Java 21 (pure library) |
| **[breakdown-mongo-adapter](https://github.com/YOUR-ORG/breakdown-mongo-adapter)** | MongoDB data access (DAOs) | 🔄 In Progress | Java 21, MongoDB driver |
| **[calculation-engine](https://github.com/YOUR-ORG/calculation-engine)** | Settlement calculation algorithm | 🚀 Pending | Java 21 (pure functions) |
| **[authentication-svc](https://github.com/YOUR-ORG/authentication-svc)** | Auth system REST API (WildFly deployment) | 🚀 Pending | Java 21, Spring Boot 3.3 |
| **[authentication](https://github.com/YOUR-ORG/authentication)** | Auth service orchestration layer | 🚀 Pending | Java 21 (library) |
| **[iam-operations](https://github.com/YOUR-ORG/iam-operations)** | IAM infrastructure (MongoDB + Kratos) | 🚀 Pending | Java 21, MongoDB, Ory Kratos |

## Documentation

| Repository | Purpose | Status |
|---|---|---|
| **[breakdown-project](https://github.com/YOUR-ORG/breakdown-project)** | Project overview & documentation | ✅ Active |

---

## Repository Setup Instructions

### Clone All Backend Repos
```bash
#!/bin/bash
cd ~/projects
git clone https://github.com/YOUR-ORG/breakdown-model.git
git clone https://github.com/YOUR-ORG/breakdown-mongo-adapter.git
git clone https://github.com/YOUR-ORG/calculation-engine.git
git clone https://github.com/YOUR-ORG/breakdown-dashboard.git
git clone https://github.com/YOUR-ORG/breakdown-dashboard-svc.git
git clone https://github.com/YOUR-ORG/iam-operations.git
git clone https://github.com/YOUR-ORG/authentication.git
git clone https://github.com/YOUR-ORG/authentication-svc.git
```

---

## Build Order (Important!)

When building locally, maintain this order due to dependencies:

**1. Core Models & Interfaces**
```bash
cd breakdown-model && mvn clean install
```

**2. Infrastructure & DAOs**
```bash
cd breakdown-mongo-adapter && mvn clean install
cd iam-operations && mvn clean install
```

**3. Business Logic**
```bash
cd calculation-engine && mvn clean install
```

**4. Service Layers**
```bash
cd breakdown-dashboard && mvn clean install
cd authentication && mvn clean install
```

**5. REST Services (WAR deployment)**
```bash
cd breakdown-dashboard-svc && mvn clean install
cd authentication-svc && mvn clean install
```

---

## How to Contribute

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Follow [Coding Conventions](./CODING_CONVENTIONS.md)
4. Write tests (80%+ coverage required)
5. Commit with meaningful messages (conventional commits)
6. Push and create a Pull Request

See individual repo READMEs for setup instructions.

---

**Replace `YOUR-ORG` with your actual GitHub organization/username.**
