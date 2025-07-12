# 🔌 Port Reference for DevOps Engineers

This document provides a categorized list of **important application ports** that are essential for DevOps engineers to know. These are commonly used in web development, CI/CD pipelines, monitoring, container orchestration, cloud infrastructure, and various programming languages.

---

## ✅ General Web & HTTP(S)
| Port | Service / Tool                        |
|------|----------------------------------------|
| 80   | HTTP (Web Servers like Apache, NGINX) |
| 443  | HTTPS                                 |
| 8080 | Alternate HTTP (Spring Boot, Tomcat)  |
| 8443 | Alternate HTTPS                       |

---

## 🐳 Docker / Containers
| Port | Service / Tool           |
|------|--------------------------|
| 2375 | Docker (non-TLS)         |
| 2376 | Docker (TLS)             |
| 5000 | Private Docker Registry  |

---

## ☸️ Kubernetes
| Port      | Service / Tool        |
|-----------|------------------------|
| 6443      | Kubernetes API Server  |
| 10250     | Kubelet                |
| 10255     | Read-only Kubelet      |
| 30000–32767 | NodePort Services    |

---

## 🧪 DevOps CI/CD Tools
| Port | Tool              |
|------|------------------|
| 8080 | Jenkins (default) |
| 7990 | Bitbucket        |
| 9000 | SonarQube        |
| 8081 | Nexus Repository |
| 3000 | Grafana          |
| 9090 | Prometheus       |
| 9100 | Node Exporter    |
| 9115 | Blackbox Exporter|
| 9323 | Docker Container Exporter |

---

## ☁️ Cloud Services & Monitoring
| Port | Tool/Service           |
|------|------------------------|
| 22   | SSH (Linux/Cloud instances) |
| 3389 | RDP (Windows servers)  |
| 514  | Syslog                 |

---

## 🐘 Databases
| Port  | Database              |
|-------|------------------------|
| 3306  | MySQL/MariaDB         |
| 5432  | PostgreSQL            |
| 1433  | Microsoft SQL Server  |
| 1521  | Oracle DB             |
| 27017 | MongoDB               |
| 6379  | Redis                 |
| 9200  | Elasticsearch (HTTP)  |
| 9300  | Elasticsearch Cluster |

---

## 🧑‍💻 Programming Languages & Frameworks
| Port | Framework / Language            |
|------|---------------------------------|
| 3000 | Node.js, React (default dev server) |
| 5000 | Flask (Python)                  |
| 8000 | Django (Python)                 |
| 4200 | Angular (default dev server)    |
| 8080 | Java (Spring Boot, Tomcat)      |
| 5001 | .NET Core (HTTPS)               |
| 8001 | FastAPI, uvicorn                |

---

## 🛡️ Security & Authentication
| Port | Service / Tool     |
|------|--------------------|
| 636  | LDAPS              |
| 389  | LDAP               |
| 1812 | RADIUS (Authentication) |
| 1645 | Alternate RADIUS   |
| 443  | OAuth, OpenID, SAML endpoints (via HTTPS) |

---

## 📘 Notes
- Some applications use different ports in production vs. development.
- Always ensure firewall and security group configurations match the required port exposure.
- It's a best practice to avoid exposing sensitive ports publicly (e.g., Jenkins, database ports).

---

**Maintained by:** DevOps Community  
**Last Updated:** July 2025


