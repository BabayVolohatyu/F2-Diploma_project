# Пояснювальна записка

Документація кваліфікаційної роботи: **«Інформаційна система автоматизованого виявлення аномалій та оцінки ризиків у публічних закупівлях для відбудови інфраструктури України»**.

## Структура документації
- [Вступ](0_intro.md)
- [Розділ 1. Аналіз предметної області та формування вимог](1_analysis.md)
- [Розділ 2. Проєктування](2_design.md)
- [Розділ 3. Реалізація](3_implementation.md)
- [Висновки](4_conclusions.md)
- [Thesis Proposal](proposal.md)

## Артефакт ЛР №4
- Монолітний документ: `F2_Diploma_Chapter1.docx`.
- Статус: аналітичний розділ сформовано; числові NFR зафіксовано як цільові показники прототипу, їх необхідно підтвердити тестами.
- Google Docs link: не створювався; документ надано у форматі DOCX.

## Матриця нефункціональних вимог (NFRs)

| ID | Метрика / обмеження | Цільовий критерій |
|---|---|---|
| NFR-01 | API latency | p95 ≤ 500 мс при 50 RPS і 50 одночасних сесіях |
| NFR-02 | Ingestion throughput | ≥ 10 документів/с без очікування зовнішнього API |
| NFR-03 | Queue processing | p95 ≤ 5 с для повідомлень до 64 KB за черги до 1 000 |
| NFR-04 | Worker scalability | 1 → 2 workers: throughput ≥ 1,5× |
| NFR-05 | Resilience | Retry/backoff, circuit breaker; повтор без дублікатів |
| NFR-06 | Consistency | PostgreSQL domain write + Outbox в одній транзакції |
| NFR-07 | Transport security | HTTPS/TLS 1.2+; автентифікація та рольова авторизація |
| NFR-08 | Secrets | Секрети поза Git, через environment/deployment secrets |
| NFR-09 | Recovery | Backup кожні 24 год; RPO ≤ 24 год; RTO ≤ 4 год |
| NFR-10 | Observability | Prometheus metrics для черг, імпорту, помилок і ML latency |
| NFR-11 | Maintainability | Clean Architecture; CI build/tests для кожного PR |
| NFR-12 | Security scanning | SonarQube + Trivy у CI; quality gate для критичних знахідок |

## Календарний план
| № | Етап | Термін | Статус |
|---:|---|---|---|
| 1 | Аналіз предметної області та аналогів | Жовтень – листопад 2026 | In Progress |
| 2 | Формалізація вимог і завершення Розділу 1 | Листопад 2026 | In Progress — очікує перевірки |
| 3 | Обґрунтування технологічного стеку | Листопад – грудень 2026 | To Do |
| 4 | Архітектура UML/C4 та моделі сховищ | Грудень 2026 – січень 2027 | To Do |
| 5 | Алгоритми ETL, графового аналізу та скорингу | Січень – лютий 2027 | To Do |
| 6 | Геопросторовий інтерфейс і Docker-розгортання | Березень 2027 | To Do |
| 7 | Черги, Polly, фонові задачі та метрики | Березень – квітень 2027 | To Do |
| 8 | Тестування, SonarQube і Trivy | Квітень 2027 | To Do |
| 9 | Апробація результатів і підготовка статті | Квітень 2027 | To Do |
| 10 | Нормоконтроль і передзахист | Травень 2027 | To Do |

## Git workflow для ЛР №4
Рекомендована гілка: `feature/chapter1-final`

Conventional Commit:
```text
docs(analysis): complete chapter 1 and formalize system requirements
```

Pull Request: `feature/chapter1-final` → `main`. Не виконувати merge до рев'ю викладачем.

https://docs.google.com/document/d/1XuWYPIQpE_dr5CCK6trEL-wadu3sW4r6c5WPZWHmYSY/edit?usp=sharing