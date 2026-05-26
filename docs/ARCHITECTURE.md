# Architecture Reference

High-level system design for the breakDown platform.

## System Overview

```
┌──────────────────────────────────────────────┐
│  API Gateway (Token validation, Rate limits) │
└──────────┬──────────────────┬────────────────┘
           │                  │
    ┌──────▼────────┐  ┌──────▼──────────┐
    │ Expense SVC   │  │ Auth SVC        │
    │ (Dashboard)   │  │ (Identity)      │
    └──────┬────────┘  └──────┬──────────┘
           │                  │
           └────────┬─────────┘
                    │
            ┌───────▼─────────┐
            │  MongoDB 6.0+   │
            │  - Users        │
            │  - Groups       │
            │  - Transactions │
            │  - Tokens       │
            └─────────────────┘
```

## Module Structure

### Expense System
```
breakdown-dashboard-svc (WAR, WildFly 35)
    └─ breakdown-dashboard (service layer)
        ├─ breakdown-model (shared types)
        ├─ breakdown-mongo-adapter (DAOs)
        └─ calculation-engine (settlement computation)
```

### Authentication System
```
authentication-svc (WAR, WildFly 35)
    └─ authentication (service layer)
        └─ iam-operations (IAM infrastructure)
```

## Frontend Architecture

### Frontend Stack
```
breakDown-ui (Next.js 15, React 19, TypeScript)
    ├─ Server Components (data fetching, server-side rendering)
    ├─ Client Components (interactive UI, form handling)
    ├─ Server Actions (mutations, form submissions)
    ├─ API Routes (middleware to Java backend)
    ├─ Design System (CSS tokens, component library)
    └─ Configuration (environment-based, no feature flags)
```

### Target Devices & Platforms
- **Web** (responsive, primary platform)
- **PWA** (installable, offline-capable)
- **iOS** (future: SwiftUI, safe areas, Keychain integration)
- **Android** (future: Jetpack Compose, Material Design)

### Responsive Breakpoints
- **Desktop** (1024px+): Side-by-side hero + form layout
- **Tablet** (768px–1023px): Stacked vertical layout
- **Mobile** (<768px): Full-width, single column

### Frontend-to-Backend Integration
All data flows through **API routes** (`app/api/`) that:
- Accept client requests (from server components, client forms)
- Call Java backend via `apiClient` (Axios wrapper)
- Parse `ResponseStructure<T>` response contract
- Return unwrapped `responseObject` to frontend

Example flow:
```
Client Component (form) → Server Action → API Route → Java Backend
                                             ↓
                          ResponseStructure { responseStatus, responseObject }
                                             ↓
                          API Route unwraps responseObject
                                             ↓
                          Client receives typed data
```

### Design System Integration
Frontend uses **breakDown design tokens** (CSS custom properties):
- Colors: `--bg-page`, `--surface-card`, `--accent-highlight`, `--money-positive`, `--money-negative`
- Typography: `--font-sans` (Space Grotesk), `--font-mono` (JetBrains Mono)
- Spacing: `4px` base unit with `--space-*` scale (xs/sm/md/lg/xl)
- Transitions: `--transition-fast` (0.12s), `--transition-med` (0.2s)
- Theme: Dark (default) and Light modes via `data-theme` attribute

All components use design tokens to maintain consistency across platforms.

## Layering Rules

**REST Layer**: Controllers are thin. Validate, delegate, return `ResponseStructure`.  
**Service Layer**: Orchestrate business logic. Access DB via delegators. Convert exceptions to response structures.  
**Infrastructure Layer**: Direct MongoDB/IAM access. Wrap errors as SystemException/IAMException.  
**Computation Layer**: Pure functions with no side effects.

## Key Patterns

- **DI**: @Autowired field injection (no constructor injection)
- **Components**: @Component + @Scope(SINGLETON) for services
- **Logging**: Log4j2 (not SLF4J)
- **Config**: application.properties + EnvironmentUtil
- **DB**: Native MongoDB driver (not Spring Data)
- **Serialization**: ObjectMapperUtil (Jackson)
- **Responses**: Always HTTP 200, outcome via SUCCESS/FAILURE
- **Errors**: Handled locally, no @ControllerAdvice

See **CLAUDE.md** for complete coding conventions.

---

**See individual repository READMEs for setup instructions.**
