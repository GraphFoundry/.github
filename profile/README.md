# Optimizing Performance & Reliability in Microservice Architectures using Graph Theory and Network Science

A research project by fourth-year students at **Sri Lanka Institute of Information Technology (SLIIT)**, aimed at revolutionizing microservice reliability through graph-based analysis, predictive simulations, and intelligent resource scheduling.

---

## 👥 Team Members

| IT Number  | Name | Component |
|------------|------|-----------|
| **IT22917270** | Wimalagunasekara D.M.E. | [Service Graph Engine](#1-service-graph-engine) |
| **IT22036148** | Minsandi I.G.Y. | [Kubernetes Scheduler Extender](#4-kubernetes-scheduler-extender) |
| **IT22314574** | Samarathunge M.P.C. | [Graph Dependency Alert Engine](#5-graph-dependency-alert-engine) |
| **IT22346872** | Sarathchandra I.T.P. | [Predictive Analysis Engine](#3-predictive-analysis-engine) |

---

## 🎓 Academic Context

**Institution:** Sri Lanka Institute of Information Technology (SLIIT)  
**Program:** B.Sc. (Hons) in Information Technology, Specializing in Software Engineering  
**Year:** 4th Year, Semester 2  
**Project Type:** Research Project  

**Supervisor:** Dr. Dharshana Kasthurirathna  
**Co-Supervisor:** Mr. Vishan Jayasinghearachchi

---

## 📊 System Overview

GraphFoundry is a **cloud-native observability and prediction platform** designed to help DevOps teams understand, simulate, and optimize their microservice architectures. The platform combines real-time service topology mapping, predictive failure analysis, intelligent scheduling, and proactive alerting to minimize downtime and improve system reliability.

---

## 🧩 Core Components

| # | Component | Tech Stack | Repository | Description |
|---|-----------|------------|------------|-------------|
| **1** | **Service Graph Engine** | Node.js, Neo4j, Graph Data Science | [🔗 Repo](https://github.com/GraphFoundry/service-graph-engine) | Real-time service topology mapping and centrality analysis |
| **2** | **Predictive Analysis Engine** | Node.js, Express | [🔗 Repo](https://github.com/GraphFoundry/analysis-engine) | Simulates failure & scaling scenarios before production changes |
| **3** | **Graph Dependency Alert Engine** | Go, Hexagonal Architecture | [🔗 Repo](https://github.com/GraphFoundry/graph-dependency-alert-engine) | Proactive alerting based on service criticality & anomaly detection |
| **4** | **Kubernetes Scheduler Extender** | Go, Kubernetes API | [🔗 Repo](https://github.com/GraphFoundry/kubernetes-scheduler-extender) | Graph-aware pod scheduling for optimal placement |
| **5** | **Microservice Test Bed** | Go, Node.js, Python, Java, C# | [🔗 Repo](https://github.com/GraphFoundry/microservice-test-bed) | 11-service e-commerce demo app (based on Google's Online Boutique) |
| **6** | **Integrated Dashboard UI** | React, TypeScript, Vite, Tailwind | [🔗 Repo](https://github.com/GraphFoundry/integrated-dashboard) | Unified web interface for simulations and monitoring |

---

## 🔍 Component Details

### 1. Service Graph Engine

**Maintainer:** Wimalagunasekara D.M.E. (IT22917270)  
**Repository:** [github.com/GraphFoundry/service-graph-engine](https://github.com/GraphFoundry/service-graph-engine)

#### Overview
The **Service Graph Engine** is the **single source of truth** for microservice topology and metrics. It continuously ingests telemetry from Istio service mesh (via Prometheus) and constructs a living graph of service dependencies stored in Neo4j. Leveraging Neo4j's **Graph Data Science (GDS)** library, it computes centrality metrics (PageRank, Betweenness) to identify critical services.

#### Key Capabilities
- **Real-time Graph Synchronization**: Polls Prometheus every 10s for `istio_requests_total` and `istio_request_duration_milliseconds`
- **Centrality Scoring**: Calculates PageRank and Betweenness Centrality every 30s to rank service criticality
- **HTTP API**: Provides RESTful endpoints for graph queries, metrics snapshots, and centrality scores
- **Infrastructure Awareness**: Tracks Kubernetes pod-to-service mappings for resource-level analysis

---

### 2. Microservice Test Bed

**Repository:** [github.com/GraphFoundry/microservice-test-bed](https://github.com/GraphFoundry/microservice-test-bed)

#### Overview
A **production-like microservice environment** based on Google's **Online Boutique** demo. Consists of 11 polyglot services communicating via gRPC, deployed on Kubernetes with Istio service mesh. Used as a live testing ground for all GraphFoundry components.

---

### 3. Predictive Analysis Engine

**Maintainer:** Sarathchandra I.T.P. (IT22346872)  
**Repository:** [github.com/GraphFoundry/analysis-engine](https://github.com/GraphFoundry/analysis-engine)

#### Overview
The **Predictive Analysis Engine** is a microservice observability tool that performs predictive impact analysis on service call graphs. It enables operators to simulate infrastructure changes—service failures and scaling operations—before executing them in production, thereby reducing risk and improving operational decision-making. It consumes graph data from the **Service Graph Engine** and runs **Breadth-First Search (BFS)** to trace dependency chains.

#### Key Capabilities
- **Failure Simulation**: Predict which services will be affected if a target service fails
- **Scaling Simulation**: Estimate load redistribution when adding/removing replicas
- **Impact Scoring**: Calculates blast radius (number of affected services, severity levels)
- **Path Tracing**: Identifies critical paths where traffic will be disrupted

#### Simulation Workflow
1. **Fetch Graph**: Retrieves live topology from Service Graph Engine
2. **Traverse Dependencies**: Uses BFS to find all downstream/upstream services
3. **Calculate Impact**: Scores based on centrality, traffic volume, and hop distance
4. **Generate Recommendations**: Suggests mitigation strategies (e.g., circuit breakers, retries)

---

### 4. Kubernetes Scheduler Extender

**Maintainer:** Minsandi I.G.Y. (IT22036148)  
**Repository:** [github.com/GraphFoundry/kubernetes-scheduler-extender](https://github.com/GraphFoundry/kubernetes-scheduler-extender)

#### Overview
A **custom Kubernetes scheduler extension** that makes pod placement decisions based on **service graph topology**. Unlike the default scheduler (which only considers resource availability), this extender factors in service dependencies to co-locate or anti-co-locate services for optimal performance and resilience.

#### Key Capabilities
- **Topology-Aware Scheduling**: Queries Service Graph Engine for service relationships before pod placement
- **Affinity Rules**: Co-locates frequently communicating services to reduce latency
- **Anti-Affinity Rules**: Spreads critical services across nodes for fault tolerance
- **Dynamic Scoring**: Assigns fitness scores to candidate nodes based on graph metrics

#### Scheduling Logic
1. Kubernetes scheduler sends node candidates to extender
2. Extender queries Service Graph Engine for service neighborhood
3. Scores nodes based on:
   - Co-location with upstream dependencies (reduce cross-node traffic)
   - Spread of high-centrality services (avoid single point of failure)
4. Returns top-ranked node to scheduler

---

### 5. Graph Dependency Alert Engine

**Maintainer:** Samarathunge M.P.C. (IT22314574)  
**Repository:** [github.com/GraphFoundry/graph-dependency-alert-engine](https://github.com/GraphFoundry/graph-dependency-alert-engine)

#### Overview
A **proactive alerting system** that monitors service health and topology changes to detect potential cascading failures before they happen. Unlike traditional metric-based alerts (CPU, memory), this engine uses **graph-based risk scoring** to predict when a service degradation might trigger wider outages.

#### Key Capabilities
- **Risk Scoring**: Combines centrality metrics, latency trends, and error rates into a unified risk score
- **Anomaly Detection**: Uses statistical forecasting (moving average, standard deviation) to detect abnormal patterns
- **Cascading Failure Prediction**: Simulates failure propagation through dependency graph
- **Webhook Notifications**: Sends alerts to Slack, PagerDuty, or custom endpoints with HMAC signatures
- **Event Bus Architecture**: Decouples detection, evaluation, and notification

---

### 6. Integrated Dashboard UI

**Repository:** [github.com/GraphFoundry/integrated-dashboard](https://github.com/GraphFoundry/integrated-dashboard)

#### Overview
Modern frontend dashboard for the Predictive Analysis Engine. Built with Vite, React, TypeScript, and Tailwind CSS. Provides a unified web interface for monitoring service health, running simulations, viewing metrics, analyzing history, and managing alerts.

---

## 🤝 Contributing

This is an academic research project for SLIIT Y4S2. For questions or collaboration inquiries, please contact:

- **Wimalagunasekara D.M.E.** (IT22917270) - Service Graph Engine
- **Minsandi I.G.Y.** (IT22036148) - Kubernetes Scheduler Extender
- **Samarathunge M.P.C.** (IT22314574) - Graph Dependency Alert Engine
- **Sarathchandra I.T.P.** (IT22346872) - Predictive Analysis Engine

---

## 🔗 Quick Links

| Resource | Link |
|----------|------|
| **Organization** | [github.com/GraphFoundry](https://github.com/GraphFoundry) |
| **Service Graph Engine** | [🔗 Repo](https://github.com/GraphFoundry/service-graph-engine) |
| **Predictive Analysis Engine** | [🔗 Repo](https://github.com/GraphFoundry/analysis-engine) |
| **Graph Alert Engine** | [🔗 Repo](https://github.com/GraphFoundry/graph-dependency-alert-engine) |
| **Scheduler Extender** | [🔗 Repo](https://github.com/GraphFoundry/kubernetes-scheduler-extender) |
| **Microservice Test Bed** | [🔗 Repo](https://github.com/GraphFoundry/microservice-test-bed) |
| **Dashboard UI** | [🔗 Repo](https://github.com/GraphFoundry/integrated-dashboard) |

---

<div align="center">

**Built by the GraphFoundry Team**

*Empowering DevOps through intelligent graph-based observability*

</div>
