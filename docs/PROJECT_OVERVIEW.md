# breakDown — Privacy-First Expense Splitting Platform

> A multi-platform, privacy-conscious expense management system for trips, families, and friend groups. Designed with technical precision and a focus on fair, transparent settlements.

**Status**: Active Development | **License**: MIT | **Tech Stack**: Java (Backend) + Web/PWA/iOS/Android (Frontend)

---

## 🎯 Project Vision

**breakDown** is built on a single conviction: expense splitting should be transparent, fair, and private.

Unlike competitors, breakDown:
- **Collects minimal PII** — username only, no emails, phone numbers, or tracking
- **Settles families together** — family units settle as one collective entity, reducing transaction overhead
- **Optimizes settlements mathematically** — computes the fewest transactions needed to clear all debts
- **Handles recurring expenses** — subscriptions and monthly bills defined once, factored into settlements automatically
- **API-first design** — REST endpoints designed for web, PWA, iOS, and Android clients

---

## 📊 Project Status

### ✅ Completed (Phase 1: Architecture & Design)

**Backend Architecture**
- Multi-module Java/Spring Boot design with clear separation of concerns
  - Expense System: Dashboard service, data models, MongoDB adapter, settlement calculation engine
  - Authentication System: Identity service, JWT token management, IAM routing
- RESTful API contracts defined
- Error handling patterns established (HTTP 200 with SUCCESS/FAILURE response status)
- Dependency injection and bean management configured
- No Lombok, no Spring Data — native MongoDB driver + manual DTOs

**User Stories & API Documentation**
- **Authentication**: Login, Logout, Token Refresh
- **Registration**: User account creation with data minimization
- **Group Management**: Create groups, add/remove members, define family units
- **Expense Tracking**: Record transactions, fetch settlement lists, query transaction history
- All stories documented with input/output contracts, success/error scenarios, and edge cases

**UX Design & Branding**
- Brand philosophy established (privacy-first, technical precision, professional tone)
- Design system documented (glassmorphism, dark/light themes, WCAG AA accessibility)
- Login page UX spec complete (responsive web, PWA, iOS, Android with multi-theme support)
- UX-to-spec automation skill created for future feature designs

**Development Infrastructure**
- CLAUDE.md coding conventions (no Lombok, DI patterns, logging, properties, error handling)
- architecture.md module dependency graphs and layering rules
- api-reference.md REST endpoint specifications
- build-and-deploy.md build order and WildFly deployment guidance
- development-guide.md step-by-step feature addition checklists

---

### 🔄 In Progress (Phase 2: Implementation)

**Backend Services**
- [ ] Expense system REST controllers and service layer
- [ ] Authentication system login/logout/refresh endpoints
- [ ] MongoDB schema and DAO implementations
- [ ] Settlement calculation algorithm (optimal transaction graph)
- [ ] Token generation and validation (RS256 JWT, refresh token rotation)

**Testing & Validation**
- [ ] Unit tests for services and DAOs (JUnit5, Mockito)
- [ ] Integration tests with MongoDB
- [ ] API contract testing (request/response validation)
- [ ] UX acceptance testing (browser, iOS simulator, Android emulator)

---

### 🚀 Future (Phase 3+: Polish & Growth)

**Advanced Features**
- [ ] Password reset and account recovery flow
- [ ] Multi-factor authentication (TOTP, biometric)
- [ ] Expense categorization and analytics
- [ ] Recurring expense scheduling (auto-calculate settlements)
- [ ] Family hierarchy and role-based permissions
- [ ] Expense splitting by custom percentages (not just equal)

**Frontend & Platform**
- [ ] Web/PWA frontend (responsive UI, dark/light themes, glassmorphism)
- [ ] iOS native app (SwiftUI, Keychain token storage)
- [ ] Android native app (Jetpack Compose, EncryptedSharedPreferences)
- [ ] Cross-platform sync (real-time)
- [ ] Offline transaction queue (works without connectivity)
- [ ] Export/backup functionality (user data portability)
- [ ] Audit logging and compliance (GDPR, privacy)
- [ ] Mobile app store distribution (App Store, Google Play)

**Operations & Scaling**
- [ ] CI/CD pipeline (automated testing, deploy to staging/production)
- [ ] Monitoring and error tracking (Sentry, Datadog)
- [ ] Performance optimization (caching, pagination, query optimization)
- [ ] Infrastructure as code (Docker, Kubernetes, cloud deployment)

**Business & Growth**
- [ ] Analytics dashboard for product insights
- [ ] Referral program and user growth
- [ ] Premium features (advanced analytics, priority support)
- [ ] API for third-party integrations

---

## 🏗️ Architecture Overview

### System Design

