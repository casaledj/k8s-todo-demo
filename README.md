# Kubernetes Todo API

A production-ready microservices todo application deployed on Google Kubernetes Engine (GKE), demonstrating modern container orchestration and cloud-native development practices.

## Architecture

```
Internet → Load Balancer → Node.js API (2 pods) → PostgreSQL + Redis
                                ↓
                          Kubernetes Services & ConfigMaps
```

### Components
- **API Gateway**: External LoadBalancer service
- **Todo API**: Node.js/Express REST API with 2 replicas
- **Database**: PostgreSQL for persistent storage
- **Cache**: Redis for future caching capabilities
- **Configuration**: ConfigMaps for application code
- **Orchestration**: GKE Autopilot cluster

## Technologies Used

- **Kubernetes**: Container orchestration (GKE Autopilot)
- **Node.js**: Backend API with Express framework
- **PostgreSQL**: Relational database for data persistence
- **Redis**: In-memory cache
- **Docker**: Container runtime
- **Google Cloud Platform**: Cloud infrastructure
- **YAML**: Infrastructure as Code

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check endpoint |
| GET | `/todos` | Retrieve all todos |
| POST | `/todos` | Create a new todo |
| DELETE | `/todos/:id` | Delete a specific todo |

### Example Usage

```bash
# Health check
curl http://YOUR-EXTERNAL-IP:3000/health

# Create a todo
curl -X POST http://YOUR-EXTERNAL-IP:3000/todos \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn Kubernetes", "description": "Built my first K8s app!"}'

# Get all todos
curl http://YOUR-EXTERNAL-IP:3000/todos
```

## Project Structure

```
k8s-todo-demo/
├── k8s/
│   ├── configmap.yaml      # Application code storage
│   ├── postgres.yaml       # PostgreSQL deployment & service
│   ├── redis.yaml          # Redis deployment & service
│   └── todo-api.yaml       # Node.js API deployment & service
├── src/
│   ├── app.js              # Main application code
│   └── package.json        # Node.js dependencies
└── README.md               # This file
```

## Deployment Instructions

### Prerequisites
- Google Cloud Platform account
- `gcloud` CLI installed and configured
- `kubectl` installed

### 1. Create GKE Cluster
```bash
gcloud container clusters create-auto todo-demo \
    --region=us-central1 \
    --release-channel=regular

gcloud container clusters get-credentials todo-demo --region=us-central1
```

### 2. Deploy Applications
```bash
# Deploy in order
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/postgres.yaml
kubectl apply -f k8s/redis.yaml
kubectl apply -f k8s/todo-api.yaml
```

### 3. Get External IP
```bash
kubectl get services
# Look for todo-api-service EXTERNAL-IP
```

### 4. Test the API
```bash
# Replace with your actual external IP
curl http://EXTERNAL-IP:3000/health
```

## Production Features

### Scalability
- **Horizontal Pod Autoscaling**: Automatically scales based on demand
- **Load Balancing**: Traffic distributed across multiple API pods
- **Resource Limits**: CPU and memory constraints for cost control

### Reliability
- **Self-Healing**: Automatic pod restart on failures
- **Health Checks**: Liveness and readiness probes (configurable)
- **Rolling Updates**: Zero-downtime deployments

### Security
- **Resource Quotas**: Prevent resource exhaustion
- **Network Policies**: Micro-segmentation between services
- **Secrets Management**: Secure credential storage

## Key Learning Outcomes

### Kubernetes Concepts Mastered
- Deployments and ReplicaSets
- Services and Load Balancing
- ConfigMaps and Secrets
- Resource Management
- Debugging and Troubleshooting

### DevOps Skills Demonstrated
- Infrastructure as Code
- Container Orchestration
- Microservices Architecture
- Production Debugging
- Cost Optimization

### Real-World Problem Solving
- **Read-only filesystem issues**: Solved ConfigMap mounting challenges
- **Memory optimization**: Tuned resource limits for npm install
- **Health check timing**: Adjusted probe timing for application startup
- **Service discovery**: Configured inter-service communication

## Debugging Experience

This project included real-world debugging scenarios:

1. **ConfigMap Read-Only Issues**: Learned about volume mounting and file permissions
2. **Out-of-Memory Errors**: Optimized resource allocation for Node.js applications
3. **Health Check Failures**: Understood Kubernetes probe timing and application lifecycle
4. **Service Networking**: Configured DNS-based service discovery

## Cost Optimization

- Used GKE Autopilot for serverless Kubernetes experience
- Implemented resource limits to control costs
- Chose efficient base images (Alpine Linux)
- Single-zone deployment for development/demo purposes

## Cleanup

```bash
# Delete all resources
kubectl delete -f k8s/

# Delete cluster
gcloud container clusters delete todo-demo --region=us-central1
```

## Next Steps

This project provides a solid foundation for understanding Kubernetes microservices architecture and can be extended with additional production features as needed.

## Contact

Built as a learning project to demonstrate Kubernetes and microservices expertise.

**Skills Demonstrated**: Kubernetes, Docker, Node.js, PostgreSQL, GCP, DevOps, Troubleshooting, Infrastructure as Code