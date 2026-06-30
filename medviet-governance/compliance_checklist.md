# NĐ13/2023 Compliance Checklist — MedViet AI Platform

## A. Data Localization
- [ ] Tất cả patient data lưu trên servers đặt tại Việt Nam
- [ ] Backup cũng phải ở trong lãnh thổ VN
- [ ] Log việc transfer data ra ngoài nếu có

## B. Explicit Consent
- [ ] Thu thập consent trước khi dùng data cho AI training
- [ ] Có mechanism để user rút consent (Right to Erasure)
- [ ] Lưu consent record với timestamp

## C. Breach Notification (72h)
- [ ] Có incident response plan
- [ ] Alert tự động khi phát hiện breach
- [ ] Quy trình báo cáo đến cơ quan có thẩm quyền trong 72h

## D. DPO Appointment
- [ ] Đã bổ nhiệm Data Protection Officer
- [ ] DPO có thể liên hệ tại: ___

## E. Technical Controls (mapping từ requirements)
| NĐ13 Requirement | Technical Control | Status | Owner |
|-----------------|-------------------|--------|-------|
| Data minimization | PII anonymization pipeline (Presidio) | ✅ Done | AI Team |
| Access control | RBAC (Casbin) + ABAC (OPA) | ✅ Done | Platform Team |
| Encryption | AES-256 at rest, TLS 1.3 in transit | 🚧 In Progress | Infra Team |
| Audit logging | CloudTrail + API access logs | ⬜ Todo | Platform Team |
| Breach detection | Anomaly monitoring (Prometheus) | ⬜ Todo | Security Team |

## F. Technical Solutions cho các mục "⬜ Todo"

### Audit Logging
- **Solution:** Implement structured logging using Python's `logging` module with JSON formatter. Ghi log tất cả API requests (user, action, resource, timestamp, IP) vào file riêng biệt. Dùng ELK stack (Elasticsearch, Logstash, Kibana) hoặc AWS CloudTrail để aggregate log. Set log retention policy tối thiểu 12 tháng theo NĐ13.

### Breach Detection
- **Solution:** Thiết lập Prometheus alerts cho các metrics: (1) Số lượng 403 errors đột biến → potential brute force, (2) Response time bất thường → potential data exfiltration, (3) Số lượng truy cập raw PII ngoài giờ làm việc. Grafana dashboard cho security team. Webhook alert vào Slack/Email cho DPO trong 72h.
