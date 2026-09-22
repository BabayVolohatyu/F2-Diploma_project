### Звіт експертної оцінки та валідації Thesis Proposal

**Роль:** Senior Software Engineer / Голова Екзаменаційної Комісії зі спеціальності F2 «Інженерія програмного забезпечення»

### 1. Аналіз відповідності інженерній глибині спеціальності F2

Наданий **Thesis Proposal** повністю виходить за межі балових CRUD-систем і повністю відповідає (та за деякими показниками перевищує) стандарти кваліфікаційного рівня бакалавра спеціальності **F2 Інженерія ПЗ**.

#### Сильні інженерні сторони проекту:

- **Мультипарадигмальна дата-інженерія:** Поєднання реляційної СУБД (PostgreSQL) для транзакційних даних, графової СУБД (Neo4j) для аналізу афілійованості та in-memory сховища (Redis) для distributed locks і кешування.
    
- **Подієво-керована асинхронність:** Використання RabbitMQ у поєднанні з Quartz.NET/Hangfire вирішує проблему низької затримки при обробці гетерогенних даних із зовнішніх API (Prozorro, DREAM, ЄДЕССБ).
    
- **Прикладна інженерія даних та ML:** Інтеграція ML.NET для автоматизованого скорингу аномалій додає аналітичної та науково-практичної цінності.
    

### 2. Рекомендації щодо архітектурного та інфраструктурного ускладнення (Enterprise-Level)

Щоб на захисті проект виглядав як бездоганне **Enterprise-рішення**, рекомендую зафіксувати в архітектурі такі інженерні патерни та практики:

1. **Забезпечення консистентності даних (Transactional Outbox Pattern):** При зборі даних з зовнішніх API та збереженні їх у PostgreSQL, публікацію подій у RabbitMQ слід виконувати через патерн Transactional Outbox. Це унеможливить втрату подій про аномалії при збоях у мережі.
    
2. **Розподілене блокування задач (Redis Distributed Lock / Redlock):** Оскільки Quartz.NET/Hangfire працюватимуть у фоні, для запобігання дублюванню запитів до Prozorro/DREAM API під час горизонтального масштабування воркерів обов'язково використати Redis для локування критичних секцій.
    
3. **Resilience & Fault Tolerance (Polly Policies):** На рівні HTTP-клієнтів взаємодії з зовнішніми реєстрами впровадити патерни **Retry**, **Circuit Breaker** та **Rate Limiting** (через бібліотеку Polly) для захисту від таймаутів та блокувань за превищення лімітів запитів.
    
4. **DevSecOps & Code Quality Pipeline:** У GitHub Actions конвеєр інтегрувати:
    
    - **SonarQube:** контроль цикломатичної складності та технічного боргу.
        
    - **Trivy / Snyk:** автоматичне сканування Docker-образів та NuGet/NPM залежностей на відомі вразливості (CVE).
        
5. **Observability & Health Checks:** Додати `.NET Health Checks` endpoint та збір метрик за допомогою `OpenTelemetry/Prometheus` для відстеження затримки (latency) ML-інференсу та довжини черг у RabbitMQ.
    

### 3. Стилістичне та технічне очищення тексту

Опис ідеї очищено від академічних канцеляризмів, суб'єктивних оцінок та розмовного стилю. Формулювання зведено до чітких інженерних термінів, прийнятих у міжнародній практиці Software Engineering та System Design.

### 4. Фінальний верифікований Thesis Proposal

#### 🇺🇦 Українська версія

**Тема:** Інформаційна система автоматизованого виявлення аномалій та оцінки ризиків у публічних закупівлях для відбудови інфраструктури України

**Спеціальність:** F2 Інженерія програмного забезпечення

##### 1. Актуальність та проблематика

Відбудова інфраструктури України вимагає прозорого та оперативного моніторингу закупівельних процесів. Основними інженерно-технічними викликами є:

- **Висока щільність корупційних ризиків:** наявність каскадних схем (змови, фірми-одноденки, штучне подрібнення лотів).
    
- **Гетерогенність та фрагментованість даних:** інформація розподілена між Prozorro (OCDS API), DREAM API, ЄДЕССБ та data.gov.ua з різними форматами та лімітами частоти запитів.
    
- **Обмеження обробки даних у реальному часі:** класичні правила аналізу не забезпечують необхідної швидкодії при обробці терабайтних обсягів неструктурованої інформації.
    

##### 2. Концепт продукту та архітектура

Розподілена подієво-керована (Event-Driven) аналітична платформа на базі мікросервісної архітектури для автоматизованого ML-скорингу ризиків, графового аналізу афілійованості та геопросторової візуалізації закупівель.

- **Архітектурні патерни:** CQRS, Event-Driven Architecture, Transactional Outbox, Repository / Unit of Work, Clean Architecture.
    
- **Надійність та масштабованість:** Асинхронна обробка через RabbitMQ, distributed caching та locks у Redis, гарантована доставка подій та ізоляція доменів.
    

##### 3. Ключовий функціонал (Core Features)

1. **Event-Driven ETL Ingestion Engine:** Асинхронний збір та нормалізація даних з Prozorro, DREAM, ЄДЕССБ у єдину доменну модель із підтримкою Polly Resilience Policies.
    
