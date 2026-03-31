

I've created a comprehensive database schema for an approvals application with the following key components:
Core Tables:

users - Stores user information including roles and departments
approval_requests - Main table for approval requests with status tracking
approval_tasks - Individual assignable tasks linked to requests and users, with sequence ordering for multi-step approvals
approval_history - Complete audit trail of all actions taken on tasks

Workflow Management:

approval_workflows - Templates for different approval processes
workflow_steps - Defines the steps and required approvers for each workflow

Supporting Tables:

attachments - File storage for supporting documents
notifications - Tracks notifications sent to users about their tasks

Key Features:

Tasks can be assigned to specific users with sequence ordering
Support for both parallel and sequential approval processes
Complete audit trail through the history table
Flexible workflow system that can be customized per request type
Foreign key relationships ensure data integrity
Indexes on frequently queried columns for performance

The schema handles various approval scenarios like expense approvals, time-off requests, purchase orders, and document approvals. You can extend the request types as needed for your specific use case.


Data
``` SQL 

-- Users table: stores information about users in the system
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    role VARCHAR(50), -- e.g., 'admin', 'manager', 'employee'
    department VARCHAR(100),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Approval requests table: stores the main approval request details
CREATE TABLE approval_requests (
    request_id INT PRIMARY KEY AUTO_INCREMENT,
    requester_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    request_type VARCHAR(50), -- e.g., 'expense', 'time_off', 'purchase', 'document'
    priority VARCHAR(20) DEFAULT 'medium', -- 'low', 'medium', 'high', 'urgent'
    status VARCHAR(50) DEFAULT 'pending', -- 'pending', 'approved', 'rejected', 'cancelled'
    amount DECIMAL(10, 2), -- for financial approvals
    due_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (requester_id) REFERENCES users(user_id)
);

-- Approval tasks table: individual approval tasks assigned to users
CREATE TABLE approval_tasks (
    task_id INT PRIMARY KEY AUTO_INCREMENT,
    request_id INT NOT NULL,
    assigned_to INT NOT NULL,
    sequence_order INT DEFAULT 1, -- for sequential approvals
    status VARCHAR(50) DEFAULT 'pending', -- 'pending', 'approved', 'rejected', 'skipped'
    comments TEXT,
    is_required BOOLEAN DEFAULT true, -- whether this approval is mandatory
    assigned_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    completed_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (request_id) REFERENCES approval_requests(request_id) ON DELETE CASCADE,
    FOREIGN KEY (assigned_to) REFERENCES users(user_id)
);

-- Approval history table: tracks all actions taken on approval tasks
CREATE TABLE approval_history (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    task_id INT NOT NULL,
    request_id INT NOT NULL,
    user_id INT NOT NULL,
    action VARCHAR(50) NOT NULL, -- 'approved', 'rejected', 'reassigned', 'commented'
    comments TEXT,
    previous_status VARCHAR(50),
    new_status VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (task_id) REFERENCES approval_tasks(task_id) ON DELETE CASCADE,
    FOREIGN KEY (request_id) REFERENCES approval_requests(request_id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- Approval workflows table: defines approval workflow templates
CREATE TABLE approval_workflows (
    workflow_id INT PRIMARY KEY AUTO_INCREMENT,
    workflow_name VARCHAR(100) NOT NULL,
    description TEXT,
    request_type VARCHAR(50), -- links to approval_requests.request_type
    is_active BOOLEAN DEFAULT true,
    requires_sequential_approval BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Workflow steps table: defines the steps in each workflow
CREATE TABLE workflow_steps (
    step_id INT PRIMARY KEY AUTO_INCREMENT,
    workflow_id INT NOT NULL,
    step_order INT NOT NULL,
    approver_role VARCHAR(50), -- role required for this step
    approver_user_id INT, -- specific user (optional, if role is not used)
    is_required BOOLEAN DEFAULT true,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (workflow_id) REFERENCES approval_workflows(workflow_id) ON DELETE CASCADE,
    FOREIGN KEY (approver_user_id) REFERENCES users(user_id)
);

-- Attachments table: stores files attached to approval requests
CREATE TABLE attachments (
    attachment_id INT PRIMARY KEY AUTO_INCREMENT,
    request_id INT NOT NULL,
    file_name VARCHAR(255) NOT NULL,
    file_path VARCHAR(500) NOT NULL,
    file_size INT, -- in bytes
    mime_type VARCHAR(100),
    uploaded_by INT NOT NULL,
    uploaded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (request_id) REFERENCES approval_requests(request_id) ON DELETE CASCADE,
    FOREIGN KEY (uploaded_by) REFERENCES users(user_id)
);

-- Notifications table: tracks notifications sent to users
CREATE TABLE notifications (
    notification_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    request_id INT,
    task_id INT,
    notification_type VARCHAR(50), -- 'task_assigned', 'approval_complete', 'deadline_reminder'
    message TEXT,
    is_read BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (request_id) REFERENCES approval_requests(request_id) ON DELETE CASCADE,
    FOREIGN KEY (task_id) REFERENCES approval_tasks(task_id) ON DELETE CASCADE
);

-- Indexes for performance optimization
CREATE INDEX idx_approval_requests_requester ON approval_requests(requester_id);
CREATE INDEX idx_approval_requests_status ON approval_requests(status);
CREATE INDEX idx_approval_tasks_assigned ON approval_tasks(assigned_to);
CREATE INDEX idx_approval_tasks_request ON approval_tasks(request_id);
CREATE INDEX idx_approval_tasks_status ON approval_tasks(status);
CREATE INDEX idx_approval_history_task ON approval_history(task_id);
CREATE INDEX idx_approval_history_request ON approval_history(request_id);
CREATE INDEX idx_notifications_user ON notifications(user_id);
CREATE INDEX idx_notifications_unread ON notifications(user_id, is_read);
```
