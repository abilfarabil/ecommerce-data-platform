# E-commerce Data Platform - Implementation PRP

## Goal

**Feature Goal**: Build a complete e-commerce data engineering platform with data ingestion, ETL processing, analytics APIs, and business intelligence capabilities that demonstrates modern Python data engineering practices.

**Deliverable**: Production-ready data platform with FastAPI backend, Airflow ETL pipelines, PostgreSQL+ClickHouse storage, and comprehensive testing suite.

**Success Definition**: Platform successfully ingests sample e-commerce data, processes it through ETL pipelines, exposes analytics APIs, and provides dashboard-ready data endpoints with 99.5% data accuracy and <3 second API response times.

## User Persona (if applicable)

**Target User**: Data engineers, business analysts, and dashboard consumers seeking comprehensive e-commerce insights.

**Use Case**: Process e-commerce transaction data, calculate business KPIs, and provide real-time analytics for business decision making.

**User Journey**:
1. Data engineer configures data sources and ingestion pipelines
2. ETL processes transform raw data into business metrics
3. Business analyst queries analytics APIs for dashboard data
4. Dashboard consumers view real-time business intelligence

**Pain Points Addressed**:
- Manual data processing and reporting
- Data quality inconsistencies
- Lack of real-time business visibility
- Scalability limitations of existing solutions

## Why

- **Business Value**: Enable data-driven decision making with automated insights generation
- **Technical Excellence**: Demonstrate proficiency in modern Python data engineering stack
- **Portfolio Impact**: Showcase enterprise-level data platform development capabilities
- **Scalability**: Modular architecture supporting growth from startup to enterprise scale
- **Industry Relevance**: E-commerce data patterns applicable across multiple industries

## What

A comprehensive data engineering platform with:

### Core Components
- **Data Ingestion Layer**: REST APIs for batch and streaming data ingestion
- **ETL Processing**: Airflow-orchestrated data transformation pipelines
- **Storage Layer**: PostgreSQL for transactional data, ClickHouse for analytics
- **Analytics APIs**: FastAPI endpoints for business metrics and reporting
- **Data Quality**: Automated validation, cleansing, and monitoring
- **Documentation**: CLAUDE.md and README.md with comprehensive setup guides

### Success Criteria

- [ ] Data ingestion APIs handle multiple source formats (JSON, CSV) with validation
- [ ] ETL pipelines process transaction data into business KPIs (revenue, conversion rates)
- [ ] Analytics APIs provide <3 second response times for dashboard queries
- [ ] Data quality validation achieves 99.5% accuracy rate
- [ ] Project includes 80%+ test coverage with comprehensive test suite
- [ ] Complete documentation enables new developer onboarding in <30 minutes
- [ ] Platform handles 10x data volume increase without architectural changes

## All Needed Context

### Context Completeness Check

_This PRP provides everything needed for someone unfamiliar with the codebase to implement a complete e-commerce data platform successfully._

### Documentation & References

```yaml
# MUST READ - Include these in your context window
- url: https://fastapi.tiangolo.com/tutorial/sql-databases/
  why: Database integration patterns for FastAPI with SQLAlchemy
  critical: Async database operations and dependency injection patterns

- url: https://docs.pydantic.dev/latest/concepts/validators/
  why: Custom validation for e-commerce data models (prices, emails, dates)
  critical: Field validation and error handling for business rules

- url: https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html
  why: DAG development patterns and project structure
  critical: Task dependencies and error handling strategies

- url: https://github.com/astral-sh/uv#getting-started
  why: UV package management and virtual environment setup
  critical: Dependency management and script execution patterns

- file: claude_md_files/CLAUDE-PYTHON-BASIC.md
  why: Project coding conventions and development patterns
  pattern: Python style guide, testing approaches, file organization
  gotcha: 100 character line limit, entity-specific database naming

- file: PRPs/contracts/ecommerce-data-platform-api-contract.md
  why: Complete API specifications with TypeScript interfaces
  pattern: Request/response models, error handling, validation rules
  gotcha: Exact field naming conventions and validation constraints

- file: PRPs/ecommerce-data-platform-prd.md
  why: Business requirements and technical architecture
  pattern: Data models, technology stack, implementation phases
  gotcha: Specific KPI calculations and data transformation requirements

- docfile: PRPs/ai_docs/getting_started.md
  why: Claude Code setup and development workflow patterns
  section: Development environment and tool usage
```

