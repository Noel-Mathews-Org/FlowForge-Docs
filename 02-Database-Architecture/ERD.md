# Entity Relationship Diagram

[← Back to Database Architecture](Overview.md)

While each service maintains its own logical database, the following conceptual ERD shows the relationships between entities across the entire FlowForge platform.

*Note: Cross-service relationships (e.g., a Task referencing a Project) are enforced logically in the application code rather than via hard foreign keys at the database level.*

```mermaid
erDiagram
    %% Auth Service DB
    USER ||--o{ INVITATION : "sends"
    USER ||--o{ NOTIFICATION : "receives"
    USER ||--o{ REFRESH_TOKEN : "owns"
    
    USER {
        uuid id PK
        string email
        string password_hash
        string entra_oid
        string role
        boolean is_active
    }
    
    %% Project Service DB
    PROJECT ||--o{ PROJECT_MEMBER : "has"
    PROJECT {
        uuid id PK
        string name
        uuid manager_id FK "References User.id"
        boolean is_archived
    }
    PROJECT_MEMBER {
        uuid project_id FK
        uuid user_id FK "References User.id"
    }

    %% Task Service DB
    PROJECT ||--o{ TASK : "contains"
    TASK ||--o{ TASK_COMMENT : "has"
    TASK ||--o{ APPROVAL_REQUEST : "requires"
    
    TASK {
        uuid id PK
        uuid project_id FK "References Project.id"
        string title
        string description
        string status "TODO, IN_PROGRESS, DONE, BLOCKED"
        uuid assignee_id FK "References User.id"
        boolean needs_approval
    }

    %% Analysis Service DB
    AUDIT_EVENT {
        uuid id PK
        string event_type
        uuid actor_id FK "References User.id"
        jsonb payload
        timestamp created_at
    }
    
    AI_USAGE_LOG {
        uuid id PK
        uuid project_id FK
        int token_count
        float estimated_cost
    }
```
