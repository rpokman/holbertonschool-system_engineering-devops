# 3. Scale up

## Infrastructure Specifics
- **Load Balancer Cluster (HAProxy)**: 
  - Added a second Load Balancer to create a High Availability (HA) cluster.
  - This eliminates the Load Balancer as a Single Point of Failure (SPOF).
  - Configured in Active-Passive mode (or Active-Active). If the active LB fails, the passive one takes over immediately using tools like Keepalived.

- **Split Web Server and Application Server**:
  - **Web Server (Nginx/Apache)**: Responsible for handling HTTP/HTTPS requests and serving static content (HTML, CSS, JS, Images). It forwards dynamic requests to the Application Server.
  - **Application Server (Python/Node/Ruby)**: Responsible for executing the business logic and generating dynamic content.
  - **Why split them?**:
    - **Independent Scaling**: You can scale the web tier (handling connections) independently from the app tier (processing logic).
    - **Performance**: Web servers are optimized for I/O and concurrency. App servers are optimized for CPU and memory processing.
    - **Security**: The web server can act as a reverse proxy and firewall, shielding the application server from direct internet traffic.

- **Database**:
  - Separate server for the database as in previous steps (Primary/Replica setup recommended for redundancy).

## Diagram
```mermaid
graph TD
    User((User)) -->|HTTPS| DNS
    DNS --> VIP[Virtual IP]
    
    subgraph LB Cluster
        LB1[HAProxy Active]
        LB2[HAProxy Passive]
        VIP -.-> LB1
        VIP -.-> LB2
    end
    
    subgraph Web Tier
        Web1[Nginx 1]
        Web2[Nginx 2]
    end
    
    subgraph App Tier
        App1[App Server 1]
        App2[App Server 2]
    end
    
    subgraph Data Tier
        DB1[(MySQL Primary)]
        DB2[(MySQL Replica)]
        DB1 -.->|Replication| DB2
    end

    LB1 --> Web1
    LB1 --> Web2
    LB2 --> Web1
    LB2 --> Web2
    
    Web1 --> App1
    Web1 --> App2
    Web2 --> App1
    Web2 --> App2
    
    App1 --> DB1
    App1 --> DB2
    App2 --> DB1
    App2 --> DB2

    subgraph Monitoring
        Mon[Monitoring Service]
    end
    
    Mon -.-> LB1
    Mon -.-> LB2
    Mon -.-> Web1
    Mon -.-> Web2
    Mon -.-> App1
    Mon -.-> App2
    Mon -.-> DB1
    Mon -.-> DB2
```