### Current Codebase tree (run `tree` in the root of the project)

```bash
ecommerce-data-platform/
├── .claude/                    # Claude Code configuration
├── .serena/                    # MCP server configuration and memories
├── PRPs/                       # Product Requirement Prompts framework
│   ├── contracts/              # API contracts
│   ├── scripts/prp_runner.py   # PRP execution script
│   ├── templates/              # PRP templates
│   └── ai_docs/                # Claude Code documentation
├── claude_md_files/            # Template files for project configuration
├── pyproject.toml              # Python project configuration (UV-managed)
├── README.md                   # Empty - to be created after implementation
├── CLAUDE.md                   # Empty - to be created before implementation
└── tujuan.md                   # Project goals and requirements (Indonesian)
```

### Desired Codebase tree with files to be added and responsibility of file

```bash
ecommerce-data-platform/
├── src/
│   ├── __init__.py
│   ├── main.py                 # FastAPI application entry point
│   ├── config/
│   │   ├── __init__.py
│   │   ├── settings.py         # Pydantic Settings for configuration
│   │   └── database.py         # Database connection and session management
│   ├── models/
│   │   ├── __init__.py
│   │   ├── base.py             # Base model classes
│   │   ├── transaction.py      # Transaction data models
│   │   ├── customer.py         # Customer data models
│   │   ├── product.py          # Product data models
│   │   └── analytics.py        # Analytics response models
│   ├── api/
│   │   ├── __init__.py
│   │   ├── v1/
│   │   │   ├── __init__.py
│   │   │   ├── ingestion.py    # Data ingestion endpoints
│   │   │   ├── analytics.py    # Analytics query endpoints
│   │   │   └── health.py       # Health check endpoints
│   ├── services/
│   │   ├── __init__.py
│   │   ├── ingestion_service.py    # Data ingestion business logic
│   │   ├── analytics_service.py    # Analytics computation logic
│   │   ├── validation_service.py   # Data quality validation
│   │   └── tests/
│   │       ├── test_ingestion_service.py
│   │       ├── test_analytics_service.py
│   │       └── test_validation_service.py
│   ├── etl/
│   │   ├── __init__.py
│   │   ├── dags/
│   │   │   ├── transaction_processing.py   # Transaction ETL DAG
│   │   │   ├── customer_analytics.py       # Customer metrics DAG
│   │   │   └── product_analytics.py        # Product metrics DAG
│   │   ├── operators/
│   │   │   ├── __init__.py
│   │   │   ├── data_quality_operator.py    # Custom data quality operator
│   │   │   └── transformation_operator.py  # Custom transformation operator
│   │   └── utils/
│   │       ├── __init__.py
│   │       ├── data_transformers.py        # Data transformation utilities
│   │       └── quality_checks.py           # Data quality validation
│   ├── database/
│   │   ├── __init__.py
│   │   ├── postgres/
│   │   │   ├── __init__.py
│   │   │   ├── models.py       # SQLAlchemy ORM models
│   │   │   └── repository.py   # Database operations
│   │   ├── clickhouse/
│   │   │   ├── __init__.py
│   │   │   ├── models.py       # ClickHouse table schemas
│   │   │   └── repository.py   # Analytics queries
│   │   └── migrations/
│   │       ├── alembic.ini
│   │       ├── env.py
│   │       └── versions/
│   └── utils/
│       ├── __init__.py
│       ├── logging.py          # Structured logging configuration
│       ├── exceptions.py       # Custom exception classes
│       └── validators.py       # Custom Pydantic validators
├── tests/
│   ├── __init__.py
│   ├── conftest.py             # Pytest configuration and fixtures
│   ├── integration/            # Integration tests
│   │   ├── test_api_endpoints.py
│   │   ├── test_etl_pipelines.py
│   │   └── test_database_integration.py
│   └── e2e/                    # End-to-end tests
│       └── test_complete_workflow.py
├── docker/
│   ├── Dockerfile              # Multi-stage build for production
│   ├── docker-compose.yml      # Development environment
│   └── docker-compose.prod.yml # Production environment
├── scripts/
│   ├── setup_dev.py            # Development environment setup
│   ├── run_migrations.py       # Database migration runner
│   └── sample_data_generator.py # Generate test data
├── docs/
│   ├── api/                    # API documentation
│   └── architecture/           # Architecture diagrams
├── CLAUDE.md                   # Development guidelines (created first)
└── README.md                   # Project documentation (created last)
```

