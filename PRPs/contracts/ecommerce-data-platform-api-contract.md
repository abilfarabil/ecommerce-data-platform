# E-commerce Data Platform API Contracts

## Overview
Comprehensive API contracts for the e-commerce data platform backend/frontend coordination, covering data ingestion, analytics, and dashboard APIs.

**Base URL**: `/api/v1`

---

## 1. Data Ingestion APIs

### 1.1 Batch Data Ingestion

#### POST /api/v1/ingest/batch
Submit batch data for processing from various sources.

**Request Body:**
```typescript
interface BatchIngestionRequest {
  source_id: string;           // Required: Source identifier
  data_type: DataType;         // Required: Type of data being ingested
  format: DataFormat;          // Required: Data format (json, csv, xml)
  metadata?: IngestionMetadata; // Optional: Additional metadata
  data: Record<string, any>[]; // Required: Array of data records
}

enum DataType {
  TRANSACTIONS = "transactions",
  PRODUCTS = "products",
  CUSTOMERS = "customers",
  INVENTORY = "inventory",
  ORDERS = "orders"
}

enum DataFormat {
  JSON = "json",
  CSV = "csv",
  XML = "xml",
  PARQUET = "parquet"
}

interface IngestionMetadata {
  source_system?: string;
  extraction_timestamp?: string; // ISO 8601
  schema_version?: string;
  compression?: string;
}
```

**Response (201 Created):**
```typescript
interface BatchIngestionResponse {
  status: "accepted" | "rejected";
  batch_id: string;
  records_received: number;
  validation_status: "pending" | "passed" | "failed";
  estimated_processing_time: string;
  created_at: string; // ISO 8601
  errors?: ValidationError[];
}
```

**Validation Rules:**
- `source_id`: Required, 3-50 characters, alphanumeric + hyphens
- `data_type`: Required, must be valid enum value
- `format`: Required, must be valid enum value
- `data`: Required, non-empty array, max 10,000 records per batch

---

### 1.2 Streaming Data Ingestion

#### POST /api/v1/ingest/stream
Submit real-time streaming data.

**Request Body:**
```typescript
interface StreamIngestionRequest {
  source_id: string;
  data_type: DataType;
  timestamp: string; // ISO 8601
  data: Record<string, any>;
  correlation_id?: string;
}
```

**Response (202 Accepted):**
```typescript
interface StreamIngestionResponse {
  status: "accepted";
  record_id: string;
  timestamp: string;
  processing_latency_ms: number;
}
```

---

### 1.3 Ingestion Status Monitoring

#### GET /api/v1/ingest/batches/{batch_id}/status
Get batch processing status.

**Response (200 OK):**
```typescript
interface BatchStatusResponse {
  batch_id: string;
  status: "processing" | "completed" | "failed" | "cancelled";
  progress: {
    total_records: number;
    processed_records: number;
    failed_records: number;
    progress_percentage: number;
  };
  started_at: string;
  completed_at?: string;
  errors?: ProcessingError[];
}
```

#### GET /api/v1/ingest/sources
List available data sources.

**Query Parameters:**
- `page`: number (default: 0)
- `size`: number (default: 20, max: 100)
- `active_only`: boolean (default: true)

**Response (200 OK):**
```typescript
interface DataSourcesResponse {
  content: DataSource[];
  totalElements: number;
  totalPages: number;
  size: number;
  number: number;
}

interface DataSource {
  source_id: string;
  name: string;
  type: "api" | "database" | "file" | "stream";
  status: "active" | "inactive" | "error";
  last_ingestion: string;
  total_records: number;
  configuration: Record<string, any>;
}
```

---

## 2. Analytics APIs

### 2.1 Business Metrics

#### GET /api/v1/analytics/metrics
Retrieve business metrics with filtering and aggregation.

