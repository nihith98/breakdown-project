# Coding Conventions

Java/Spring patterns for the breakDown backend.

## Universal Rules

- ✅ **No Lombok** — Write getters, setters, toString manually
- ✅ **Field Injection** — @Autowired field injection only (no constructor)
- ✅ **@Component + @Scope(SINGLETON)** — Use for services (not @Service)
- ✅ **Log4j2** — `private static final Logger logger = LogManager.getLogger(...)`
- ✅ **application.properties** — Flat key-value (not YAML)
- ✅ **EnvironmentUtil** — All runtime config (not System.getenv directly)
- ✅ **ObjectMapperUtil** — Jackson for serialization
- ✅ **Native MongoDB** — No Spring Data repositories
- ✅ **BigDecimal** — For monetary values (never double/float)

## Architecture Patterns

### REST Layer
```java
@RestController
@RequestMapping("/api/endpoint")
public class MyController {
    private ResponseStructure generateResponseStructure(...);
}
```
- Controllers: thin, validate, delegate, return wrapper
- No exception handling (delegates to service)
- Always return ResponseStructure (HTTP 200)

### Service Layer
```java
@Component
@Scope(value = BeanDefinition.SCOPE_SINGLETON)
public class MyService {
    private MyDAO myDAO;
    
    public ResponseStructure execute(Input input) {
        try {
            // business logic
            return ResponseStructureUtil.success(...);
        } catch (SystemException e) {
            return ResponseStructureUtil.failure(e.getMessage());
        }
    }
}
```
- Access DB via delegators (not directly)
- Convert exceptions to response structures
- Catch and handle all exceptions

### DAO Layer
```java
@Component
public class MyDAO {
    private MongoDBOperations mongoOps;
    
    public Result find(Query query) {
        try {
            return mongoOps.find(...);
        } catch (MongoException e) {
            throw new SystemException("Error message");
        }
    }
}
```
- Native MongoDB driver (not Spring Data)
- Wrap all exceptions as SystemException
- Pure data access (no business logic)

## Testing Patterns

| Layer | Framework | Pattern |
|-------|-----------|---------|
| **REST** | JUnit5 + Mockito + MockMvc | `standaloneSetup`, assert `jsonPath` |
| **Service** | JUnit5 + Mockito | `@InjectMocks`, `MockedStatic` for converters |
| **DAO** | JUnit5 + Mockito | Mock `MongoDBOperations`, assert queries |

### Test Naming
```java
methodName_condition_expectedOutcome()
// Example:
login_validCredentials_returnsToken()
login_invalidPassword_returnsFAILURE()
```

### Test Structure
```java
@Test
void methodName_condition_expectedOutcome() {
    // Arrange
    User user = new User("test", "password");
    
    // Act
    ResponseStructure result = service.login(user);
    
    // Assert
    assertEquals("SUCCESS", result.getResponseStatus());
}
```

## Responses

Always use response wrappers:
```java
// Success
ResponseStructure.success(data, "Operation successful");

// Failure
ResponseStructure.failure("Error message");

// Always HTTP 200, outcome in status field
```

## Properties File Format
```properties
server.port=8080
spring.data.mongodb.uri=${MONGO_URI:mongodb://localhost:27017/breakdown}
app.jwt.secret=${JWT_SECRET:dev-secret}
```

Access with: `@Value("${key:defaultValue}")`

---

See **CLAUDE.md** in root for complete conventions.