### Known Gotchas of our codebase & Library Quirks

```python
# CRITICAL: UV package management patterns
# Use 'uv add' for dependencies, never edit pyproject.toml directly
# Use 'uv run' prefix for all command execution

# CRITICAL: FastAPI async patterns
# All endpoints must be async for proper database connection handling
async def endpoint_function():
    async with get_db_session() as session:
        # Database operations

# CRITICAL: Pydantic v2 validation patterns
# Use Field() for validation, not field validators in most cases
class Model(BaseModel):
    price: Decimal = Field(..., gt=0, decimal_places=2)

# CRITICAL: Database naming conventions from CLAUDE.md
# Entity-specific primary keys: transaction_id, customer_id, product_id
# Foreign keys: {referenced_entity}_id
# Timestamps: created_at, updated_at, processed_at

# CRITICAL: Testing patterns
# Tests co-located with code in tests/ subdirectories
# Use pytest fixtures for database and API testing
# AsyncClient for FastAPI endpoint testing

# CRITICAL: Airflow DAG patterns
# DAGs in src/etl/dags/ with proper task dependencies
# Use custom operators for reusable functionality
# Proper error handling and retry strategies
```

## Implementation Blueprint

### Data models and structure

Create comprehensive Pydantic models ensuring type safety and business rule validation.

```python
# Core business entities with validation
class Transaction(BaseModel):
    transaction_id: str = Field(..., min_length=10, max_length=50)
    customer_id: str = Field(..., min_length=5, max_length=50)
    amount: Decimal = Field(..., gt=0, decimal_places=2)
    currency: str = Field(..., regex=r"^[A-Z]{3}$")
    status: TransactionStatus
    timestamp: datetime
    products: List[ProductLineItem]

# Analytics response models
class RevenueMetrics(BaseModel):
    date: datetime
    total_revenue: Decimal
    transaction_count: int
    avg_order_value: Decimal
    unique_customers: int

# Request/response models for APIs
class BatchIngestionRequest(BaseModel):
    source_id: str = Field(..., min_length=3, max_length=50)
    data_type: DataType
    format: DataFormat
    data: List[Dict[str, Any]] = Field(..., min_items=1, max_items=10000)
```

### Implementation Tasks (ordered by dependencies)