**Query Parameters:**
```typescript
interface MetricsQueryParams {
  metric_type: MetricType;           // Required
  time_range: TimeRange;             // Required
  granularity: Granularity;          // Required
  filters?: MetricFilters;           // Optional
  aggregation?: AggregationType;     // Optional
  include_comparison?: boolean;      // Optional: Include period comparison
}

enum MetricType {
  REVENUE = "revenue",
  TRANSACTIONS = "transactions",
  CUSTOMERS = "customers",
  INVENTORY = "inventory",
  CONVERSION = "conversion",
  AOV = "average_order_value"
}

interface TimeRange {
  start: string; // ISO 8601
  end: string;   // ISO 8601
}

enum Granularity {
  HOURLY = "hourly",
  DAILY = "daily",
  WEEKLY = "weekly",
  MONTHLY = "monthly",
  QUARTERLY = "quarterly"
}

interface MetricFilters {
  product_category?: string[];
  customer_segment?: string[];
  region?: string[];
  channel?: string[];
  currency?: string;
}

enum AggregationType {
  SUM = "sum",
  AVG = "avg",
  COUNT = "count",
  MIN = "min",
  MAX = "max"
}
```

**Response (200 OK):**
```typescript
interface MetricsResponse {
  metric_type: MetricType;
  time_range: TimeRange;
  granularity: Granularity;
  data: MetricDataPoint[];
  summary: MetricSummary;
  comparison?: ComparisonData;
  metadata: QueryMetadata;
}

interface MetricDataPoint {
  timestamp: string;
  value: number;
  additional_metrics?: Record<string, number>;
}

interface MetricSummary {
  total: number;
  average: number;
  min: number;
  max: number;
  change_from_previous: number;
  change_percentage: number;
}

interface ComparisonData {
  previous_period: MetricDataPoint[];
  variance: number;
  variance_percentage: number;
}

interface QueryMetadata {
  total_records: number;
  query_time_ms: number;
  cache_hit: boolean;
  data_freshness: string; // ISO 8601
}
```

---

### 2.2 Customer Analytics

#### GET /api/v1/analytics/customers/segments
Get customer segmentation data.

**Response (200 OK):**
```typescript
interface CustomerSegmentationResponse {
  segments: CustomerSegment[];
  total_customers: number;
  segmentation_date: string;
}

interface CustomerSegment {
  segment_id: string;
  name: string;
  description: string;
  customer_count: number;
  percentage: number;
  avg_lifetime_value: number;
  avg_order_value: number;
  retention_rate: number;
  characteristics: Record<string, any>;
}
```

#### GET /api/v1/analytics/customers/{customer_id}/profile
Get detailed customer analytics profile.

**Response (200 OK):**
```typescript
interface CustomerProfileResponse {
  customer_id: string;
  segment: string;
  lifetime_value: number;
  total_orders: number;
  avg_order_value: number;
  first_purchase_date: string;
  last_purchase_date: string;
  predicted_next_purchase?: string;
  risk_score: number;
  preferences: CustomerPreferences;
  behavior_metrics: BehaviorMetrics;
}
```

---

### 2.3 Product Analytics

#### GET /api/v1/analytics/products/performance
Get product performance metrics.

**Query Parameters:**
- `category`: string (optional)
- `time_range`: TimeRange (required)
- `sort_by`: "revenue" | "quantity" | "margin" (default: revenue)
- `limit`: number (default: 50, max: 500)

**Response (200 OK):**
```typescript
interface ProductPerformanceResponse {
  products: ProductMetrics[];
  category_summary?: CategorySummary;
  time_range: TimeRange;
}

interface ProductMetrics {
  product_id: string;
  name: string;
  category: string;
  revenue: number;
  quantity_sold: number;
  margin: number;
  conversion_rate: number;
  inventory_turnover: number;
  trend: "increasing" | "decreasing" | "stable";
}
```

---

## 3. Dashboard APIs

### 3.1 Dashboard Configuration

#### GET /api/v1/dashboards
List available dashboards.

**Response (200 OK):**
```typescript
interface DashboardsResponse {
  dashboards: Dashboard[];
}

interface Dashboard {
  dashboard_id: string;
  name: string;
  description: string;
  category: "executive" | "operations" | "finance" | "marketing";
  widgets: DashboardWidget[];
  created_at: string;
  updated_at: string;
  is_public: boolean;
}

interface DashboardWidget {
  widget_id: string;
  type: WidgetType;
  title: string;
  position: WidgetPosition;
  configuration: WidgetConfiguration;
  refresh_interval: number; // seconds
}

enum WidgetType {
  METRIC_CARD = "metric_card",
  LINE_CHART = "line_chart",
  BAR_CHART = "bar_chart",
  PIE_CHART = "pie_chart",
  TABLE = "table",
  MAP = "map"
}
```

