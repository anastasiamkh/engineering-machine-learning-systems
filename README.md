# Engineering Machine Learning Systems
Structured notes and resources on designing and implementing scalable and fault-tolerant machine learning systems, including system design, MLOps, and case studies.

Machine learning engineers work at the intersection of several professional fields, including software/backend engineering, DevOps, and data engineering. To build high-quality AI/ML systems, one must deepen their understanding of **software** (covered in [System Design](#1-system-design-generic--applications-in-ml-systems)), **data** ([Chapter 2 Data Infrastructure](#2-data-infrastructure)), and **DevOps** ([Chapter 6 - Observability and Monitoring](#6-observability-and-monitoring) and [Chapter 7- DevOps Practices](#7-devops-practices-in-ml)). engineering practices. These chapters cover topics relevant to a broad range of engineering professionals, not only those working in AI/ML.

Beyond these, ML engineers must also recognize what makes ML systems unique compared to traditional software. This includes understanding **typical ML workflows** and how to automate them ([Chapter 3 Feature Engineering](#3-feature-engineering), [Chapter 4 Model Training and Experimentation](#4-model-training-and-development), and [Chapter 5 Model Deployment and Inference](#5-model-deployment)). 

This repository does not discuss selecting optimal ML algorithms for a specific business problem. Instead, the focus is on **transforming existing data and ML research solutions designed by data scientists into scalable, well-engineered production services**.

## Table of Contents
## 1. System Design
Understanding system design principles is essential for building scalable and reliable software systems, including ML applications where additional considerations like caching predictions or scaling model inference apply.
- [**System Requirements & Trade-Offs**](#11-system-requirements): What is availability, fault tolerance, scalability. ML-specific requirements.
- [**Networking**](#12-networking): Basics of DNS, HTTP/HTTPS, and load balancing; considerations for model serving over APIs.
- [**APIs**](#13-apis): REST vs. gRPC for ML model serving; handling high-concurrency inference requests.
- [**Proxies and Load Balancing**](#14-proxies--load-balancing): Ensuring high availability and low latency for inference services.
- [**Caching**](#15-caching): Key-value stores for caching predictions, embeddings, or frequently used features.
- [**Storage**](#16-storage): SQL, NoSQL, and data lakes; storing features, metadata, and model outputs.
- [**Microservices**](#17-microservices): Best practices for decoupling ML services; microservices vs. monolith in ML systems.
- [**Event-Driven Architectures**](#18-event-driven-architectures): Using message queues (Kafka, RabbitMQ) for real-time feature pipelines and asynchronous inference.
- [**Distributed Systems**](#19-distributed-systems): CAP theorem, consistency models, and their relevance in ML pipelines.
- **Best Practices**
- **Challenges and Trade-Offs**

---

## 2. Data Infrastructure
A solid data infrastructure supports ML workflows at all stages, by enabling efficient data ingestion, processing, and storage for both training and inference.
- **Data Pipelines**: Designing ETL and ELT workflows for ML datasets.
- **Batch vs. Stream Processing**: Tools and trade-offs for ML data ingestion.
- **Data Storage**: Choosing between OLAP, OLTP, data lakes, and feature stores for ML.
- **Data Governance**: Ensuring data quality, lineage, and compliance for ML.
- **Data Versioning**: Tools and best practices to manage datasets and features over time.
- **Best Practices**
- **Challenges and Trade-Offs**

---

## 3. Feature Engineering
Automating feature extraction and management, which includes building batch and real-time pipelines, as well as feature store for data reuse.
- **Feature Stores**: How they enable feature reuse and consistency across training and inference.
- **Real-Time vs. Offline Features**: Use cases and challenges in ML systems.
- **Scaling Data Processing**: Optimizing resources and infrastructure for feature extraction.
- **Best Practices**
- **Challenges and Trade-Offs**

---

## 4. Model Training and Experimentation
Covers creating efficient workflows for managing experiments and scaling training to large datasets.
- **Distributed Training**: Frameworks for scaling training across multiple nodes (e.g., PyTorch DDP, Horovod).
- **Experiment Tracking**: Tools and workflows for reproducibility.
- **Model Explainability**: Understanding and visualizing model predictions.
- **Hyperparameter Tuning**: Automated tools (e.g., Ray Tune, Optuna) and best practices.
- **Best Practices**
- **Challenges and Trade-Offs**

---

## 5. Model Deployment
Turning trained models into production-ready services depending on end user needs.
- **Rollout Strategies**: A/B testing, canary deployments, shadow traffic.
- **Inference Design**: Real-time APIs, batch inference, and hybrid setups.
- **Model Packaging**: Using containers (Docker) and orchestration (Kubernetes).
- **Scaling and Optimization**: Balancing latency and model performance.
- **Best Practices**
- **Challenges and Trade-Offs**

---

## 6. Observability and Monitoring
Setting up monitoring systems to track metrics like latency, accuracy, and resource usage in real time.
- **Metrics**: Understanding what to track in production ML systems, from ML-based KPIs to latency and error rates.
- **ML-based**
  - **Metrics**: throughput, training time, model performance, data drift
  - **Data Drift Monitoring**: Tracking data and concept drift to avoid silent failures.
  - **Model Performance**: Logging metrics for accuracy, latency, and resource utilization.
  - **Model Explainability**: Understanding and visualizing model predictions.
- **Service-based**
  - **Metrics**: latency, resource utilization, error rates and costs.
  - **Logs and Traces**: Creating a trail to debugging failed ML pipelines.
  - **Alerts**: Setting sensible thresholds and SLOs. Avoid too many alerts (noise).
- **Best Practices**
- **Challenges and Trade-Offs**

---

## 7. DevOps Practices in ML
Focuses on applying DevOps principles to ML systems, including automation, testing, and environment management.
- **CI/CD for ML**: Automating pipeline integration and deployment.
- **Infrastructure as Code**: Managing cloud resources with Terraform or CDK.
- **Containerization**: Docker and Kubernetes.
- **Orchestration**: Using tools like Kubeflow, MetaFlow etc to automate the entite ML Workflow
- **Best Practices**
- **Challenges and Trade-Offs**


# 1 System Design

### What is actually System Design? How do ML system differnt from other Software Systems?

System design is the process of defining the **architecture** and **components** of a software system to meet specific requirements. Common requirements include **scalability**, **fault-tolerance**, **performance**, and **availability**. In traditional software, performance often focuses on *latency*, whereas ML engineers must also consider *model performance* (e.g., accuracy, RMSE) alongside latency.

Designing any software application requires balancing these requirements while accounting for real-world constraints such as **cost**, **hardware limitations**, and **user experience**. A well-designed system ensures that all components work cohesively and efficiently under varying conditions.

**ML engineers** face additional complexities unique to **machine learning systems**, which involve managing both code and model artifacts. Some of the added challenges include:
- Processing **large volumes of data** while balancing inference latency, accuracy, and cost.  
- Ensuring **consistency between training and inference pipelines**.  
- Managing the **computational demands** of model training and serving, while addressing both latency and high availability (common software engineering concerns) as well as model performance (an ML-specific consideration).  

In the context of ML, system design involves making key decisions that shape the entire ML workflow:
- How do we architect a system to handle **real-time predictions** at scale?  
- How can we design **feature pipelines** to ensure **reproducibility** and **consistency** across environments?  
- What infrastructure is required to support **distributed training**?  

While we will explore *ML-specific system design questions* in later chapters, it is crucial to first understand the **foundations of system design**. Topics such as **networking**, **caching**, **load balancing**, and **messaging** form the backbone of all software systems, including those for ML applications.

## 1.1 System Requirements

Before designing a system, it’s critical to understand the **non-functional requirements** it needs to fulfill. These requirements define the goals of the system and influence the trade-offs made during design. The most common requirements include:

### **Scalability**
Scalability is the ability of a system to handle an increasing amount of work or accommodate growth in users, data, or requests. Systems can scale:
- **Vertically**: By upgrading hardware (e.g., adding more memory or CPU power to a single server).  
- **Horizontally**: By adding more machines to distribute the workload.

> **ML Systems** For ML systems, scalability often involves:
> - Handling growing datasets for training and inference.
> - Scaling model inference to serve requests in real-time.
> - Supporting distributed training for larger models.

---

### **Fault Tolerance**
Fault tolerance is the ability of a system to continue operating even when some of its components fail. This is achieved through redundancy and failover mechanisms.

> **ML Systems** fault tolerance is crucial to:
> - Ensure model serving remains available during infrastructure failures.
> - Handle partial failures in distributed training without halting the entire process.
> - Maintain data integrity in feature pipelines and storage.

---

### **Performance**
Performance measures how efficiently a system executes its tasks, often evaluated through:
- **Latency**: The time it takes to process a single request.
- **Throughput**: The number of requests a system can handle in a given time.

For ML systems, performance extends to:
- **Inference latency**: The time taken to generate predictions.
- **Training efficiency**: The speed/duration of model training, it's throughput
- **Model performance**: Metrics like accuracy, RMSE, or F1-score, which are unique to ML applications.

##### Designing for Latency
- **Load Balancing**: Distribute incoming traffic across multiple servers to minimize request processing time.  
- **Caching**: Use in-memory caches (e.g., Redis, Memcached) to store frequently accessed data or pre-computed results.  
- **Efficient Algorithms**: Optimize algorithms for lower computational complexity to reduce processing time.  
- **Edge Computing**: Process requests closer to the user to reduce network delays.  
- **Database Optimization**: Use indexing, query optimization, and partitioning to speed up database queries.  
- **Asynchronous Processing**: Defer non-critical tasks to background jobs to improve user-facing latency.  

##### Designing for Throughput
- **Horizontal Scaling**: Add more instances or servers to handle increased workloads.  
- **Batch Processing**: Aggregate multiple requests or data points into a single operation to reduce system overhead.  
- **Concurrency**: Optimize applications to process multiple requests in parallel using multi-threading, async programming, or worker pools.  
- **Sharding**: Divide data into smaller, manageable chunks distributed across multiple nodes.  
- **Efficient Protocols**: Use lightweight communication protocols (e.g., gRPC) for high-throughput systems.  

##### Designing for Model Performance
- **Model Quantization**: Reduce the size and precision of models (e.g., FP32 to INT8) to speed up inference without significant accuracy loss.  
- **Pruning and Distillation**: Simplify models by removing redundant parameters or using smaller distilled versions of large models.  
- **Feature Selection**: Remove unnecessary features to improve training and inference efficiency.  
- **Specialized Hardware**: Use GPUs, TPUs, or inference accelerators for faster model training and serving.  

---

### **Availability**
Availability is the degree to which a system is operational and accessible when needed. High availability systems aim for minimal downtime, even during failures or maintenance.

ML systems must maintain availability to:
- Serve predictions reliably in real-time applications.
- Prevent downtime in critical pipelines, such as fraud detection or recommendation engines.

#### Designing for Availability
- **Replication Across Regions**: Replicate services and data in multiple regions to avoid single points of failure.  
- **Load Balancing**: Redirect traffic to healthy nodes during failures.  
- **Zero-Downtime Deployments**: Use strategies like blue-green or canary deployments.  
- **Backup and Restore**: Regularly back up data and test recovery procedures.  
- **Distributed Systems**: Distribute the workload to reduce reliance on any single component.  
- **Quorum Mechanisms**: Use consensus protocols (e.g., Paxos, Raft) to ensure availability in distributed systems.  

---

### **Cost**
Cost constraints include infrastructure, operational, and development expenses. In ML systems, cost considerations include:
- The storage and processing of large datasets.
- Computational resources for model training and inference.
- Optimization of resource usage to balance performance and budget.

#### Designing for Cost Efficiency
- **Choose Optimal Resource Types**: Use reserved or spot instances for predictable workloads.  
- **Right-Sizing Resources**: Allocate resources based on actual workload requirements.  
- **Serverless Architectures**: Use pay-as-you-go models for infrequent workloads (e.g., AWS Lambda).  
- **Data Lifecycle Management**: Archive or delete outdated data to reduce storage costs.  
- **Auto-scaling**: Scale down resources during low-demand periods.  
- **Batch Processing**: Aggregate and process data in batches to reduce compute costs.  

---

### **Balancing Trade-offs**
System requirements often compete with one another, requiring careful trade-offs. For example:
- Increasing fault tolerance might add latency due to additional redundancy.
- Reducing latency might require higher costs to provision more powerful hardware.

An ML engineer must evaluate these trade-offs while considering the unique challenges of ML systems, ensuring that the design meets both **business goals** and **technical constraints**.


## 1.2 Networking


### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.3 APIs

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.4 Proxies & Load Balancing

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.5 Caching

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.6 Storage

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.7 Microservices

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.8 Event-Driven Architectures

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.9 Distributed Systems

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.10 Best Practices (Patterns)

### Best Practices (Patterns)
### Chanllenges and Trade-Offs

## 1.11 Chanllenges and Trade-Offs

