# BÁO CÁO ĐỀ TÀI: TRIỂN KHAI WEBSITE WORDPRESS VÀ HỆ THỐNG GIÁM SÁT (OBSERVABILITY STACK) VỚI DOCKER

## 1. Thông tin sinh viên & Đề tài
- **Học viên / Sinh viên:** Trần Trung Hiếu
- **Mã số sinh viên:** DTC245180057
- **Đề tài:** Triển khai hạ tầng Web WordPress đa tầng kết hợp hệ thống giám sát thời gian thực (Prometheus, Loki, Promtail, Grafana) trên nền tảng Docker Container.

---

## 2. Sơ đồ kiến trúc hệ thống (System Architecture)

```mermaid
graph TD
    Client[Người dùng / Trình duyệt] -->|Port 80/443| Nginx[wp_nginx: Web Server & Reverse Proxy]
    Client -->|Port 8080| PMA[wp_phpmyadmin: Quản trị CSDL]
    Client -->|Port 3000| Grafana[wp_grafana: Dashboard trực quan]

    subgraph Frontend_Net [Mạng: mssvproject_frontend-net]
        Nginx --> WP[wp_app: WordPress Application]
    end

    subgraph Backend_Net [Mạng: mssvproject_backend-net]
        WP --> DB[(wp_mysql: Cơ sở dữ liệu MySQL)]
        PMA --> DB
    end

    subgraph Monitoring_Net [Mạng: mssvproject_monitoring-net]
        Promtail[wp_promtail: Agent gom logs] -->|Đẩy Logs| Loki[wp_loki: Lưu trữ & index log]
        Prometheus[wp_prometheus: Thu thập Metrics] -->|Lấy Metrics| Nginx
        Grafana -->|Query PromQL| Prometheus
        Grafana -->|Query LogQL| Loki
    end
