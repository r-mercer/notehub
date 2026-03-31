
I've created an Entity Relationship Diagram (ERD) that visualizes the database schema. The diagram shows:
Central Relationships:

Users create approval requests and are assigned approval tasks
Approval requests contain multiple approval tasks (one-to-many)
Each task is assigned to a specific user

Supporting Relationships:

Approval history tracks all actions on both tasks and requests
Attachments link to approval requests
Notifications connect users to their tasks and requests
Workflows define templates with multiple steps

Key Visual Elements:

Primary keys (PK) and foreign keys (FK) are marked in each table
Relationship lines show how tables connect
Cardinality is indicated (one-to-many relationships with ||--o{)

The diagram makes it easy to see how data flows through the system, from a user creating a request, through task assignment and approval, to the complete audit trail in the history table.

erDiagram
    users ||--o{ approval_requests : "creates"
    users ||--o{ approval_tasks : "assigned to"
    users ||--o{ approval_history : "performs action"
    users ||--o{ attachments : "uploads"
    users ||--o{ notifications : "receives"
    users ||--o{ workflow_steps : "can be approver"
    
    approval_requests ||--o{ approval_tasks : "has"
    approval_requests ||--o{ approval_history : "tracks"
    approval_requests ||--o{ attachments : "has"
    approval_requests ||--o{ notifications : "triggers"
    
    approval_tasks ||--o{ approval_history : "logged in"
    approval_tasks ||--o{ notifications : "generates"
    
    approval_workflows ||--o{ workflow_steps : "contains"
    
    users {
        int user_id PK
        varchar username
        varchar email
        varchar first_name
        varchar last_name
        varchar role
        varchar department
        boolean is_active
        timestamp created_at
        timestamp updated_at
    }
    
    approval_requests {
        int request_id PK
        int requester_id FK
        varchar title
        text description
        varchar request_type
        varchar priority
        varchar status
        decimal amount
        date due_date
        timestamp created_at
        timestamp updated_at
    }
    
    approval_tasks {
        int task_id PK
        int request_id FK
        int assigned_to FK
        int sequence_order
        varchar status
        text comments
        boolean is_required
        timestamp assigned_at
        timestamp completed_at
        timestamp created_at
        timestamp updated_at
    }
    
    approval_history {
        int history_id PK
        int task_id FK
        int request_id FK
        int user_id FK
        varchar action
        text comments
        varchar previous_status
        varchar new_status
        timestamp created_at
    }
    
    approval_workflows {
        int workflow_id PK
        varchar workflow_name
        text description
        varchar request_type
        boolean is_active
        boolean requires_sequential_approval
        timestamp created_at
        timestamp updated_at
    }
    
    workflow_steps {
        int step_id PK
        int workflow_id FK
        int step_order
        varchar approver_role
        int approver_user_id FK
        boolean is_required
        text description
        timestamp created_at
    }
    
    attachments {
        int attachment_id PK
        int request_id FK
        varchar file_name
        varchar file_path
        int file_size
        varchar mime_type
        int uploaded_by FK
        timestamp uploaded_at
    }
    
    notifications {
        int notification_id PK
        int user_id FK
        int request_id FK
        int task_id FK
        varchar notification_type
        text message
        boolean is_read
        timestamp created_at
    }