```
┌───────────────────────────────────────────────────────────┐
│                                                         │
│  API Gateway / Load Balancer                           │
│  ├─ Token validation (JWKS endpoint)                   │
│  ├─ Rate limiting                                       │
│  └─ Request routing                                     │
│                                                         │
└──────────┬──────────────────────────────┬──────────────┘
           │                              │
           │                              │
┌──────────▼────────────┐    ┌───────────▼──────────────┐
│                       │    │                          │
│ Expense System        │    │ Authentication System    │
│ (Dashboard SVC)       │    │ (Auth SVC)              │
│                       │    │                          │
│ ├─ Dashboard service  │    │ ├─ Login/Logout        │
│ ├─ Settlement engine  │    │ ├─ Token management    │
│ ├─ DB routing         │    │ ├─ IAM delegation      │
│ └─ REST controllers   │    │ └─ REST controllers    │
│                       │    │                          │
└──────────┬────────────┘    └────────────┬─────────────┘
           │                              │
           │    ┌───────────────────────┐ │
           │    │                       │ │
           └────▼─────────────────────┬─┘ │
                │                     │   │
                │   MongoDB 6.0+      │   │
                │  ├─ Users           │   │
                │  ├─ Groups          │   │
                │  ├─ Transactions    │   │
                │  ├─ Settlements     │   │
                │  ├─ Sessions        │   │
                │  └─ Tokens          │   │
                │                     │   │
                └─────────────────────┘   │
                                          │
                 Alternative IAM: Ory Kratos (optional)
                 (User management, identity federation)
```

### Module Structure

**Expense System**
```
breakdown-dashboard-svc (WAR, deployed to WildFly 35)
    └─ breakdown-dashboard (service layer)
        ├─ breakdown-model (shared types)
        ├─ breakdown-mongo-adapter (MongoDB infrastructure)
        └─ calculation-engine (settlement computation)
```

**Authentication System**
```
authentication-svc (WAR, deployed to WildFly 35)
    └─ authentication (service layer)
        └─ iam-operations (IAM infrastructure + shared types)
            ├─ MongoDB backend
            └─ Ory Kratos integration (optional)
```

---

## 📚 Documentation

| Document | Purpose |
|----------|---------|
| **`CLAUDE.md`** | Coding conventions, patterns, DI strategy, test organization |
| **`architecture.md`** | Module roles, dependency graphs, layering rules, error handling |
| **`api-reference.md`** | REST endpoint specifications, request/response contracts, error scenarios |
| **`build-and-deploy.md`** | Build order, WildFly deployment, environment variables, CI/CD |
| **`development-guide.md`** | Step-by-step checklists for adding new features |
| **`branding/branding-philosophy.md`** | Brand values, user archetypes, tone guidelines, design principles |
| **`branding/frontend-implementation-guide.md`** | Color palette, typography, spacing, component specs, accessibility |
| **`user_stories/`** | Feature requirements by user intent, API contracts, error handling |
| **`ux-spec/`** | Detailed UX specifications for each feature (responsive design, multi-platform) |

---

## 🔗 GitHub Repositories