2. **Graph-based Network Analysis Engine:** Побудова графа зв'язків у **Neo4j** для виявлення прихованої афілійованості контрагентів, спільних бенефіціарів та картельних змов.
    
3. **ML-based Risk Scoring Engine:** Автоматизований скоринг ризиків на базі **ML.NET** (Gradient Boosting / Isolation Forest) з ранжуванням тендерів за рівнем ризикованості.
    
4. **Geospatial Analytics Dashboard:** Інтерактивна мапа відбудови України (Angular + Leaflet/Mapbox) з тепловими картами аномалій та регіональною фільтрацією.
    
5. **Orchestration & Background Jobs Engine:** Управління регулярними задачами та розрахунками за допомогою **Quartz.NET / Hangfire** із блокуваннями у Redis.
    
6. **Real-time Event Alerting:** Сповіщення користувачів через WebSockets / RabbitMQ про виявлення аномальних тендерів високого рівня ризику.
    
7. **Automated Audit Reporting (Docs-as-Code):** Автоматична генерація деталізованих PDF/JSON звітів аналізу ризиків з візуалізацією графів афілійованості.
    

##### 4. Технологічний стек

- **Languages:** C# (.NET 8/9), TypeScript
    
- **Backend:** .NET Core Web API, EF Core, Polly
    
- **Frontend:** Angular 17+, RxJS, TailwindCSS, Leaflet.js / Mapbox GL
    
- **Databases:** PostgreSQL (Primary OLTP), Neo4j (Graph DB), Redis (In-Memory Cache & Distributed Lock)
    
- **Machine Learning:** ML.NET (Anomaly Detection, Classification)
    
- **Messaging & Scheduling:** RabbitMQ, Quartz.NET / Hangfire
    
- **DevSecOps & Quality:** Docker, GitHub Actions, SonarQube, Trivy
    
- **Observability:** Prometheus, Grafana, Serilog
    

#### 🇬🇧 English Version

**Topic:** Automated Anomaly Detection and Risk Assessment Information System in Public Procurement for Ukraine's Infrastructure Reconstruction

**Specialty:** F2 Software Engineering

##### 1. Problem Statement & Relevance

Infrastructure reconstruction in Ukraine requires transparent and real-time monitoring of procurement operations. Key engineering challenges include:

- **High density of fraud vectors:** Collusion schemes, shell companies, price inflation, and artificially split procurement lots.
    
- **Data Heterogeneity & Fragmentation:** Data is distributed across Prozorro (OCDS API), DREAM API, EDR, and data.gov.ua with varying data formats and rate-limiting constraints.
    
- **Latency Bottlenecks in Data Processing:** Traditional rule-based evaluation mechanisms fail to process terabytes of unstructured procurement data in real-time.
    

##### 2. Product Concept & Architecture

A distributed, Event-Driven microservices platform designed for automated ML risk scoring, graph-based connectivity analysis, and geospatial procurement analytics.

- **Architectural Patterns:** CQRS, Event-Driven Architecture, Transactional Outbox, Repository / Unit of Work, Clean Architecture.
    
- **Reliability & Scalability:** Asynchronous processing via RabbitMQ, distributed caching/locking via Redis, guaranteed event delivery, and strict domain isolation.
    

##### 3. Core Features

1. **Event-Driven ETL Ingestion Engine:** Asynchronous ingestion and data normalization pipeline integrating Prozorro, DREAM, and state registries with fault-tolerant Polly Policies.
    
2. **Graph-based Network Analysis Engine:** Knowledge Graph execution in **Neo4j** to uncover hidden affiliations, shared ultimate beneficial owners (UBOs), and cartel collusion.
    
3. **ML-based Risk Scoring Engine:** Automated risk classification via **ML.NET** (Gradient Boosting / Isolation Forest) categorizing tenders by severity levels.
    
4. **Geospatial Analytics Dashboard:** Interactive spatial dashboard (Angular + Leaflet/Mapbox) displaying risk heatmaps and infrastructure restoration progress across regions.
    
5. **Orchestration & Background Task Engine:** Background job management via **Quartz.NET / Hangfire** with Redis distributed lock coordination.
    
6. **Real-time Event Alerting System:** Push notification pipeline delivering high-risk transaction alerts over WebSockets and RabbitMQ.
    
7. **Automated Audit Reporting:** PDF/JSON audit report engine generating risk factor chains and subgraph visualization artifacts.
    

##### 4. Technology Stack

- **Languages:** C# (.NET 8/9), TypeScript
    
- **Backend:** .NET Core Web API, EF Core, Polly
    
- **Frontend:** Angular 17+, RxJS, TailwindCSS, Leaflet.js / Mapbox GL
    
- **Databases:** PostgreSQL, Neo4j, Redis
    
- **Machine Learning:** ML.NET
    
- **Messaging & Tasks:** RabbitMQ, Quartz.NET / Hangfire
    
- **DevSecOps & Quality:** Docker, GitHub Actions, SonarQube, Trivy
    
- **Observability:** Prometheus, Grafana, Serilog