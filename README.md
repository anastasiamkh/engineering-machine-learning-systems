# Engineering Machine Learning Systems
Structured notes and resources on designing and implementing scalable and fault-tolerant machine learning systems, including system design, MLOps, and case studies.

Machine learning engineers work at the intersection of several professional fields, including software/backend engineering, DevOps, and data engineering. To build high-quality AI/ML systems, one must deepen their understanding of **software** (covered in [System Design](#1-system-design-generic--applications-in-ml-systems)), **data** ([Chapter 2 Data Infrastructure](#2-data-infrastructure)), and **DevOps** ([Chapter 6 - Observability and Monitoring](#6-observability-and-monitoring) and [Chapter 7- DevOps Practices](#7-devops-practices-in-ml)). engineering practices. These chapters cover topics relevant to a broad range of engineering professionals, not only those working in AI/ML.

Beyond these, ML engineers must also recognize what makes ML systems unique compared to traditional software. This includes understanding **typical ML workflows** and how to automate them ([Chapter 3 Feature Engineering](#3-feature-engineering), [Chapter 4 Model Training and Experimentation](#4-model-training-and-development), and [Chapter 5 Model Deployment and Inference](#5-model-deployment)). 

This repository does not discuss selecting optimal ML algorithms for a specific business problem. Instead, the focus is on **transforming existing data and ML research solutions designed by data scientists into scalable, well-engineered production services**.


## 1. System Design
Understanding system design principles is essential for building scalable and reliable software systems, including ML applications where additional considerations like caching predictions or scaling model inference apply.
- **Introduction to System Requirements**: What is availability, fault tolerance, scalability. ML-specific requirements.
- **Networking**: Basics of DNS, HTTP/HTTPS, and load balancing; considerations for model serving over APIs.
- **APIs**: REST vs. gRPC for ML model serving; handling high-concurrency inference requests.
- **Proxies and Load Balancing**: Ensuring high availability and low latency for inference services.
- **Caching**: Key-value stores for caching predictions, embeddings, or frequently used features.
- **Microservices**: Best practices for decoupling ML services; microservices vs. monolith in ML systems.
- **Databases/Storage**: SQL, NoSQL, and data lakes; storing features, metadata, and model outputs.
- **Event-Driven Architectures**: Using message queues (Kafka, RabbitMQ) for real-time feature pipelines and asynchronous inference.
- **Distributed Systems**: CAP theorem, consistency models, and their relevance in ML pipelines.
- **Meeting System Requirements**: Designing for availability, fault tolerance, etc.
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