| Repo | Purpose | Status |
|------|---------|--------|
| **[breakdown-dashboard-svc](https://github.com/YOUR-ORG/breakdown-dashboard-svc)** | Expense system REST entry point | In Progress |
| **[breakdown-dashboard](https://github.com/YOUR-ORG/breakdown-dashboard)** | Service orchestration layer | In Progress |
| **[breakdown-model](https://github.com/YOUR-ORG/breakdown-model)** | Shared data models & interfaces | In Progress |
| **[breakdown-mongo-adapter](https://github.com/YOUR-ORG/breakdown-mongo-adapter)** | MongoDB data access | In Progress |
| **[calculation-engine](https://github.com/YOUR-ORG/calculation-engine)** | Settlement computation | Pending |
| **[authentication-svc](https://github.com/YOUR-ORG/authentication-svc)** | Authentication REST entry point | Pending |
| **[authentication](https://github.com/YOUR-ORG/authentication)** | IAM service layer | Pending |
| **[iam-operations](https://github.com/YOUR-ORG/iam-operations)** | IAM infrastructure & DAOs | Pending |

*Note: Replace `YOUR-ORG` with your actual GitHub organization.*

---

## 🎨 Design System

### Dark Mode (Default)
- **Base**: Deep navy gradient `#0d1117` → `#1a0f2e`
- **Text**: Light gray `#e5e7eb` (primary), `#9ca3af` (secondary)
- **Accent**: Indigo `#6366f1` (interactive), Lavender `#a78bfa` (hover)
- **Status**: Green `#22c55e` (owed to you), Red `#ef4444` (you owe)
- **Card Style**: Glassmorphism (blur 10px, rgba indigo with 0.1 opacity)

### Light Mode (Alternative)
- **Base**: Off-white gradient `#f5f3ff` → `#faf8ff`
- **Text**: Dark gray `#1f2937` (primary), `#4b5563` (secondary)
- **Colors**: Same as dark mode inverted for contrast
- **Accessibility**: WCAG AA compliance (minimum 4.5:1 contrast)

### Typography
- **Sans-serif**: System fonts (`-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, `Roboto`)
- **Monospace**: `Courier New`, `SF Mono` (for data displays, amounts, code)
- **Headings**: H1 (32px, 600wt), H2 (24px, 600wt), H3 (18px, 600wt)
- **Body**: 14px, 400 weight, 1.6 line-height

---

## 👥 Team & Contributions

| Role | Responsible |
|------|-------------|
| **Architecture & Backend** | [Your Name] |
| **Frontend & UX Design** | [Your Name] |
| **Database & Infrastructure** | [Your Name] |
| **Product & Design Systems** | [Your Name] |

---

## 📈 Key Metrics & Milestones

### Phase 1: Architecture & Design ✅
- [x] Architecture defined and documented
- [x] User stories written for 10 features
- [x] UX specifications created (Login page complete)
- [x] Design system documented
- [x] Coding conventions established
- **Timeline**: April — May 2026
- **Outcome**: Complete blueprint for development

### Phase 2: Implementation 🔄
- [ ] Backend services implemented
- [ ] Integration and end-to-end testing
- **Timeline**: June — September 2026 (estimated)
- **Success Criteria**: All user stories passing acceptance tests, 80%+ code coverage

### Phase 3: Frontend & Launch 🚀
- [ ] Web/PWA frontend implementation
- [ ] iOS and Android native apps
- [ ] Production deployment to cloud
- [ ] Mobile app store distribution
- [ ] User onboarding and analytics
- **Timeline**: October 2026 onwards
- **Vision**: 10,000+ active users tracking 1M+ transactions

---

## 🛠️ Tech Stack

### Backend
- **Language**: Java 21+
- **Framework**: Spring Boot 3.3+
- **ORM/Database**: Native MongoDB Java driver (no Spring Data)
- **Build**: Maven 3.8+
- **Deployment**: WildFly 35 (WAR format)
- **Testing**: JUnit 5, Mockito, TestContainers

### Infrastructure
- **Database**: MongoDB 6.0+ (Atlas or self-hosted)
- **Identity**: Ory Kratos (optional, default: MongoDB)
- **Deployment**: Docker + Kubernetes (future)
- **Monitoring**: TBD (Sentry, Datadog, etc.)

---

## 🔒 Security & Privacy

### Privacy Guarantees
- **Minimal data collection**: Username only during signup, no tracking or analytics PII
- **No third-party sharing**: Data never sold or shared with advertisers or third parties
- **User control**: Users can export, backup, or delete their data at any time
- **Transparent storage**: All transactions stored securely on owned/controlled servers

### Security Practices
- **Authentication**: RS256 JWT access tokens (15-min TTL), opaque refresh tokens (90-day sliding window)
- **Storage**: Passwords hashed with bcrypt; refresh tokens SHA-256 hashed in DB (never raw tokens)
- **Transport**: HTTPS/TLS 1.3+ for all connections
- **Session Management**: HttpOnly, Secure, SameSite=Strict cookies (web); encrypted app-managed tokens (mobile)
- **Error Messages**: Generic authentication errors prevent username enumeration attacks
- **Rate Limiting**: Per-IP and per-user limits to prevent brute-force attacks (future)

---

## 🤝 Contributing

This is a portfolio project showcasing full-stack design and development. Contributions welcome!

### Getting Started
1. Review `CLAUDE.md` for coding conventions
2. Read `architecture.md` for module structure
3. Check `development-guide.md` for feature checklists
4. See individual repo READMEs for setup instructions

### Code Review Process
- All changes require peer review
- Tests must pass (JUnit 5, 80%+ coverage)
- CLAUDE.md conventions must be followed
- Commit messages follow conventional commits format

---

## 📞 Contact & Support

| Channel | Purpose |
|---------|---------|
| **GitHub Issues** | Bug reports, feature requests |
| **GitHub Discussions** | Questions, design discussions |
| **Email** | nihithsistla98@gmail.com |

---

## 📄 License

This project is licensed under the MIT License. See `LICENSE` file for details.

---

## 🎓 What This Project Demonstrates

✨ **Full-Stack System Design**
- Multi-tier architecture with clear separation of concerns
- REST API design and contract-driven development
- Database schema design and query optimization

✨ **Design System & UX Specification**
- Accessibility-first approach (WCAG AA compliance)
- Dark/light theme design system with complete color, typography, and spacing specifications
- Glassmorphism visual language and component library defined
- UX specifications for multi-platform (web, iOS, Android) authored and ready for implementation

✨ **Backend Engineering**
- Java/Spring Boot microservices
- MongoDB data modeling and querying
- JWT token management and authentication flows
- Error handling and resilience patterns

✨ **Software Craftsmanship**
- Comprehensive documentation (architecture, API, development guides)
- Coding conventions and patterns (no code generation, manual DI, native drivers)
- Test-driven development practices
- Automated UX specification generation

---

**Last Updated**: May 20, 2026  
**Version**: 1.0 — Architecture & Design Complete

---

## 📚 Quick Links

- 🏗️ [Architecture Reference](./architecture.md)
- 📖 [API Reference](./api-reference.md)
- 🎨 [Branding & Design System](./branding/branding-philosophy.md)
- 📋 [User Stories](./user_stories/index.md)
- 🎯 [UX Specifications](./ux-spec/)
- 👨‍💻 [Development Guide](./development-guide.md)
- 🚀 [Build & Deploy](./build-and-deploy.md)
- ⚙️ [Coding Conventions](./CLAUDE.md)
