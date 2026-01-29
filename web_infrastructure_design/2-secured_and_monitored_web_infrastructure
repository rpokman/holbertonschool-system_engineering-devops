# 2. Secured and Monitored Web Infrastructure

## Infrastructure Specifics
- **Firewalls**: Restricted access to ports.
  - Only allow port 80 (HTTP), 443 (HTTPS), and 22 (SSH) from specific sources.
  - Protects against unauthorized access and DoS.
- **SSL Certificate (HTTPS)**:
  - Encrypts traffic between the user and the server (Load Balancer/Web Server).
  - Ensures data integrity and privacy.
- **Monitoring**:
  - Tools (e.g., Datadog, Prometheus) collect metrics (CPU, RAM, QPS, 5xx errors).
  - Alerts engineers when thresholds are breached.

## Issues with this Infrastructure
- **SPOF**: The Load Balancer is still a single point of failure.
- **Terminating SSL**: If SSL is terminated at the LB, traffic between LB and Web Servers might be unencrypted (depending on configuration), or if terminated at Web Servers, CPU load increases there.

## Diagram
```mermaid
graph TD
    User((User)) -->|HTTPS/443| DNS[DNS Service]
    DNS --> LB[Load Balancer\nHAProxy\n+ Firewall]
    
    subgraph Monitoring
        Sumo[Sumo Logic/Datadog]
    end

    subgraph Server 1
        FW1[Firewall]
        Web1[Nginx + SSL]
        App1[App Server]
    end
    
    subgraph Server 2
        FW2[Firewall]
        Web2[Nginx + SSL]
        App2[App Server]
    end
    
    subgraph Data Tier
        DB_Master[(MySQL Primary)]
        DB_Replica[(MySQL Replica)]
    end

    LB -->|Encrypted Traffic| FW1
    LB -->|Encrypted Traffic| FW2
    
    FW1 --> Web1
    FW2 --> Web2
    Web1 --> App1
    Web2 --> App2
    
    App1 --> DB_Master
    App1 --> DB_Replica
    App2 --> DB_Master
    App2 --> DB_Replica

    Sumo -.->|Metrics| LB
    Sumo -.->|Metrics| Web1
    Sumo -.->|Metrics| Web2
```