```yaml
Task 1: CREATE CLAUDE.md (FIRST - before any implementation)
  - COPY template: claude_md_files/CLAUDE-PYTHON-BASIC.md to CLAUDE.md
  - CUSTOMIZE: Add data engineering specific patterns and dependencies
  - ADD: UV commands for data engineering workflow
  - VALIDATE: Ensure all project-specific conventions are documented

Task 2: CREATE src/config/settings.py
  - IMPLEMENT: Pydantic BaseSettings with environment variables
  - INCLUDE: Database URLs, API keys, logging levels, feature flags
  - PATTERN: Use @lru_cache for singleton settings instance
  - VALIDATION: Environment-specific validation rules

Task 3: CREATE src/models/base.py and domain models
  - IMPLEMENT: Base Pydantic models with common fields (created_at, updated_at)
  - CREATE: Transaction, Customer, Product, Analytics models
  - FOLLOW: Exact field naming from API contracts
  - VALIDATION: Business rules (price > 0, valid email formats)

Task 4: CREATE src/database/postgres/models.py
  - IMPLEMENT: SQLAlchemy ORM models matching Pydantic schemas
  - NAMING: Entity-specific primary keys (transaction_id, customer_id)
  - RELATIONSHIPS: Proper foreign key relationships
  - INDEXES: Performance indexes for common queries

Task 5: CREATE src/config/database.py
  - IMPLEMENT: Async database session management
  - PATTERN: Dependency injection for FastAPI endpoints
  - CONNECTION: PostgreSQL and ClickHouse connection pools
  - ERROR: Proper connection error handling and retries

Task 6: CREATE src/services/validation_service.py
  - IMPLEMENT: Data quality validation using Pydantic models
  - BUSINESS RULES: Transaction amount validation, customer email validation
  - QUALITY SCORING: Calculate data quality scores
  - ERROR HANDLING: Detailed validation error responses

Task 7: CREATE src/services/ingestion_service.py
  - IMPLEMENT: Batch and streaming data ingestion logic
  - DEPENDENCIES: ValidationService for data quality checks
  - STORAGE: Persist data to PostgreSQL with proper error handling
  - ASYNC: Use async patterns for database operations

Task 8: CREATE src/api/v1/ingestion.py
  - IMPLEMENT: FastAPI endpoints for data ingestion
  - MODELS: Use request/response models from API contracts
  - VALIDATION: Automatic Pydantic validation with custom error responses
  - ASYNC: All endpoints async with proper database session handling

Task 9: CREATE src/services/analytics_service.py
  - IMPLEMENT: Business metrics calculation (revenue, conversion rates)
  - QUERIES: ClickHouse queries for analytical data
  - AGGREGATION: Time-based aggregations (daily, weekly, monthly)
  - CACHING: Response caching for frequently accessed metrics

Task 10: CREATE src/api/v1/analytics.py
  - IMPLEMENT: Analytics query endpoints with filtering and pagination
  - PARAMETERS: Time ranges, filters, aggregation options
  - RESPONSE: Structured analytics data with metadata
  - PERFORMANCE: Query optimization and caching strategies

Task 11: CREATE src/etl/dags/transaction_processing.py
  - IMPLEMENT: Airflow DAG for transaction data processing
  - TASKS: Extract, transform, validate, load operations
  - DEPENDENCIES: Proper task ordering and error handling
  - SCHEDULE: Appropriate scheduling for data freshness requirements

Task 12: CREATE src/main.py
  - IMPLEMENT: FastAPI application setup with all routers
  - MIDDLEWARE: CORS, error handling, request logging
  - STARTUP: Database connection initialization
  - HEALTH: Health check endpoints for monitoring

Task 13: CREATE comprehensive test suite
  - UNIT TESTS: All service methods with positive and negative cases
  - INTEGRATION: API endpoint testing with test database
  - E2E: Complete workflow testing from ingestion to analytics
  - FIXTURES: Reusable test data and database fixtures

Task 14: CREATE Docker configuration
  - DOCKERFILE: Multi-stage build with production optimizations
  - COMPOSE: Development environment with all services
  - VOLUMES: Proper data persistence and log access
  - NETWORKS: Service communication and security

Task 15: CREATE scripts and documentation
  - SETUP: Development environment initialization script
  - MIGRATIONS: Database migration management
  - SAMPLE DATA: Test data generation for development
  - README: Comprehensive project documentation (LAST)
```

### Implementation Patterns & Key Details

