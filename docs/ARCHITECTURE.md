# Architecture Reference

High-level system design for the breakDown platform.

## System Overview

```
┌──────────────────────────────────────────────┐
│  Frontend: Web/PWA, iOS, Android             │
│  (Glasmorphic UI, Dark/Light Themes)         │
└────────────────┬─────────────────────────────┘
                 │ HTTPS/TLS 1.3+
┌────────────────▼─────────────────────────────┐
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