---

### 3.2 Real-time Dashboard Data

#### GET /api/v1/dashboards/{dashboard_id}/data
Get real-time dashboard data.

**Response (200 OK):**
```typescript
interface DashboardDataResponse {
  dashboard_id: string;
  data: Record<string, WidgetData>;
  last_updated: string;
  next_refresh: string;
}

interface WidgetData {
  widget_id: string;
  data: any;
  status: "success" | "loading" | "error";
  error_message?: string;
  cache_status: "hit" | "miss" | "stale";
}
```

#### WebSocket: /ws/dashboards/{dashboard_id}
Real-time dashboard updates via WebSocket.

**Message Format:**
```typescript
interface DashboardUpdate {
  type: "widget_update" | "dashboard_refresh" | "error";
  widget_id?: string;
  data?: any;
  timestamp: string;
  error?: string;
}
```

---

### 3.3 Alerting System

#### GET /api/v1/alerts
Get active alerts.

**Query Parameters:**
- `severity`: "low" | "medium" | "high" | "critical"
- `status`: "active" | "acknowledged" | "resolved"
- `limit`: number (default: 100)

**Response (200 OK):**
```typescript
interface AlertsResponse {
  alerts: Alert[];
  summary: AlertSummary;
}

interface Alert {
  alert_id: string;
  title: string;
  description: string;
  severity: "low" | "medium" | "high" | "critical";
  status: "active" | "acknowledged" | "resolved";
  metric_type: string;
  threshold_value: number;
  current_value: number;
  created_at: string;
  acknowledged_at?: string;
  resolved_at?: string;
}
```

#### POST /api/v1/alerts/{alert_id}/acknowledge
Acknowledge an alert.

**Response (200 OK):**
```typescript
interface AlertAcknowledgeResponse {
  alert_id: string;
  status: "acknowledged";
  acknowledged_at: string;
  acknowledged_by: string;
}
```

---

## 4. Data Quality APIs

### 4.1 Data Quality Monitoring

#### GET /api/v1/data-quality/reports
Get data quality reports.

**Query Parameters:**
- `source_id`: string (optional)
- `data_type`: DataType (optional)
- `time_range`: TimeRange (required)

**Response (200 OK):**
```typescript
interface DataQualityReportsResponse {
  reports: DataQualityReport[];
  overall_score: number;
  trend: "improving" | "declining" | "stable";
}

interface DataQualityReport {
  report_id: string;
  source_id: string;
  data_type: DataType;
  quality_score: number;
  checks_performed: QualityCheck[];
  issues_found: QualityIssue[];
  generated_at: string;
}

interface QualityCheck {
  check_type: "completeness" | "accuracy" | "consistency" | "validity";
  passed: boolean;
  score: number;
  details: string;
}

interface QualityIssue {
  issue_type: string;
  severity: "low" | "medium" | "high";
  count: number;
  sample_records: string[];
  suggested_action: string;
}
```

---

## 5. Common Data Types & Error Handling

### 5.1 Common Response Wrapper

```typescript
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: ApiError;
  timestamp: string;
  request_id: string;
}

interface ApiError {
  code: string;
  message: string;
  details?: Record<string, any>;
  field_errors?: FieldError[];
}

interface FieldError {
  field: string;
  message: string;
  rejected_value?: any;
}
```

### 5.2 Pagination

```typescript
interface PageRequest {
  page: number;    // 0-based
  size: number;    // 1-100
  sort?: string;   // field,direction (e.g., "name,asc")
}

interface PageResponse<T> {
  content: T[];
  totalElements: number;
  totalPages: number;
  size: number;
  number: number;
  first: boolean;
  last: boolean;
}
```

### 5.3 HTTP Status Codes