```python
# FastAPI async endpoint pattern
@router.post("/ingest/batch", response_model=BatchIngestionResponse)
async def ingest_batch_data(
    request: BatchIngestionRequest,
    session: AsyncSession = Depends(get_db_session),
    ingestion_service: IngestionService = Depends()
) -> BatchIngestionResponse:
    # PATTERN: Pydantic automatic validation
    # CRITICAL: Use async session management
    try:
        result = await ingestion_service.process_batch(request, session)
        return BatchIngestionResponse(
            status="accepted",
            batch_id=result.batch_id,
            records_received=len(request.data)
        )
    except ValidationError as e:
        # PATTERN: Structured error responses
        raise HTTPException(status_code=400, detail=str(e))

# Service method pattern with error handling
async def process_transaction_data(
    self,
    transactions: List[Dict[str, Any]],
    session: AsyncSession
) -> ProcessingResult:
    # PATTERN: Data validation first
    validated_transactions = []
    for txn_data in transactions:
        try:
            txn = Transaction(**txn_data)
            validated_transactions.append(txn)
        except ValidationError as e:
            # CRITICAL: Log validation errors for monitoring
            logger.warning(f"Transaction validation failed: {e}")
            continue

    # PATTERN: Batch database operations for performance
    # GOTCHA: PostgreSQL has limits on batch insert size
    batch_size = 1000
    for i in range(0, len(validated_transactions), batch_size):
        batch = validated_transactions[i:i + batch_size]
        await self._persist_batch(batch, session)

    return ProcessingResult(
        processed_count=len(validated_transactions),
        error_count=len(transactions) - len(validated_transactions)
    )

# Airflow DAG pattern
from airflow import DAG
from airflow.operators.python import PythonOperator

def create_transaction_processing_dag():
    dag = DAG(
        'transaction_processing',
        default_args={
            'owner': 'data-engineering',
            'retries': 3,
            'retry_delay': timedelta(minutes=5),
        },
        schedule_interval='@hourly',
        catchup=False,
    )

    # PATTERN: Task dependencies with proper error handling
    extract_task = PythonOperator(
        task_id='extract_transactions',
        python_callable=extract_transaction_data,
        dag=dag,
    )

    validate_task = PythonOperator(
        task_id='validate_data_quality',
        python_callable=validate_transaction_quality,
        dag=dag,
    )

    # CRITICAL: Proper task dependencies
    extract_task >> validate_task

    return dag

# Database model pattern
class TransactionModel(Base):
    __tablename__ = 'transactions'

    # CRITICAL: Entity-specific primary key naming
    transaction_id = Column(String(50), primary_key=True)
    customer_id = Column(String(50), ForeignKey('customers.customer_id'))
    amount = Column(Numeric(10, 2), nullable=False)
    currency = Column(String(3), nullable=False)
    status = Column(Enum(TransactionStatus), nullable=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), onupdate=func.now())

    # PATTERN: Relationships for data integrity
    customer = relationship("CustomerModel", back_populates="transactions")
```

### Integration Points

```yaml
DATABASE:
  - postgresql: "Primary database for transactional data"
  - clickhouse: "Analytics database for aggregated metrics"
  - migrations: "Alembic for schema management"

CONFIG:
  - environment: "Development, staging, production configurations"
  - secrets: "Database URLs, API keys via environment variables"
  - logging: "Structured logging with correlation IDs"

MONITORING:
  - health: "/health endpoint for service monitoring"
  - metrics: "Prometheus-compatible metrics endpoint"
  - logging: "JSON structured logs for aggregation"

EXTERNAL:
  - apis: "External data source integrations"
  - storage: "File-based data ingestion support"
  - notifications: "Alert system for data quality issues"
```

## Validation Loop

### Level 1: Syntax & Style (Immediate Feedback)

```bash
# Run after each file creation - fix before proceeding
uv run ruff check src/ --fix        # Auto-format and fix linting issues
uv run mypy src/                    # Type checking
uv run ruff format src/             # Ensure consistent formatting

# Expected: Zero errors. If errors exist, READ output and fix before proceeding.
```

### Level 2: Unit Tests (Component Validation)

