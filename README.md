# 📊 Containerized Infrastructure Monitoring & Alerting Stack

A production-grade telemetry monitoring and automated incident response pipeline built using **Prometheus**, **Alertmanager**, **Grafana**, **Node Exporters**, and **Telegram Bot Integration**, orchestrated seamlessly with **Docker Compose**.

This stack provides real-time metric collection from both Linux and Windows host environments, time-series visualization, and low-latency incident dispatching with custom HTML formatting.

---

## 🏗️ Visual Architecture Diagram


┌─────────────────────────────────────────────────────────────────────────────┐
│                       MONITORED INFRASTRUCTURE (EXPORTERS)                  │
│                                                                             │
│  ┌──────────────────────┐   ┌──────────────────────┐   ┌─────────────────┐  │
│  │    Node Exporter     │   │   Windows Exporter   │   │ Prometheus Self │  │
│  │ (Linux Host Metrics) │   │  (Win Host Metrics)  │   │    Monitoring   │  │
│  │     Port: 9100       │   │     Port: 9182       │   │   Port: 9090    │  │
│  └──────────┬───────────┘   └──────────┬───────────┘   └────────┬────────┘  │
└─────────────┼──────────────────────────┼────────────────────────┼───────────┘
              │                          │                        │            
              └─────────────────┬────────┴────────────────────────┘            
                                │  Scrapes Metrics (HTTP / 15s)                
                                ▼                                              
┌─────────────────────────────────────────────────────────────────────────────┐
│                         MONITORING CORE (DOCKER)                            │
│                                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                           PROMETHEUS SERVER                           │  │
│  │                 (Time Series Storage & Rule Engine)                   │  │
│  └──────────────────┬─────────────────────────────────┬──────────────────┘  │
│                     │                                 │                     │
│    Evaluates Alert  │                                 │ Data Source         │
│    Rules Every 15s  │                                 │ Queries             │
│                     ▼                                 ▼                     │
│  ┌──────────────────────────────────────┐   ┌────────────────────────────┐  │
│  │            ALERTMANAGER              │   │          GRAFANA           │  │
│  │  (Grouping, Wait Timers & Routing)   │   │  (Dashboards & Visuals)    │  │
│  └──────────────────┬───────────────────┘   └────────────────────────────┘  │
└─────────────────────┼───────────────────────────────────────────────────────┘
                      │                                                        
                      │ Sends Formatted HTML Alerts                            
                      ▼                                                        
┌─────────────────────────────────────────────────────────────────────────────┐
│                          NOTIFICATION CHANNEL                               │
│                                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                          TELEGRAM BOT API                             │  │
│  │               (Instant FIRING & RESOLVED Messages)                    │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘

⚡ Features & Optimizations

Cross-Platform Telemetry: Scrapes metrics from Linux hosts (:9100), Windows hosts (:9182), and self-monitored metrics (:9090).

Low-Latency Dispatching: Optimized group_wait (5s) and evaluation_interval (15s) settings ensure sub-45-second alerting during downtime events.

Robust Telegram Templating: Uses custom HTML parsing mode to cleanly handle system underscores and avoid Telegram API 400 Bad Request exceptions.

Isolated Data Persistence: Docker named volumes ensure dashboards, metric history, and configuration states survive service restarts.