| Status | Usage |
|--------|-------|
| 200 | OK - Successful GET/PUT |
| 201 | Created - Successful POST |
| 202 | Accepted - Async processing started |
| 204 | No Content - Successful DELETE |
| 400 | Bad Request - Validation errors |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource doesn't exist |
| 409 | Conflict - Duplicate resource |
| 422 | Unprocessable Entity - Business logic error |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error - Server error |
| 503 | Service Unavailable - Maintenance mode |

### 5.4 Error Response Examples

**Validation Error (400):**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "field_errors": [
      {
        "field": "source_id",
        "message": "Source ID is required",
        "rejected_value": null
      },
      {
        "field": "data",
        "message": "Data array cannot be empty",
        "rejected_value": []
      }
    ]
  },
  "timestamp": "2025-01-15T14:30:00Z",
  "request_id": "req_12345"
}
```

**Business Logic Error (422):**
```json
{
  "success": false,
  "error": {
    "code": "INSUFFICIENT_DATA_QUALITY",
    "message": "Data quality score too low for processing",
    "details": {
      "quality_score": 45,
      "minimum_required": 70,
      "failed_checks": ["completeness", "accuracy"]
    }
  },
  "timestamp": "2025-01-15T14:30:00Z",
  "request_id": "req_12346"
}
```

---

## 6. Authentication & Authorization

### 6.1 Authentication
- **Method**: Bearer Token (JWT)
- **Header**: `Authorization: Bearer <token>`
- **Token Expiry**: 24 hours
- **Refresh Endpoint**: `POST /api/v1/auth/refresh`

### 6.2 Rate Limiting
- **General APIs**: 1000 requests/hour per API key
- **Analytics APIs**: 500 requests/hour per API key
- **Streaming APIs**: 10,000 requests/hour per API key
- **Headers**: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

### 6.3 CORS Configuration
- **Allowed Origins**: Frontend domains (configurable)
- **Allowed Methods**: GET, POST, PUT, DELETE, OPTIONS
- **Allowed Headers**: Content-Type, Authorization, X-Requested-With
- **Credentials**: Supported

---

## 7. Implementation Notes

### 7.1 Backend Implementation (FastAPI)
```python
from pydantic import BaseModel, Field
from typing import List, Optional
from enum import Enum

# Example model matching TypeScript interface
class BatchIngestionRequest(BaseModel):
    source_id: str = Field(..., min_length=3, max_length=50)
    data_type: DataType
    format: DataFormat
    metadata: Optional[IngestionMetadata] = None
    data: List[dict] = Field(..., min_items=1, max_items=10000)

# FastAPI endpoint
@app.post("/api/v1/ingest/batch", response_model=BatchIngestionResponse)
async def ingest_batch_data(request: BatchIngestionRequest):
    # Implementation
    pass
```

### 7.2 Frontend Implementation (TypeScript/React)
```typescript
// API client configuration
const apiClient = axios.create({
  baseURL: '/api/v1',
  headers: {
    'Content-Type': 'application/json',
  },
});

// Type-safe API call
async function ingestBatchData(
  request: BatchIngestionRequest
): Promise<BatchIngestionResponse> {
  const response = await apiClient.post<ApiResponse<BatchIngestionResponse>>(
    '/ingest/batch',
    request
  );
  return response.data.data!;
}

// React Query hook
function useIngestBatch() {
  return useMutation(ingestBatchData, {
    onSuccess: (data) => {
      // Handle success
    },
    onError: (error) => {
      // Handle error
    }
  });
}
```

### 7.3 Validation Rules Alignment

**Backend (Pydantic):**
```python
class MetricsQueryParams(BaseModel):
    metric_type: MetricType
    time_range: TimeRange
    granularity: Granularity = Field(default=Granularity.DAILY)
    filters: Optional[MetricFilters] = None
```

**Frontend (Zod):**
```typescript
const MetricsQuerySchema = z.object({
  metric_type: z.nativeEnum(MetricType),
  time_range: TimeRangeSchema,
  granularity: z.nativeEnum(Granularity).default(Granularity.DAILY),
  filters: MetricFiltersSchema.optional()
});
```

---

This API contract provides comprehensive type-safe interfaces for all major platform operations, ensuring consistent backend/frontend integration and enabling scalable e-commerce data platform development.