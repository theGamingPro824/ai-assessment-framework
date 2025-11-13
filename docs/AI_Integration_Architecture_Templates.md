# AI Solution Architecture Templates

**Version:** 1.0
**Owner:** Cesar (Security Architect)
**Purpose:** Reference architectures and security patterns for common AI solution types
**Last Updated:** November 12, 2025

---

## Overview

This document provides pre-approved architecture templates for common AI solution patterns. Using these templates accelerates development, ensures security best practices, and provides consistent approaches across projects.

### How to Use These Templates

1. **Identify your pattern:** Match your use case to one of the templates
2. **Customize as needed:** Adapt the template to your specific requirements
3. **Security review:** Even with templates, Medium+ risk solutions need security review
4. **Document deviations:** Note any changes from the template and rationale

---

## Template Index

1. [Simple API-Only Chatbot](#template-1-simple-api-only-chatbot)
2. [RAG with Vector Database](#template-2-rag-with-vector-database)
3. [Database-Backed AI Application](#template-3-database-backed-ai-application)
4. [Document Processing Pipeline](#template-4-document-processing-pipeline)
5. [Real-Time AI Agent with Actions](#template-5-real-time-ai-agent-with-actions)
6. [Multi-Modal AI (Text + Images)](#template-6-multi-modal-ai-text--images)
7. [AI Analytics Dashboard](#template-7-ai-analytics-dashboard)
8. [Code Assistant with Repository Access](#template-8-code-assistant-with-repository-access)

---

## Template 1: Simple API-Only Chatbot

### Use Cases
- Internal team assistant
- Q&A chatbot (no knowledge base)
- Conversational interface for simple tasks
- Proof of concept / prototype

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│   Web UI    │ HTTPS   │   Backend    │  TLS    │  AI API         │
│  (React)    │────────▶│  (FastAPI)   │────────▶│  (Claude/GPT)   │
│             │         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │
                                │
                                ▼
                        ┌──────────────┐
                        │    Redis     │
                        │  (Sessions)  │
                        └──────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- TailwindCSS for styling
- WebSocket or polling for real-time updates

**Backend:**
- Python FastAPI or Node.js Express
- Redis for session management
- JWT tokens for authentication

**AI Provider:**
- Anthropic Claude API (recommended for enterprise)
- OpenAI API
- Or other model provider

**Infrastructure:**
- Docker containers
- Nginx reverse proxy
- Tailscale VPN for internal access

### Data Flow

1. User authenticates via SSO
2. User sends message to backend
3. Backend validates session (Redis)
4. Backend calls AI API with user message
5. AI response streamed back to user
6. Conversation stored in Redis (ephemeral, session-only)

### Security Controls

#### Authentication & Authorization
- [ ] SSO integration (Google Workspace, Azure AD)
- [ ] MFA enabled for all users
- [ ] JWT tokens with 1-hour expiration
- [ ] Session stored in Redis with 24-hour TTL

#### API Security
- [ ] Rate limiting: 10 requests/minute per user
- [ ] Input validation and sanitization
- [ ] Output encoding to prevent XSS
- [ ] CORS configured for known origins only

#### Data Protection
- [ ] TLS 1.3 for all communications
- [ ] No persistent storage of conversations (ephemeral)
- [ ] API keys stored in secrets manager (not code)
- [ ] Environment-based configuration

#### Monitoring & Logging
- [ ] Request/response logging (sanitized)
- [ ] Error logging with alerting
- [ ] API usage tracking and cost monitoring
- [ ] Uptime monitoring

### Configuration Example

```yaml
# docker-compose.yml
version: '3.8'
services:
  backend:
    image: your-backend:latest
    environment:
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=${JWT_SECRET}
      - SSO_CLIENT_ID=${SSO_CLIENT_ID}
      - SSO_CLIENT_SECRET=${SSO_CLIENT_SECRET}
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data
    command: redis-server --maxmemory 256mb --maxmemory-policy allkeys-lru

  nginx:
    image: nginx:alpine
    ports:
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
```

### Deployment Checklist

- [ ] Secrets configured in environment (no hardcoded keys)
- [ ] TLS certificates installed and valid
- [ ] Rate limiting configured at nginx + application level
- [ ] Session expiration tested
- [ ] SSO integration tested with real users
- [ ] API cost alerts configured
- [ ] Monitoring and alerting operational
- [ ] Backup/recovery procedures documented

### Cost Estimate

**Development:** $5,000 - $10,000 (2-3 weeks)
**Monthly Operational:**
- AI API: $50-200 (depends on usage)
- Infrastructure: $0-50 (if self-hosted)
- Total: $50-250/month

### Risk Assessment

**Typical Risk Score:** 15-25 (Low Risk)
**Required Approvals:** Team Lead
**Security Review:** Self-assessment checklist

---

## Template 2: RAG with Vector Database

### Use Cases
- Knowledge base chatbot
- Document Q&A system
- Technical documentation assistant
- Internal wiki/knowledge search

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│   Web UI    │ HTTPS   │   Backend    │  TLS    │  AI API         │
│  (React)    │────────▶│  (FastAPI)   │────────▶│  (Claude/GPT)   │
│             │         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │                          │
                                │                          │
                                ▼                          ▼
                        ┌──────────────┐         ┌─────────────────┐
                        │  PostgreSQL  │         │   Embedding     │
                        │  (metadata)  │         │   Model API     │
                        └──────────────┘         └─────────────────┘
                                │                          │
                                │                          │
                                ▼                          ▼
                        ┌─────────────────────────────────┐
                        │    Vector Database              │
                        │  (Pinecone/Qdrant/pgvector)     │
                        └─────────────────────────────────┘
                                ▲
                                │
                        ┌───────┴────────┐
                        │  Document      │
                        │  Ingestion     │
                        │  Pipeline      │
                        └────────────────┘
                                ▲
                                │
                        ┌───────┴────────┐
                        │  Document      │
                        │  Storage       │
                        │  (S3/Local)    │
                        └────────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- TailwindCSS
- Markdown rendering for responses
- File upload component

**Backend:**
- Python FastAPI
- PostgreSQL for metadata
- Redis for caching/sessions
- Celery for async document processing

**Vector Database Options:**
- **pgvector (PostgreSQL extension)** - Best for starting, low cost
- **Qdrant** - Self-hosted, good performance
- **Pinecone** - Managed service, easy to scale
- **Weaviate** - Self-hosted, advanced features

**Document Processing:**
- Python libraries: PyPDF2, unstructured, langchain
- Embedding model: OpenAI embeddings or open-source (all-MiniLM-L6-v2)
- Chunking: Recursive character splitter (500-1000 tokens)

### Data Flow

#### Document Ingestion Flow
1. Admin uploads documents (PDF, MD, TXT, DOCX)
2. Documents stored in S3/local storage
3. Async job triggered for processing
4. Document parsed and chunked
5. Chunks sent to embedding API
6. Embeddings + metadata stored in vector DB
7. Original document metadata stored in PostgreSQL

#### Query Flow
1. User submits question
2. Question converted to embedding
3. Vector DB similarity search (top-k results)
4. Retrieved chunks + question sent to AI API
5. AI generates response using context
6. Response streamed to user
7. Feedback captured for improvement

### Security Controls

#### Authentication & Authorization
- [ ] SSO integration with MFA
- [ ] Role-based access control (RBAC):
  - **User:** Query knowledge base
  - **Admin:** Upload/manage documents
  - **Super Admin:** All permissions
- [ ] Document-level access control (if needed)

#### Data Protection
- [ ] Encryption at rest (PostgreSQL TDE, vector DB encryption)
- [ ] Encryption in transit (TLS 1.3)
- [ ] Document access auditing
- [ ] PII scanning before ingestion (optional)
- [ ] Data retention policies (auto-delete old documents)

#### API Security
- [ ] Rate limiting: 20 queries/minute per user
- [ ] Input validation (file types, sizes)
- [ ] Malware scanning on uploaded files
- [ ] API authentication for embedding API

#### Infrastructure Security
- [ ] Network segmentation (VPN for internal users)
- [ ] Firewall rules (restrict vector DB access)
- [ ] Backup strategy (PostgreSQL + vector DB)
- [ ] Disaster recovery procedures

### Configuration Example

```python
# config.py
class RAGConfig:
    # AI Model
    LLM_PROVIDER = "anthropic"  # or "openai"
    LLM_MODEL = "claude-3-5-sonnet-20241022"

    # Embedding Model
    EMBEDDING_PROVIDER = "openai"
    EMBEDDING_MODEL = "text-embedding-3-small"
    EMBEDDING_DIMENSIONS = 1536

    # Vector Database
    VECTOR_DB = "qdrant"  # or "pinecone", "pgvector"
    VECTOR_DB_URL = os.getenv("VECTOR_DB_URL")
    COLLECTION_NAME = "knowledge_base"

    # RAG Parameters
    CHUNK_SIZE = 800
    CHUNK_OVERLAP = 200
    TOP_K_RESULTS = 5
    SIMILARITY_THRESHOLD = 0.7

    # Document Processing
    SUPPORTED_FILE_TYPES = [".pdf", ".md", ".txt", ".docx"]
    MAX_FILE_SIZE_MB = 50

    # Security
    ENABLE_PII_SCANNING = True
    ENABLE_MALWARE_SCANNING = True
```

### RAG Prompt Template

```python
SYSTEM_PROMPT = """You are a helpful AI assistant with access to a knowledge base.
Answer questions using ONLY the provided context. If the context doesn't contain
enough information, say so rather than making assumptions.

Context from knowledge base:
{context}

User question: {question}

Instructions:
- Answer based on the context above
- Cite sources when possible (e.g., "According to [document name]...")
- If unsure, admit uncertainty
- Be concise but thorough
"""
```

### Deployment Checklist

- [ ] Vector database initialized and indexed
- [ ] Embedding API tested and operational
- [ ] Document ingestion pipeline tested with sample docs
- [ ] Retrieval quality tested (precision/recall)
- [ ] Chunking strategy validated
- [ ] Similarity threshold tuned
- [ ] Access controls configured and tested
- [ ] Backup procedures tested
- [ ] Cost monitoring configured (API usage)

### Cost Estimate

**Development:** $15,000 - $30,000 (4-8 weeks)
**Monthly Operational:**
- AI API (LLM): $100-500
- Embedding API: $20-100
- Vector DB: $0-200 (depending on self-hosted vs managed)
- Storage: $10-50
- Infrastructure: $50-200
- Total: $180-1,050/month

### Risk Assessment

**Typical Risk Score:** 25-45 (Medium Risk)
**Required Approvals:** Security Architect
**Security Review:** Threat modeling + security assessment

### Performance Optimization

1. **Caching:** Cache frequently asked questions
2. **Indexing:** Optimize vector DB index configuration
3. **Chunking:** Experiment with chunk size for your content
4. **Hybrid search:** Combine vector + keyword search
5. **Reranking:** Add reranking step after retrieval

---

## Template 3: Database-Backed AI Application

### Use Cases
- AI-powered CRM insights
- Intelligent data analysis tools
- AI-assisted business applications
- Data visualization with AI explanations

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│   Web UI    │ HTTPS   │   Backend    │  TLS    │  AI API         │
│  (React)    │────────▶│  (FastAPI)   │────────▶│  (Claude/GPT)   │
│             │         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │
                                │
                                ▼
                        ┌──────────────┐
                        │  PostgreSQL  │
                        │  (App Data)  │
                        └──────────────┘
                                │
                                │
                                ▼
                        ┌──────────────┐
                        │    Redis     │
                        │  (Cache)     │
                        └──────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- Chart.js or D3.js for visualizations
- TailwindCSS

**Backend:**
- Python FastAPI
- SQLAlchemy ORM
- Alembic for migrations
- Celery for background tasks

**Database:**
- PostgreSQL 15+
- Read replicas for analytics (optional)
- Connection pooling (pgBouncer)

**Cache:**
- Redis for query caching and sessions

### Data Flow

1. User requests AI analysis of data
2. Backend queries database for relevant data
3. Data aggregated/transformed as needed
4. Data + prompt sent to AI API
5. AI generates insights/explanations
6. Results cached for similar queries
7. Response returned to user with visualizations

### Security Controls

#### Data Access
- [ ] Row-level security (RLS) in PostgreSQL
- [ ] Parameterized queries (prevent SQL injection)
- [ ] Principle of least privilege (database user permissions)
- [ ] Read-only database user for AI queries

#### AI Data Handling
- [ ] Data anonymization before sending to AI (if PII)
- [ ] Aggregate data only (no individual records to AI)
- [ ] Audit logging of all data accessed by AI
- [ ] User consent for AI analysis of their data

#### Application Security
- [ ] Input validation on all user inputs
- [ ] CSRF protection
- [ ] XSS prevention (output encoding)
- [ ] Rate limiting per user and globally

#### Database Security
- [ ] Encryption at rest (TDE)
- [ ] Encryption in transit (TLS)
- [ ] Database firewall rules
- [ ] Regular backups (automated, tested)
- [ ] Point-in-time recovery capability

### SQL Query Safety

```python
# Example: Secure database query with AI integration
from sqlalchemy import text
from typing import List, Dict

async def get_sales_insights(user_id: int, date_range: tuple) -> str:
    # Row-level security: only show user's data
    query = text("""
        SELECT
            date_trunc('day', created_at) as date,
            SUM(amount) as total_sales,
            COUNT(*) as num_orders
        FROM orders
        WHERE user_id = :user_id
        AND created_at BETWEEN :start_date AND :end_date
        GROUP BY date_trunc('day', created_at)
        ORDER BY date
    """)

    # Use parameterized query (prevent SQL injection)
    result = await db.execute(
        query,
        {"user_id": user_id, "start_date": date_range[0], "end_date": date_range[1]}
    )

    data = result.fetchall()

    # Anonymize/aggregate before sending to AI
    aggregated_data = {
        "total_revenue": sum(row.total_sales for row in data),
        "total_orders": sum(row.num_orders for row in data),
        "daily_average": sum(row.total_sales for row in data) / len(data) if data else 0
    }

    # Send only aggregated data to AI
    prompt = f"""Analyze this sales data and provide insights:
    {json.dumps(aggregated_data)}

    Provide 3 key insights and 2 recommendations."""

    ai_response = await call_ai_api(prompt)

    # Log access for audit
    await log_ai_data_access(user_id, "sales_insights", aggregated_data)

    return ai_response
```

### Deployment Checklist

- [ ] Database migrations tested
- [ ] Connection pooling configured
- [ ] Query performance optimized (indexes)
- [ ] Backup and restore tested
- [ ] Row-level security policies configured
- [ ] AI data anonymization verified
- [ ] Audit logging operational
- [ ] Read replicas configured (if needed)
- [ ] Cache invalidation strategy tested

### Cost Estimate

**Development:** $20,000 - $40,000 (6-10 weeks)
**Monthly Operational:**
- AI API: $100-500
- Database hosting: $50-300
- Infrastructure: $100-300
- Total: $250-1,100/month

### Risk Assessment

**Typical Risk Score:** 35-55 (Medium-High Risk)
**Required Approvals:** Security Architect + Executive (if customer data)
**Security Review:** Comprehensive security assessment + penetration testing

---

## Template 4: Document Processing Pipeline

### Use Cases
- Invoice/receipt processing
- Contract analysis
- Resume screening
- Compliance document review

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│  File Upload│         │   Ingestion  │         │  Processing     │
│    (Web)    │────────▶│    Queue     │────────▶│    Workers      │
│             │         │   (RabbitMQ) │         │   (Celery)      │
└─────────────┘         └──────────────┘         └─────────────────┘
                                                           │
                                                           │
                        ┌──────────────────────────────────┘
                        │
                        ▼
              ┌───────────────────┐
              │  OCR / Extraction │
              │   (Tesseract)     │
              └───────────────────┘
                        │
                        ▼
              ┌───────────────────┐
              │   AI Analysis     │
              │  (Claude/GPT)     │
              └───────────────────┘
                        │
                        ▼
              ┌───────────────────┐         ┌──────────────┐
              │   PostgreSQL      │◀────────│  Document    │
              │   (Results)       │         │  Storage     │
              └───────────────────┘         │  (S3/MinIO)  │
                                            └──────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- Drag-and-drop file upload
- Progress indicators
- Results dashboard

**Backend:**
- Python FastAPI (API layer)
- Celery (async task processing)
- RabbitMQ or Redis (message queue)

**Document Processing:**
- PyPDF2 / pdfplumber (PDF extraction)
- Tesseract OCR (image text extraction)
- python-docx (Word documents)
- Pillow (image processing)

**Storage:**
- PostgreSQL (metadata and results)
- S3 / MinIO (document storage)
- Redis (job status cache)

### Data Flow

1. User uploads document(s)
2. Files stored in S3/MinIO
3. Job created in queue with metadata
4. Worker picks up job from queue
5. Document extracted (OCR if needed)
6. Text sent to AI API for analysis
7. Results parsed and stored in database
8. User notified of completion
9. Results displayed in UI

### Security Controls

#### File Upload Security
- [ ] File type validation (whitelist only)
- [ ] File size limits enforced
- [ ] Malware scanning (ClamAV)
- [ ] Virus scan before processing
- [ ] Filename sanitization
- [ ] Quota limits per user

#### Document Storage
- [ ] Encryption at rest
- [ ] Access-controlled storage (signed URLs)
- [ ] Document retention policies
- [ ] Automatic deletion after processing (optional)
- [ ] Audit log of document access

#### Processing Security
- [ ] Sandboxed document processing
- [ ] Resource limits (CPU, memory, time)
- [ ] Input validation on extracted content
- [ ] PII detection and redaction (optional)
- [ ] Toxic content detection

#### Data Protection
- [ ] TLS for all communications
- [ ] Database encryption at rest
- [ ] Secure deletion of documents
- [ ] No persistent storage in workers

### Configuration Example

```python
# config.py
class DocumentProcessingConfig:
    # File Upload
    ALLOWED_FILE_TYPES = [".pdf", ".png", ".jpg", ".docx", ".xlsx"]
    MAX_FILE_SIZE_MB = 100
    MAX_FILES_PER_UPLOAD = 10

    # Storage
    STORAGE_BACKEND = "s3"  # or "minio", "local"
    S3_BUCKET = "document-processing"
    DOCUMENT_RETENTION_DAYS = 90

    # Processing
    ENABLE_OCR = True
    OCR_LANGUAGE = "eng"
    MAX_PROCESSING_TIME_SECONDS = 300

    # AI Analysis
    AI_PROVIDER = "anthropic"
    AI_MODEL = "claude-3-5-sonnet-20241022"

    # Security
    ENABLE_MALWARE_SCAN = True
    ENABLE_PII_DETECTION = True
    PII_REDACTION = "mask"  # or "remove", "none"

    # Queue
    CELERY_BROKER = "redis://redis:6379/0"
    CELERY_RESULT_BACKEND = "redis://redis:6379/1"
```

### Processing Worker Example

```python
# tasks.py
from celery import Celery
import pdfplumber
import boto3

app = Celery('document_processing')

@app.task(bind=True, max_retries=3)
def process_document(self, document_id: str):
    try:
        # 1. Retrieve document from storage
        doc = download_from_s3(document_id)

        # 2. Malware scan
        if not is_safe(doc):
            raise SecurityException("Malware detected")

        # 3. Extract text
        if doc.type == "pdf":
            text = extract_pdf_text(doc)
        elif doc.type in ["png", "jpg"]:
            text = ocr_extract(doc)

        # 4. PII detection and redaction
        if config.ENABLE_PII_DETECTION:
            text, pii_found = detect_and_redact_pii(text)
            if pii_found:
                log_pii_detection(document_id)

        # 5. Send to AI for analysis
        analysis = await analyze_with_ai(text, doc.analysis_type)

        # 6. Store results
        save_results(document_id, analysis)

        # 7. Notify user
        notify_user(doc.user_id, document_id, "completed")

        # 8. Clean up temp files
        cleanup(doc)

    except Exception as e:
        self.retry(exc=e, countdown=60)
```

### Deployment Checklist

- [ ] Message queue operational (RabbitMQ/Redis)
- [ ] Worker scaling configured (autoscaling)
- [ ] File upload limits tested
- [ ] Malware scanning operational
- [ ] OCR accuracy validated
- [ ] Storage quotas configured
- [ ] Document retention policies configured
- [ ] Dead letter queue configured (failed jobs)
- [ ] Monitoring and alerting for queue depth

### Cost Estimate

**Development:** $25,000 - $50,000 (8-12 weeks)
**Monthly Operational:**
- AI API: $200-1,000 (depends on volume)
- Storage: $20-200
- Infrastructure: $100-400
- OCR/processing: $50-200
- Total: $370-1,800/month

### Risk Assessment

**Typical Risk Score:** 40-60 (Medium-High Risk)
**Required Approvals:** Security Architect + Executive
**Security Review:** Comprehensive security assessment + file upload pentesting

---

## Template 5: Real-Time AI Agent with Actions

### Use Cases
- IT helpdesk automation
- Customer support agent
- DevOps assistant (deploy, rollback, check status)
- Intelligent monitoring and alerting

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│   Web UI    │ WSS     │   Backend    │  TLS    │  AI API         │
│  (React)    │◀───────▶│  (FastAPI)   │────────▶│  (Claude)       │
│             │         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │                          │
                                │                          │
                                │                          ▼
                                │                ┌─────────────────┐
                                │                │  Function       │
                                │                │  Calling /      │
                                │                │  Tool Use       │
                                │                └─────────────────┘
                                │                          │
                                ▼                          │
                        ┌──────────────┐                  │
                        │  PostgreSQL  │                  │
                        │  (Audit Log) │                  │
                        └──────────────┘                  │
                                                          │
                        ┌─────────────────────────────────┘
                        │
                        ▼
              ┌───────────────────┐
              │  Action Handlers  │
              │  (Tools/Plugins)  │
              └───────────────────┘
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│  Jira API    │ │  Slack API   │ │  AWS API     │
└──────────────┘ └──────────────┘ └──────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- WebSocket for real-time communication
- Action confirmation dialogs
- Audit trail display

**Backend:**
- Python FastAPI with WebSocket support
- Anthropic Python SDK (tool use / function calling)
- PostgreSQL for audit logging
- Redis for rate limiting

**Tool/Action Framework:**
- Plugin architecture for extensibility
- Each tool has: execute, validate, rollback functions
- Approval workflow for destructive actions

### Data Flow

1. User sends request via WebSocket
2. Backend routes to AI API with available tools
3. AI decides which tool(s) to use
4. Backend validates tool call and permissions
5. User approval requested (if needed for destructive action)
6. Tool executed with parameters
7. Result returned to AI for interpretation
8. AI responds to user with outcome
9. All actions logged to audit trail

### Security Controls

#### Action Authorization
- [ ] Role-based permissions for each tool
- [ ] Approval workflow for destructive actions
- [ ] Audit logging of all actions
- [ ] Action rate limiting (prevent abuse)
- [ ] Dry-run mode for testing

#### Tool Security
- [ ] Input validation on tool parameters
- [ ] Least-privilege credentials for tools
- [ ] Timeout limits on tool execution
- [ ] Rollback capability for reversible actions
- [ ] Sandbox execution for risky tools

#### AI Safety
- [ ] System prompt with safety guidelines
- [ ] Tool use validation (confirm intent)
- [ ] Human-in-the-loop for critical actions
- [ ] Action confirmation before execution
- [ ] Abort capability

#### Audit & Compliance
- [ ] Comprehensive audit logging
- [ ] Immutable audit trail
- [ ] Who/what/when/why tracking
- [ ] Compliance reporting
- [ ] Anomaly detection

### Tool/Function Definition Example

```python
# tools.py
from typing import Dict, Any, Callable
from enum import Enum

class ToolRiskLevel(Enum):
    SAFE = "safe"  # Read-only, no side effects
    LOW = "low"  # Minor changes, easily reversible
    MEDIUM = "medium"  # Significant changes, reversible
    HIGH = "high"  # Critical changes, approval required
    CRITICAL = "critical"  # Destructive, executive approval

class Tool:
    def __init__(
        self,
        name: str,
        description: str,
        risk_level: ToolRiskLevel,
        required_permissions: List[str],
        execute_fn: Callable,
        validate_fn: Callable = None,
        rollback_fn: Callable = None,
    ):
        self.name = name
        self.description = description
        self.risk_level = risk_level
        self.required_permissions = required_permissions
        self.execute_fn = execute_fn
        self.validate_fn = validate_fn
        self.rollback_fn = rollback_fn

# Example: Create Jira Ticket Tool
async def create_jira_ticket(summary: str, description: str, priority: str) -> Dict:
    # Input validation
    if not summary or len(summary) > 200:
        raise ValueError("Invalid summary")

    # Execute action
    ticket = jira_client.create_issue(
        project="SUPPORT",
        summary=summary,
        description=description,
        priority=priority,
    )

    return {
        "ticket_id": ticket.key,
        "url": ticket.permalink(),
        "status": "created"
    }

create_ticket_tool = Tool(
    name="create_jira_ticket",
    description="Create a Jira ticket for issue tracking",
    risk_level=ToolRiskLevel.LOW,
    required_permissions=["jira.create"],
    execute_fn=create_jira_ticket,
)

# Example: Restart Service Tool (Higher Risk)
async def restart_service(service_name: str, environment: str) -> Dict:
    # Validation
    if environment == "production":
        raise PermissionError("Production restarts require approval")

    allowed_services = ["web-app", "api-server", "worker"]
    if service_name not in allowed_services:
        raise ValueError(f"Service not allowed: {service_name}")

    # Execute
    result = kubernetes_client.restart_deployment(service_name, environment)

    return {
        "service": service_name,
        "environment": environment,
        "status": "restarted",
        "rollback_available": True
    }

restart_service_tool = Tool(
    name="restart_service",
    description="Restart a service in the specified environment",
    risk_level=ToolRiskLevel.MEDIUM,
    required_permissions=["kubernetes.write", "service.restart"],
    execute_fn=restart_service,
    rollback_fn=lambda params: kubernetes_client.rollback_deployment(params["service_name"]),
)
```

### AI Integration with Tools

```python
# agent.py
import anthropic

async def run_ai_agent(user_message: str, user_id: int, available_tools: List[Tool]):
    client = anthropic.Anthropic()

    # Convert tools to Anthropic format
    anthropic_tools = [
        {
            "name": tool.name,
            "description": tool.description,
            "input_schema": tool.get_input_schema(),
        }
        for tool in available_tools
        if user_has_permission(user_id, tool.required_permissions)
    ]

    # Initial AI call
    response = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=4096,
        tools=anthropic_tools,
        messages=[{"role": "user", "content": user_message}]
    )

    # Process tool calls
    while response.stop_reason == "tool_use":
        tool_use = response.content[-1]  # Get last tool use block
        tool_name = tool_use.name
        tool_input = tool_use.input

        # Find and validate tool
        tool = next((t for t in available_tools if t.name == tool_name), None)
        if not tool:
            raise ValueError(f"Tool not found: {tool_name}")

        # Check permissions
        if not user_has_permission(user_id, tool.required_permissions):
            result = {"error": "Permission denied"}
        else:
            # Request approval if high-risk
            if tool.risk_level in [ToolRiskLevel.HIGH, ToolRiskLevel.CRITICAL]:
                approved = await request_user_approval(user_id, tool_name, tool_input)
                if not approved:
                    result = {"error": "User denied approval"}
                else:
                    result = await execute_tool_safely(tool, tool_input)
            else:
                result = await execute_tool_safely(tool, tool_input)

        # Log action
        await log_tool_execution(user_id, tool_name, tool_input, result)

        # Continue conversation with tool result
        response = client.messages.create(
            model="claude-3-5-sonnet-20241022",
            max_tokens=4096,
            tools=anthropic_tools,
            messages=[
                {"role": "user", "content": user_message},
                {"role": "assistant", "content": response.content},
                {"role": "user", "content": [
                    {
                        "type": "tool_result",
                        "tool_use_id": tool_use.id,
                        "content": json.dumps(result),
                    }
                ]}
            ]
        )

    return response.content[0].text
```

### Deployment Checklist

- [ ] All tools tested in isolation
- [ ] Permission system configured
- [ ] Approval workflow tested
- [ ] Rollback mechanisms tested
- [ ] Audit logging verified
- [ ] Rate limiting per user and tool
- [ ] Tool timeout limits enforced
- [ ] WebSocket connection security validated
- [ ] Dry-run mode operational

### Cost Estimate

**Development:** $40,000 - $80,000 (12-16 weeks)
**Monthly Operational:**
- AI API: $300-1,500 (heavy usage with tool calls)
- Infrastructure: $100-400
- Third-party API costs: Variable
- Total: $400-2,000+/month

### Risk Assessment

**Typical Risk Score:** 55-75 (High Risk)
**Required Approvals:** Security Architect + Executive
**Security Review:** Comprehensive security assessment + red team testing

---

## Template 6: Multi-Modal AI (Text + Images)

### Use Cases
- Product catalog analysis
- Medical image interpretation (with human review)
- Document + diagram analysis
- Visual quality inspection

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│   Web UI    │ HTTPS   │   Backend    │  TLS    │  AI API         │
│  (React)    │────────▶│  (FastAPI)   │────────▶│  (Claude/GPT-4V)│
│ Image Upload│         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │
                                │
                                ▼
                        ┌──────────────┐         ┌──────────────┐
                        │  Image       │         │  Object      │
                        │  Processing  │────────▶│  Storage     │
                        │  (Pillow)    │         │  (S3/MinIO)  │
                        └──────────────┘         └──────────────┘
                                │
                                ▼
                        ┌──────────────┐
                        │  PostgreSQL  │
                        │  (Metadata)  │
                        └──────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- Image preview and cropping
- Drag-and-drop upload
- Multi-image support

**Backend:**
- Python FastAPI
- Pillow (image processing)
- PostgreSQL (metadata)
- S3/MinIO (image storage)

**AI Provider:**
- Anthropic Claude (with vision)
- OpenAI GPT-4V (with vision)
- Google Gemini Pro Vision

### Data Flow

1. User uploads image(s) with optional text prompt
2. Images validated and processed (resize, format conversion)
3. Images stored in S3/MinIO
4. Images + text encoded and sent to AI API
5. AI analyzes images and generates response
6. Results stored with metadata
7. Response returned to user

### Security Controls

#### Image Upload Security
- [ ] File type validation (JPEG, PNG, WebP only)
- [ ] File size limits (< 20MB per image)
- [ ] Image validation (ensure it's actually an image)
- [ ] Malware scanning
- [ ] Metadata stripping (EXIF removal for privacy)
- [ ] Image quota per user

#### Content Moderation
- [ ] NSFW image detection
- [ ] Violence/gore detection
- [ ] PII in images (faces, documents)
- [ ] Reject/flag inappropriate content

#### Data Protection
- [ ] Encryption at rest (S3)
- [ ] Encryption in transit (TLS)
- [ ] Time-limited access URLs (signed)
- [ ] Automatic deletion after retention period
- [ ] Watermarking (optional)

#### AI Safety
- [ ] Prompt injection detection
- [ ] Output validation
- [ ] Confidence scoring
- [ ] Human review for critical use cases

### Configuration Example

```python
# config.py
class MultiModalConfig:
    # Image Upload
    ALLOWED_IMAGE_TYPES = [".jpg", ".jpeg", ".png", ".webp"]
    MAX_IMAGE_SIZE_MB = 20
    MAX_IMAGES_PER_REQUEST = 5

    # Image Processing
    MAX_IMAGE_DIMENSION = 2048  # Resize larger images
    STRIP_EXIF_DATA = True
    CONVERT_TO_FORMAT = "jpeg"  # Standardize format
    JPEG_QUALITY = 85

    # AI Model
    AI_PROVIDER = "anthropic"
    AI_MODEL = "claude-3-5-sonnet-20241022"  # Supports vision

    # Content Moderation
    ENABLE_NSFW_DETECTION = True
    ENABLE_FACE_DETECTION = True  # Privacy concern
    MODERATION_SERVICE = "aws-rekognition"  # or "google-vision"

    # Storage
    STORAGE_BACKEND = "s3"
    IMAGE_RETENTION_DAYS = 30
    SIGNED_URL_EXPIRATION_HOURS = 24
```

### Image Processing Example

```python
# image_processing.py
from PIL import Image
import io
import boto3

async def process_and_upload_image(file: UploadFile, user_id: int) -> str:
    # 1. Validate file type
    if not file.content_type.startswith("image/"):
        raise ValueError("Invalid file type")

    # 2. Load image
    image_data = await file.read()
    image = Image.open(io.BytesIO(image_data))

    # 3. Strip EXIF data (privacy)
    if config.STRIP_EXIF_DATA:
        image = strip_exif(image)

    # 4. Resize if too large
    if max(image.size) > config.MAX_IMAGE_DIMENSION:
        image = resize_image(image, config.MAX_IMAGE_DIMENSION)

    # 5. Content moderation
    if config.ENABLE_NSFW_DETECTION:
        moderation_result = await moderate_image(image_data)
        if moderation_result["nsfw"]:
            raise ValueError("Inappropriate content detected")

    # 6. Convert format and compress
    output = io.BytesIO()
    image.save(output, format="JPEG", quality=config.JPEG_QUALITY)
    output.seek(0)

    # 7. Upload to S3
    image_id = generate_unique_id()
    s3_key = f"images/{user_id}/{image_id}.jpg"

    s3_client.upload_fileobj(
        output,
        config.S3_BUCKET,
        s3_key,
        ExtraArgs={
            "ContentType": "image/jpeg",
            "ServerSideEncryption": "AES256",
            "Metadata": {
                "user_id": str(user_id),
                "uploaded_at": datetime.utcnow().isoformat(),
            }
        }
    )

    # 8. Store metadata
    await store_image_metadata(image_id, user_id, s3_key, image.size)

    return image_id
```

### AI Vision Analysis

```python
# vision_analysis.py
import anthropic
import base64

async def analyze_images(image_ids: List[str], prompt: str) -> str:
    client = anthropic.Anthropic()

    # Retrieve images from S3
    image_contents = []
    for image_id in image_ids:
        image_data = await get_image_from_s3(image_id)
        encoded = base64.standard_b64encode(image_data).decode("utf-8")
        image_contents.append({
            "type": "image",
            "source": {
                "type": "base64",
                "media_type": "image/jpeg",
                "data": encoded,
            }
        })

    # Construct message with images
    message_content = image_contents + [
        {"type": "text", "text": prompt}
    ]

    # Call Claude with vision
    response = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=4096,
        messages=[
            {"role": "user", "content": message_content}
        ]
    )

    return response.content[0].text
```

### Deployment Checklist

- [ ] Image processing pipeline tested
- [ ] Content moderation operational
- [ ] EXIF stripping verified
- [ ] Storage quotas configured
- [ ] Signed URL generation tested
- [ ] Retention/deletion policies configured
- [ ] AI vision API tested with sample images
- [ ] Cost monitoring for image analysis

### Cost Estimate

**Development:** $20,000 - $40,000 (6-10 weeks)
**Monthly Operational:**
- AI API (vision): $300-2,000 (vision more expensive than text)
- Image storage: $20-200
- Content moderation API: $50-300
- Infrastructure: $100-300
- Total: $470-2,800/month

### Risk Assessment

**Typical Risk Score:** 40-65 (Medium-High Risk)
**Required Approvals:** Security Architect + (Privacy Officer if faces/PII)
**Security Review:** Security assessment + privacy review

---

## Template 7: AI Analytics Dashboard

### Use Cases
- Business intelligence with AI insights
- Anomaly detection and alerting
- Predictive analytics dashboard
- Data exploration assistant

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│   Web UI    │ HTTPS   │   Backend    │  TLS    │  AI API         │
│ (Dashboard) │────────▶│  (FastAPI)   │────────▶│  (Claude)       │
│             │         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │
                                │
                                ▼
                        ┌──────────────┐
                        │  PostgreSQL  │
                        │  (Analytics) │
                        └──────────────┘
                                │
                                ▼
                        ┌──────────────┐
                        │    Redis     │
                        │  (Cache)     │
                        └──────────────┘
```

### Technology Stack

**Frontend:**
- React + TypeScript
- Chart.js / Recharts for visualizations
- TailwindCSS
- Real-time data updates (WebSocket or polling)

**Backend:**
- Python FastAPI
- Pandas for data manipulation
- PostgreSQL for data storage
- Redis for caching expensive queries

**Analytics:**
- SQL for aggregations
- Python for data transformations
- AI for natural language insights

### Data Flow

1. User views dashboard or asks question
2. Backend queries database (cached if possible)
3. Data aggregated/transformed
4. Visualizations generated
5. AI analyzes data for insights
6. Results displayed with charts + AI commentary
7. User can ask follow-up questions

### Security Controls

#### Data Access
- [ ] Row-level security in database
- [ ] Data aggregation (no individual records shown)
- [ ] Role-based dashboard views
- [ ] Audit logging of data access

#### AI Integration
- [ ] Only aggregated data sent to AI
- [ ] No PII sent to AI
- [ ] Data anonymization
- [ ] Query result limits

#### Application Security
- [ ] Input validation on queries
- [ ] SQL injection prevention
- [ ] Rate limiting on expensive queries
- [ ] Query timeout limits

### Example: Natural Language to SQL

```python
# nl_to_sql.py
import anthropic

async def natural_language_query(question: str, user_id: int) -> Dict:
    # Get user's accessible tables and columns (based on permissions)
    schema = await get_user_accessible_schema(user_id)

    # Use AI to generate SQL
    client = anthropic.Anthropic()
    response = client.messages.create(
        model="claude-3-5-sonnet-20241022",
        max_tokens=1024,
        system=f"""You are a SQL expert. Generate safe, read-only SQL queries
        based on user questions. Only use the following schema:

        {schema}

        Important:
        - Only SELECT queries (no INSERT, UPDATE, DELETE)
        - Include LIMIT clause (max 1000 rows)
        - Use aggregations when possible
        - Parameterize any user inputs""",
        messages=[
            {"role": "user", "content": question}
        ]
    )

    sql_query = extract_sql_from_response(response.content[0].text)

    # Validate query (ensure it's safe)
    if not is_safe_query(sql_query):
        raise ValueError("Unsafe SQL query generated")

    # Execute query with timeout
    results = await execute_query_with_timeout(sql_query, timeout=30)

    # Generate AI insights on results
    insights = await generate_insights(results, question)

    return {
        "query": sql_query,
        "results": results,
        "insights": insights,
        "chart_suggestion": suggest_chart_type(results)
    }
```

### Deployment Checklist

- [ ] Database query performance optimized
- [ ] Query result caching configured
- [ ] Row-level security tested
- [ ] Query timeout limits enforced
- [ ] AI-generated SQL validation tested
- [ ] Data anonymization verified
- [ ] Dashboard permissions configured
- [ ] Real-time updates tested (if applicable)

### Cost Estimate

**Development:** $25,000 - $50,000 (8-12 weeks)
**Monthly Operational:**
- AI API: $100-600
- Database: $100-500
- Infrastructure: $100-300
- Total: $300-1,400/month

### Risk Assessment

**Typical Risk Score:** 35-55 (Medium-High Risk)
**Required Approvals:** Security Architect + Data Owner
**Security Review:** Security assessment + SQL injection testing

---

## Template 8: Code Assistant with Repository Access

### Use Cases
- Code review assistant
- Documentation generator
- Bug finder and fixer
- Refactoring suggestions

### Architecture Diagram

```
┌─────────────┐         ┌──────────────┐         ┌─────────────────┐
│    IDE      │ HTTPS   │   Backend    │  TLS    │  AI API         │
│  Extension  │────────▶│  (FastAPI)   │────────▶│  (Claude)       │
│             │         │              │         │                 │
└─────────────┘         └──────────────┘         └─────────────────┘
                                │
                                │
                                ▼
                        ┌──────────────┐         ┌──────────────┐
                        │  Code        │         │  Git Repo    │
                        │  Indexing    │◀────────│  (GitHub)    │
                        │  (Vector DB) │         │              │
                        └──────────────┘         └──────────────┘
                                │
                                ▼
                        ┌──────────────┐
                        │  PostgreSQL  │
                        │  (Metadata)  │
                        └──────────────┘
```

### Technology Stack

**Frontend/IDE:**
- VS Code extension
- JetBrains plugin
- Or web-based code editor

**Backend:**
- Python FastAPI
- GitPython for repository operations
- Tree-sitter for code parsing
- Vector database for code search

**Code Analysis:**
- AST parsing
- Static analysis tools (pylint, eslint, etc.)
- AI for semantic understanding

### Security Controls

#### Repository Access
- [ ] OAuth with GitHub/GitLab
- [ ] Read-only access by default
- [ ] Repository permissions respected
- [ ] No access to private keys or secrets

#### Code Handling
- [ ] Secrets scanning before sending to AI
- [ ] Proprietary code protection
- [ ] Code anonymization (optional)
- [ ] Audit logging of code accessed

#### AI Integration
- [ ] Only relevant code snippets sent (not full repo)
- [ ] Context window management
- [ ] Code watermarking (for tracking)
- [ ] Opt-out for sensitive files

### Configuration Example

```python
# config.py
class CodeAssistantConfig:
    # Repository Access
    GITHUB_APP_ID = os.getenv("GITHUB_APP_ID")
    GITHUB_PRIVATE_KEY = os.getenv("GITHUB_PRIVATE_KEY")
    MAX_FILE_SIZE_KB = 500  # Skip large files

    # Code Analysis
    SUPPORTED_LANGUAGES = ["python", "javascript", "typescript", "java", "go"]
    ENABLE_SECRET_SCANNING = True
    SECRET_PATTERNS = ["api_key", "password", "token", "secret"]

    # AI Model
    AI_PROVIDER = "anthropic"
    AI_MODEL = "claude-3-5-sonnet-20241022"
    MAX_CONTEXT_LINES = 500  # Limit code sent to AI

    # Vector Database (for code search)
    VECTOR_DB = "qdrant"
    EMBEDDING_MODEL = "openai/text-embedding-3-small"

    # Security
    EXCLUDED_PATHS = [".env", "*.pem", "*.key", "secrets/"]
    REQUIRE_APPROVAL_FOR_WRITE = True
```

### Deployment Checklist

- [ ] GitHub/GitLab OAuth configured
- [ ] Secrets scanning operational
- [ ] Repository permissions tested
- [ ] Code indexing pipeline tested
- [ ] IDE extension/plugin tested
- [ ] Write operations require approval
- [ ] Audit logging operational

### Cost Estimate

**Development:** $30,000 - $60,000 (10-16 weeks)
**Monthly Operational:**
- AI API: $200-1,000
- Vector DB: $50-200
- Infrastructure: $100-300
- Total: $350-1,500/month

### Risk Assessment

**Typical Risk Score:** 45-65 (Medium-High Risk)
**Required Approvals:** Security Architect + Engineering Lead
**Security Review:** Security assessment + code access controls review

---

## Common Security Patterns Across All Templates

### 1. Authentication & Authorization Pattern

```python
# auth.py - Reusable across all templates

from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
import jwt

security = HTTPBearer()

async def get_current_user(
    credentials: HTTPAuthorizationCredentials = Depends(security)
) -> User:
    try:
        token = credentials.credentials
        payload = jwt.decode(token, settings.JWT_SECRET, algorithms=["HS256"])
        user_id = payload.get("sub")
        if user_id is None:
            raise HTTPException(status_code=401, detail="Invalid token")

        user = await get_user_by_id(user_id)
        if not user:
            raise HTTPException(status_code=401, detail="User not found")

        return user
    except jwt.PyJWTError:
        raise HTTPException(status_code=401, detail="Invalid token")

def require_permission(permission: str):
    async def permission_checker(user: User = Depends(get_current_user)):
        if not user.has_permission(permission):
            raise HTTPException(status_code=403, detail="Insufficient permissions")
        return user
    return permission_checker

# Usage in route:
@app.post("/api/sensitive-action")
async def sensitive_action(
    user: User = Depends(require_permission("admin.write"))
):
    # Only users with "admin.write" permission can access
    pass
```

### 2. Rate Limiting Pattern

```python
# rate_limit.py - Reusable rate limiting

from fastapi import Request, HTTPException
import redis
from datetime import timedelta

redis_client = redis.Redis(host='redis', port=6379)

async def rate_limit(
    request: Request,
    max_requests: int = 10,
    window: int = 60,  # seconds
    user: User = Depends(get_current_user)
):
    key = f"rate_limit:{user.id}:{request.url.path}"

    current = redis_client.get(key)
    if current is None:
        redis_client.setex(key, window, 1)
    else:
        current = int(current)
        if current >= max_requests:
            raise HTTPException(
                status_code=429,
                detail=f"Rate limit exceeded. Try again in {window} seconds."
            )
        redis_client.incr(key)

# Usage:
@app.post("/api/ai-query", dependencies=[Depends(rate_limit)])
async def ai_query(...):
    pass
```

### 3. Audit Logging Pattern

```python
# audit.py - Comprehensive audit logging

from datetime import datetime
from typing import Dict, Any
import json

async def log_audit_event(
    user_id: int,
    action: str,
    resource_type: str,
    resource_id: str,
    details: Dict[Any, Any] = None,
    ip_address: str = None,
    result: str = "success",  # or "failure"
):
    audit_entry = {
        "timestamp": datetime.utcnow().isoformat(),
        "user_id": user_id,
        "action": action,  # e.g., "ai_query", "document_upload", "tool_execution"
        "resource_type": resource_type,  # e.g., "chatbot", "document", "api"
        "resource_id": resource_id,
        "details": json.dumps(details) if details else None,
        "ip_address": ip_address,
        "result": result,
    }

    # Store in database (immutable audit log)
    await db.audit_log.insert_one(audit_entry)

    # Also log to file for backup
    logger.info(f"AUDIT: {json.dumps(audit_entry)}")

# Usage:
@app.post("/api/sensitive-action")
async def sensitive_action(request: Request, user: User = Depends(get_current_user)):
    result = await perform_action()

    await log_audit_event(
        user_id=user.id,
        action="sensitive_action",
        resource_type="api",
        resource_id="action_123",
        details={"parameters": request.json()},
        ip_address=request.client.host,
        result="success" if result else "failure"
    )
```

### 4. Secrets Detection Pattern

```python
# secrets_detection.py - Detect secrets before sending to AI

import re
from typing import List, Tuple

# Common secret patterns
SECRET_PATTERNS = {
    "api_key": r"(?i)(api[_-]?key|apikey)[\s:=]+['\"]?([a-zA-Z0-9_\-]{20,})['\"]?",
    "password": r"(?i)(password|passwd|pwd)[\s:=]+['\"]?([^\s'\"]{8,})['\"]?",
    "token": r"(?i)(token|auth)[\s:=]+['\"]?([a-zA-Z0-9_\-\.]{20,})['\"]?",
    "aws_key": r"(AKIA[0-9A-Z]{16})",
    "private_key": r"-----BEGIN (?:RSA |EC |OPENSSH )?PRIVATE KEY-----",
    "credit_card": r"\b\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}\b",
    "ssn": r"\b\d{3}-\d{2}-\d{4}\b",
}

def detect_secrets(text: str) -> List[Tuple[str, str]]:
    """
    Scan text for potential secrets.
    Returns list of (secret_type, matched_value) tuples.
    """
    found_secrets = []

    for secret_type, pattern in SECRET_PATTERNS.items():
        matches = re.findall(pattern, text)
        if matches:
            for match in matches:
                value = match if isinstance(match, str) else match[1]
                found_secrets.append((secret_type, value))

    return found_secrets

def redact_secrets(text: str) -> str:
    """
    Replace detected secrets with [REDACTED].
    """
    for secret_type, pattern in SECRET_PATTERNS.items():
        text = re.sub(pattern, f"[REDACTED_{secret_type.upper()}]", text)

    return text

# Usage before AI call:
async def send_to_ai(user_content: str):
    # Check for secrets
    secrets = detect_secrets(user_content)
    if secrets:
        logger.warning(f"Secrets detected in user input: {[s[0] for s in secrets]}")
        # Either reject or redact
        user_content = redact_secrets(user_content)

    # Now safe to send to AI
    response = await ai_api.call(user_content)
    return response
```

### 5. Cost Tracking Pattern

```python
# cost_tracking.py - Track AI API costs

from dataclasses import dataclass
from datetime import datetime

@dataclass
class AIUsage:
    user_id: int
    model: str
    input_tokens: int
    output_tokens: int
    cost_usd: float
    timestamp: datetime

async def track_ai_usage(
    user_id: int,
    model: str,
    input_tokens: int,
    output_tokens: int
):
    # Calculate cost based on model pricing
    cost = calculate_cost(model, input_tokens, output_tokens)

    # Store usage
    usage = AIUsage(
        user_id=user_id,
        model=model,
        input_tokens=input_tokens,
        output_tokens=output_tokens,
        cost_usd=cost,
        timestamp=datetime.utcnow()
    )

    await db.ai_usage.insert_one(usage.__dict__)

    # Check budget alerts
    monthly_usage = await get_monthly_usage(user_id)
    if monthly_usage > settings.USER_MONTHLY_BUDGET:
        await send_alert(f"User {user_id} exceeded monthly budget")

def calculate_cost(model: str, input_tokens: int, output_tokens: int) -> float:
    # Anthropic Claude pricing (as of 2024)
    pricing = {
        "claude-3-5-sonnet-20241022": {
            "input": 0.003,  # per 1K tokens
            "output": 0.015,
        },
        "claude-3-opus": {
            "input": 0.015,
            "output": 0.075,
        },
        "claude-3-haiku": {
            "input": 0.00025,
            "output": 0.00125,
        }
    }

    model_pricing = pricing.get(model, pricing["claude-3-5-sonnet-20241022"])
    cost = (input_tokens / 1000 * model_pricing["input"]) + \
           (output_tokens / 1000 * model_pricing["output"])

    return round(cost, 6)
```

---

## Template Selection Guide

| Use Case | Recommended Template | Complexity | Risk Level |
|----------|---------------------|------------|------------|
| Simple Q&A chatbot | Template 1: API-Only | Low | Low |
| Knowledge base assistant | Template 2: RAG | Medium | Medium |
| Business app with AI insights | Template 3: Database-Backed | Medium | Medium-High |
| Document extraction/analysis | Template 4: Document Pipeline | High | Medium-High |
| IT automation agent | Template 5: AI Agent with Actions | Very High | High |
| Image analysis | Template 6: Multi-Modal | Medium | Medium-High |
| BI dashboard with AI | Template 7: Analytics Dashboard | High | Medium-High |
| Code review/assistance | Template 8: Code Assistant | High | High |

---

## Next Steps After Selecting a Template

1. **Customize architecture** to your specific needs
2. **Complete AI Solution Questionnaire** to capture requirements
3. **Calculate risk score** using Risk Scoring Matrix
4. **Get required approvals** based on risk level
5. **Set up development environment** using template as baseline
6. **Implement security controls** from template checklist
7. **Test thoroughly** before production deployment
8. **Monitor and iterate** based on real-world usage

---

**Document Version:** 1.0
**Last Updated:** November 12, 2025
**Next Review:** February 12, 2026
**Owner:** Cesar (Security Architect)

**For questions or architecture consultation, contact:**
- **Cesar (Security Architect)** - cesar@alsolutions.com
- **Chris (AI Specialist)** - For AI model selection and implementation
