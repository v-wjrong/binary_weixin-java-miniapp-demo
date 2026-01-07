# 系统架构

外部请求 → DispatcherServlet → Controller → Service → Repository/Util → 返回结果
```

若将来演化为微服务架构，则可能出现以下交互路径：

- 微信回调消息 → API Gateway → Auth Service → Business Logic Service
- 登录授权 → User Service ↔ Redis Cache
- 订单创建 → Order Service → Payment Service (via REST or MQ)
- 异步通知 → Message Queue → Notification Worker

## 整体架构概览图 (Mermaid 语法)

```mermaid
graph TD
    A[微信小程序客户端] -- HTTPS/WebSocket --> B(Java Spring Boot App)
    
    subgraph 单体应用层
        B --> C[WxJava SDK]
        B --> D[Session Manager]
        B --> E[Logging & Debug Tools]
    end
    
    subgraph 数据管理层
        D --> F[(In-Memory State)]
        B -.-> G[Future DB: MySQL/PostgreSQL]
        B -.-> H[Future Cache: Redis]
        B -.-> I[Future MQ: RabbitMQ/Kafka]
    end



