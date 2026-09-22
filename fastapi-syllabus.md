# FastAPI Course Outline

**對象**：熟悉 Python，但缺乏 Web 與 API 開發經驗的數據科學同仁。

---

# Chapter 1: API and FastAPI Fundamentals

## 1. API, HTTP, REST, and Webhooks

### Topics

- API 與 API contract
- HTTP request 與 response
- URL、HTTP method、header、body、status code 與 JSON
- REST API 的資源導向設計
- Webhook 的基本概念

### Description

建立 API 的基本模型，理解 client 如何透過 HTTP 與 service 交換資料。簡易說明 REST 與 Webhook 只比較互動方式 (還不涉及程式實作)。

### References

- [MDN - HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview)
- [MDN - API](https://developer.mozilla.org/en-US/docs/Glossary/API)：說明 API 作為介面契約的概念。
- [MDN HTTP Request Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
- [MDN HTTP Response Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)
- [MDN - REST](https://developer.mozilla.org/en-US/docs/Glossary/REST)
- [FastAPI - OpenAPI Webhooks](https://fastapi.tiangolo.com/advanced/openapi-webhooks/)：僅閱讀 Webhook 概念，不實作 OpenAPI Webhooks。

## 2. FastAPI Project

### Topics

- `FastAPI` application
- `APIRouter` 與 `include_router()`
- Router prefix 與 tags
- Path operation 的 `summary` 與 `description`
- Request schema 與 response schema
- Router、schema 與 service 的檔案分工
- Swagger UI、ReDoc 與 OpenAPI schema

### Description

從第一個範例就使用 Multiple Files 與 `APIRouter`，不先建立大型單檔案 application。Router 處理 HTTP 交互，schema 定義資料契約，service 放置主要處理邏輯。建立 router 與 endpoint 時同步加入 tags、summary 與 description，並直接在 Swagger UI、ReDoc 與 OpenAPI schema 觀察產生的 API 文件。

```text
app/
├── main.py
├── api/
│   ├── router.py
│   └── routes/
│       ├── health.py
│       └── jobs.py
├── schemas/
│   └── job.py
└── services/
    └── job_service.py
```

### References

- [FastAPI - First Steps](https://fastapi.tiangolo.com/tutorial/first-steps/)
- [FastAPI - Request Body](https://fastapi.tiangolo.com/tutorial/body/)
- [FastAPI - Path Operation Configuration](https://fastapi.tiangolo.com/tutorial/path-operation-configuration/)
- [FastAPI - Bigger Applications: Multiple Files](https://fastapi.tiangolo.com/tutorial/bigger-applications/)
- [Metadata and Docs URLs](https://fastapi.tiangolo.com/tutorial/metadata/)


## 3. Path Operations and Parameters

### Topics

- `GET`、`POST`、`PUT`、`PATCH` 與 `DELETE`
- `PUT` 與 `PATCH` 的語意差異
- Path parameter
- Query parameter
- Required、optional 與 default value
- 字串、數值與邊界驗證

### Description

使用資料處理 job 的建立、查詢、更新與刪除說明 HTTP method 和 path operation，並區分 `PUT` 完整替換與 `PATCH` 部分更新的語意。參數範例聚焦常用的型別轉換、必填與選填值，以及字串長度與數值範圍。

### References

- [FastAPI - Path Parameters](https://fastapi.tiangolo.com/tutorial/path-params/)
- [FastAPI - Query Parameters](https://fastapi.tiangolo.com/tutorial/query-params/)
- [FastAPI - Query Parameters and String Validations](https://fastapi.tiangolo.com/tutorial/query-params-str-validations/)
- [FastAPI - Path Parameters and Numeric Validations](https://fastapi.tiangolo.com/tutorial/path-params-numeric-validations/)
- [FastAPI - Body Updates](https://fastapi.tiangolo.com/tutorial/body-updates/)：對照 `PUT` 與 `PATCH` 的更新範例。

## 4. Request Bodies and Pydantic Models

### Topics

- Pydantic request model
- Required 與 optional fields
- `Field()` 驗證
- Path、Query 與 Body 的組合
- 一層 nested model
- List 與 dict
- OpenAPI request example
- Partial update model
- 區分「未提供」與 `null`

### Description

使用單一、清楚的 request model 定義 API 輸入，透過 Pydantic 型別與 `Field()` 建立驗證規則。Nested model 只示範一層常見結構。配合 `PATCH` 建立 partial update model，並用 `model_dump(exclude_unset=True)` 區分欄位未提供與明確傳入 `null`；可接受 `None` 的型別仍須搭配預設值，欄位才可省略。

### References

- [FastAPI - Request Body](https://fastapi.tiangolo.com/tutorial/body/)
- [FastAPI - Body: Multiple Parameters](https://fastapi.tiangolo.com/tutorial/body-multiple-params/)
- [FastAPI - Body Fields](https://fastapi.tiangolo.com/tutorial/body-fields/)
- [FastAPI - Body Nested Models](https://fastapi.tiangolo.com/tutorial/body-nested-models/)
- [FastAPI - Declare Request Example Data](https://fastapi.tiangolo.com/tutorial/schema-extra-example/)
- [FastAPI - Body Updates](https://fastapi.tiangolo.com/tutorial/body-updates/)

## 5. Header Parameters

### Topics

- `Header()`
- Header 名稱轉換
- Request ID 與 client version

### Description

說明 header 適合承載的 request metadata，並以 request ID 與 client version 示範 `Header()` 的讀取與名稱轉換。

### References

- [FastAPI - Header Parameters](https://fastapi.tiangolo.com/tutorial/header-params/)

## 6. Response Contracts and Status Codes

### Topics

- Return type 與 `response_model`
- Request model 與 response model 分離
- Response data filtering
- Collection response
- `200 OK`、`201 Created` 與 `204 No Content`

### Description

將 response schema 視為 API contract，明確定義對外欄位，避免回傳內部或敏感資料。配合建立、查詢與刪除 job 的範例選擇適當 status code。

### References

- [FastAPI - Response Model: Return Type](https://fastapi.tiangolo.com/tutorial/response-model/)
- [FastAPI - Response Status Code](https://fastapi.tiangolo.com/tutorial/response-status-code/)

---

# Chapter 2: Dependency Injection, Authentication, and Browser Integration

## 1. Dependency Injection Fundamentals

### Topics

- `Depends()`
- Function dependency
- Dependency 的參數與回傳值
- Endpoint dependency
- Path operation decorator dependencies
- Router-level dependency
- 一層 dependency chain

### Description

將共用的參數解析、認證與驗證邏輯抽成 function dependency。實作 endpoint 取得 dependency 結果，以及將只需執行的檢查套用在單一 path operation 或整個 router。不延伸至 class dependency、global dependency、`yield`、複雜 sub-dependencies 與 advanced dependencies。

### References

- [FastAPI - Dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/)
- [FastAPI - Sub-dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/sub-dependencies/)
- [FastAPI - Dependencies in Path Operation Decorators](https://fastapi.tiangolo.com/tutorial/dependencies/dependencies-in-path-operation-decorators/)
- [FastAPI - Bigger Applications: Multiple Files](https://fastapi.tiangolo.com/tutorial/bigger-applications/)：參考 router-level dependencies。

## 2. Application Lifespan

### Topics

- Application startup 與 shutdown
- `lifespan` async context manager
- Application-scoped shared resource
- 資源初始化與釋放

### Description

以應用啟動時載入共享的資料處理資源或模型為範例，說明只需初始化一次的物件不應在每次 request 中重新建立，並在 shutdown 時釋放資源。

### References

- [FastAPI - Lifespan Events](https://fastapi.tiangolo.com/advanced/events/)

## 3. Bearer Tokens and Password Hashing

### Topics

- Authentication 與 authorization
- Password hash 與 password verification
- JSON 帳號密碼登入
- Opaque bearer token
- `Authorization: Bearer <token>`
- `HTTPBearer` 與 `HTTPAuthorizationCredentials`
- Current-user dependency
- `401 Unauthorized` 與 `WWW-Authenticate`

### Description

使用 JSON request body 接收 username 與 password，驗證 password hash 後產生隨機、不攜帶使用者資料的 opaque token。Client 以 Bearer token 呼叫受保護的 API，由 function dependency 驗證 token 並取得 current user。教學範例將 token 暫存於 application memory，重啟後 token 會消失。不使用 Form Data、OAuth2 password flow 或 JWT。

### References

- [FastAPI - Security](https://fastapi.tiangolo.com/tutorial/security/)
- [FastAPI - Security Tools](https://fastapi.tiangolo.com/reference/security/)
- [FastAPI - Get Current User](https://fastapi.tiangolo.com/tutorial/security/get-current-user/)：參考 current-user dependency 的做法，認證 scheme 仍使用 `HTTPBearer`。
- [FastAPI - OAuth2 with Password and Bearer with JWT Tokens](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/)：僅參考 password hashing 與 verification，不使用 JWT。

## 4. Middleware and Request Context

### Topics

- Middleware request／response flow
- Request duration
- Request ID
- Response header
- 使用 `Request` 取得 request context
- Middleware、dependency 與 exception handler 的責任

### Description

實作一個簡單 middleware，建立或接收 request ID、計算處理時間，並將結果放入 response header。同時說明 middleware 適合處理跨 endpoint 的邏輯，不用來取代業務層或精細授權檢查。

### References

- [FastAPI - Middleware](https://fastapi.tiangolo.com/tutorial/middleware/)
- [FastAPI - Using the Request Directly](https://fastapi.tiangolo.com/advanced/using-request-directly/)
- [FastAPI - Response Headers](https://fastapi.tiangolo.com/advanced/response-headers/)

## 5. CORS, Frontend, and Static Files

### Topics

- Origin 的組成
- Same-origin 與 cross-origin
- Browser preflight request
- `CORSMiddleware`
- `allow_origins`、methods、headers 與 credentials
- HTML 與 JavaScript `fetch()` 小範例
- `StaticFiles`

### Description

以 `localhost:5500` 的靜態前端呼叫 `localhost:8000` 的 FastAPI，觀察 browser 的 CORS 限制與 preflight request，再透過 `CORSMiddleware` 明確開放需要的 origin、method 與 header。同時使用 `StaticFiles` 展示 FastAPI 提供靜態資源的方式。

### References

- [FastAPI - CORS](https://fastapi.tiangolo.com/tutorial/cors/)
- [FastAPI - Frontend](https://fastapi.tiangolo.com/tutorial/frontend/)
- [FastAPI - Static Files](https://fastapi.tiangolo.com/tutorial/static-files/)
- [MDN - CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

---

# Chapter 3: From FastAPI Application to API Service

## 1. Request Flow and Traffic Entry Points

### Topics

- Client、HTTPS／TLS 與 FastAPI process
- Reverse proxy、load balancer 與 API gateway 的主要責任
- Forwarded headers、trusted proxy 與 URL prefix／`root_path`

### Description

以 `Client → 流量入口 → FastAPI process → 外部服務` 認識 request 路徑。流量入口可能負責 TLS termination、轉送、負載分配或 API 管理；這些功能可以由同一元件提供，並非每個部署都需要三種獨立產品。代理後方只信任已知 proxy 送出的 forwarded headers；只有 proxy 移除 URL prefix 時，才需要依部署方式設定 `root_path`。

### References

- [FastAPI - Deployment Concepts](https://fastapi.tiangolo.com/deployment/concepts/)
- [FastAPI - About HTTPS](https://fastapi.tiangolo.com/deployment/https/)
- [FastAPI - Behind a Proxy](https://fastapi.tiangolo.com/advanced/behind-a-proxy/)
- [NGINX - Reverse Proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)
- [NGINX - HTTP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)
- [AWS - What Is Amazon API Gateway?](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

## 2. Timeouts, Retries, and Resource Protection

### Topics

- Timeout、有限次 retry 與 backoff
- HTTP method 的冪等性與重複提交風險
- Rate limiting、`429 Too Many Requests` 與 `Retry-After`
- Request 大小、回傳筆數與高成本操作限制

### Description

Timeout 表示等待方停止等待，不代表後端操作沒有完成；retry 可能造成重複寫入，須依操作語意決定是否重試，必要時使用 idempotency key，並以持久化紀錄去重。以 rate limit、輸入大小與回傳量限制保護服務資源，並辨識 `429` 與 `Retry-After` 的用途。

### References

- [MDN - Idempotent Method](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)
- [AWS Builders' Library - Timeouts, Retries, and Backoff with Jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/)
- [AWS Builders' Library - Making Retries Safe with Idempotent APIs](https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/)
- [MDN - 429 Too Many Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/429)
- [OWASP API Security - Unrestricted Resource Consumption](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/)

## 3. Background Tasks and Job Queues

### Topics

- FastAPI `BackgroundTasks` 與 in-process 工作
- Job queue、worker 與 job status 查詢
- `202 Accepted` 與非同步工作的回應

### Description

以資料處理 job 比較兩種做法：`BackgroundTasks` 在回應送出後由同一 application process 執行，適合短小、可容忍遺失的工作；長時間或必須可靠重試的工作交由工作佇列與獨立 worker 處理。接受任務可回傳 `202 Accepted` 與 job ID，讓 client 後續查詢狀態；`202` 不保證任務已完成。

### References

- [FastAPI - Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/)
- [MDN - 202 Accepted](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/202)