```bash
# Test each component as it's created
uv run pytest src/services/tests/ -v
uv run pytest src/api/tests/ -v
uv run pytest src/models/tests/ -v

# Coverage validation
uv run pytest src/ --cov=src --cov-report=term-missing --cov-fail-under=80

# Expected: All tests pass with 80%+ coverage
```

### Level 3: Integration Testing (System Validation)

```bash
# Database connectivity
uv run python -c "from src.config.database import test_connection; test_connection()"

# FastAPI service startup
uv run uvicorn src.main:app --reload &
sleep 5

# Health check validation
curl -f http://localhost:8000/health || echo "Health check failed"

# API endpoint testing
curl -X POST http://localhost:8000/api/v1/ingest/batch \
  -H "Content-Type: application/json" \
  -d '{"source_id": "test", "data_type": "transactions", "format": "json", "data": [{"transaction_id": "test123", "amount": 100.0}]}' \
  | jq .

# Analytics endpoint testing
curl http://localhost:8000/api/v1/analytics/metrics?metric_type=revenue\&time_range.start=2025-01-01T00:00:00Z\&time_range.end=2025-01-31T23:59:59Z\&granularity=daily | jq .

# Expected: All endpoints responding correctly with valid JSON
```

### Level 4: Creative & Domain-Specific Validation

```bash
# Docker environment validation
docker-compose up -d
sleep 30
docker-compose ps  # All services should be healthy

# ETL pipeline validation (if Airflow configured)
# Test DAG syntax and execution
python -c "from src.etl.dags.transaction_processing import dag; print('DAG validation passed')"

# Data quality validation with sample data
uv run python scripts/sample_data_generator.py --count 1000
uv run python scripts/validate_data_quality.py --source test_data.json

# Performance validation
# Basic load testing on ingestion endpoint
ab -n 100 -c 10 -T 'application/json' -p sample_data.json http://localhost:8000/api/v1/ingest/batch

# Expected: Consistent response times under 3 seconds, no errors
```

## Final Validation Checklist

### Technical Validation

- [ ] All 4 validation levels completed successfully
- [ ] All tests pass: `uv run pytest src/ -v`
- [ ] No linting errors: `uv run ruff check src/`
- [ ] No type errors: `uv run mypy src/`
- [ ] No formatting issues: `uv run ruff format src/ --check`
- [ ] Docker environment builds and runs successfully

### Feature Validation

- [ ] Data ingestion APIs accept and validate multiple data formats
- [ ] ETL pipelines process data and calculate business KPIs correctly
- [ ] Analytics APIs return correct metrics within 3 second response time
- [ ] Data quality validation achieves 99.5% accuracy on test data
- [ ] All API endpoints follow contract specifications exactly
- [ ] Error handling provides meaningful error messages

### Code Quality Validation

- [ ] Follows existing codebase patterns and naming conventions
- [ ] File placement matches desired codebase tree structure
- [ ] Entity-specific database naming conventions followed
- [ ] Async patterns used consistently throughout
- [ ] Dependencies properly managed with UV
- [ ] Configuration externalized through environment variables

### Documentation & Deployment

- [ ] CLAUDE.md created first with project-specific conventions
- [ ] README.md created last with comprehensive setup instructions
- [ ] API documentation automatically generated and accessible
- [ ] Docker configuration enables easy development environment setup
- [ ] Sample data generation enables immediate testing and validation

---

## Anti-Patterns to Avoid

- ❌ Don't create synchronous functions in async context
- ❌ Don't hardcode database connections - use dependency injection
- ❌ Don't skip data validation - use Pydantic models consistently
- ❌ Don't ignore database transaction boundaries
- ❌ Don't create generic error messages - be specific
- ❌ Don't skip testing for "simple" functions
- ❌ Don't edit pyproject.toml directly - use UV commands
- ❌ Don't mix entity naming conventions within the codebase

**Confidence Score**: 9/10 for one-pass implementation success

This PRP provides comprehensive context, specific patterns, and detailed validation steps needed to implement a complete e-commerce data platform successfully on the first attempt.