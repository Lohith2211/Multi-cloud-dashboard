# 🌐 Multi-Cloud Dashboard

Real-time monitoring dashboard for multi-cloud infrastructure with intelligent load balancing. Monitors AWS, Azure, and GCP cloud servers with Docker containerization, Prometheus metrics, and automatic failover.

![Dashboard Preview](https://img.shields.io/badge/Status-Active-success) ![Node.js](https://img.shields.io/badge/Node.js-18+-green) ![React](https://img.shields.io/badge/React-18-blue) ![Docker](https://img.shields.io/badge/Docker-Ready-blue)

## 🚀 Features

- **📊 Real-time Metrics**: Live monitoring from cloud servers and load balancer
- **⚖️ Intelligent Load Balancing**: Health-based routing with automatic failover
- **🐳 Docker Integration**: Full containerization of all services
- **🎨 Modern UI**: Built with React, TypeScript, and Tailwind CSS
- **📈 Interactive Charts**: Response time, traffic distribution, and request analytics
- **⚡ Auto-refresh**: Updates every 5 seconds
- **🔄 Multi-source Data**: Real load balancer metrics + Docker container stats
- **📊 Prometheus**: Time-series metrics collection and monitoring

## 📋 Prerequisites

- **Docker Desktop** (with Docker Compose)
- **Node.js** 18 or higher
- **npm** or **yarn**

## 🎯 Quick Start

### Option 1: Automated Setup (Recommended)

**Windows PowerShell:**
```powershell
.\start.ps1
```

**Windows Command Prompt:**
```cmd
start.bat
```

Then open another terminal:
```bash
cd frontend
npm install
npm run dev
```

### Option 2: Manual Setup

### Option 2: Manual Setup

**1. Start Cloud Infrastructure:**
```bash
docker-compose up -d
```

This starts:
- AWS Server (port 5001)
- Azure Server (port 5002)
- GCP Server (port 5003)
- Load Balancer (port 5000)
- Prometheus (port 9090)
- Backend API (port 3001)

**2. Start Frontend:**
```bash
cd frontend
npm install
npm run dev
```

**3. Access Dashboard:**
Open http://localhost:5173

## 🏗️ Architecture

```
Frontend (React)
      ↓
Backend API (Node.js)
      ↓
Load Balancer (Python/Flask)
      ↓
AWS / Azure / GCP Servers (Docker Containers)
```

## 🌐 Service URLs

- **Dashboard**: http://localhost:5173
- **Backend API**: http://localhost:3001/api/metrics
- **Load Balancer**: http://localhost:5000/metrics
- **Prometheus**: http://localhost:9090
- **AWS Server**: http://localhost:5001
- **Azure Server**: http://localhost:5002
- **GCP Server**: http://localhost:5003

## 📊 Generating Load

Test the system with load:
```powershell
# PowerShell
1..100 | ForEach-Object { Invoke-WebRequest -Uri http://localhost:5000 -UseBasicParsing }
```

## 🐳 Docker Commands

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Check status
docker-compose ps

# Stop all services
docker-compose down

# Restart a specific service
docker-compose restart aws-server
```

## 📁 Project Structure

```
Multi-Cloud-Dashboard/
├── backend/              # Node.js/Express API
│   ├── server.js        # Main server with Docker & LB integration
│   ├── package.json     # Backend dependencies
│   ├── Dockerfile       # Backend container
│   └── README.md        # Backend documentation
├── frontend/            # React/TypeScript UI
│   ├── src/
│   │   ├── components/  # UI components
│   │   ├── hooks/       # Custom hooks (useMetrics)
│   │   └── pages/       # Main dashboard page
│   ├── package.json     # Frontend dependencies
│   └── .env            # API configuration
├── cloud-servers/       # Cloud infrastructure
│   ├── aws-server/      # AWS cloud server
│   ├── azure-server/    # Azure cloud server
│   ├── gcp-server/      # GCP cloud server
│   ├── load-balancer/   # Intelligent load balancer
│   └── monitoring/      # Prometheus config
├── docker-compose.yml   # Service orchestration
├── start.ps1           # PowerShell startup script
├── start.bat           # Batch startup script
├── SETUP.md            # Detailed setup guide
└── README.md           # This file
```

## 🔧 Configuration

### Backend Environment Variables
```bash
PORT=3001                                    # Backend API port
LOAD_BALANCER_URL=http://load-balancer:5000 # Load balancer endpoint (Docker)
# or
LOAD_BALANCER_URL=http://localhost:5000     # Load balancer endpoint (local)
```

### Frontend Configuration
Configure the API URL in `frontend/.env`:
```env
VITE_API_URL=http://localhost:3001
```

## 🌟 API Endpoints

### Backend API

**`GET /api/metrics`**
Returns complete dashboard metrics from load balancer + Docker containers
```json
{
  "servers": [...],
  "totalRequests": 1234,
  "totalFailures": 56,
  "avgResponseTime": 45.23,
  "activeCloud": "AWS",
  "responseTimeHistory": [...],
  "loadTest": {...},
  "source": "load-balancer"
}
```

**`GET /api/health`**
Health check and Docker connection status

### Load Balancer API

**`GET /`**
Routes request to best performing cloud server

**`GET /metrics`**
Returns JSON metrics for dashboard

**`GET /prometheus`**
Returns Prometheus-formatted metrics

**`GET /health`**
Load balancer health check

## 🎨 Tech Stack

### Frontend
- React 18
- TypeScript
- Vite
- Tailwind CSS
- Recharts
- Radix UI
- React Router

### Backend
- Node.js
- Express
- Dockerode (Docker API client)
- Axios (HTTP client)
- CORS

### Cloud Infrastructure
- Python 3.10
- Flask (Web framework)
- Flask-CORS
- Requests

### DevOps
- Docker & Docker Compose
- Prometheus (Metrics & Monitoring)

## 🧪 Testing

1. **Check Backend Health**:
   ```bash
   curl http://localhost:3001/api/health
   ```

2. **Check Load Balancer**:
   ```bash
   curl http://localhost:5000/metrics
   ```

3. **Check Metrics**:
   ```bash
   curl http://localhost:3001/api/metrics
   ```

4. **Test Cloud Servers**:
   ```bash
   curl http://localhost:5001  # AWS
   curl http://localhost:5002  # Azure
   curl http://localhost:5003  # GCP
   ```

5. **Open Dashboard**:
   Navigate to `http://localhost:5173` in your browser

## 🐛 Troubleshooting

### Docker services not starting
```bash
# Check Docker Desktop is running
docker info

# View detailed logs
docker-compose logs -f

# Restart services
docker-compose restart
```

### Backend can't connect to Load Balancer
- Check load balancer is running: `docker-compose ps`
- Verify LOAD_BALANCER_URL environment variable
- Check logs: `docker-compose logs load-balancer`

### Frontend shows no data
- Verify backend is running on port 3001
- Check browser console for CORS errors
- Ensure `.env` file exists: `VITE_API_URL=http://localhost:3001`

### Port already in use
```powershell
# Windows: Find what's using the port
netstat -ano | findstr :5000

# Kill the process (replace PID)
taskkill /PID <PID> /F
```

### Load Balancer can't reach cloud servers
- Ensure all containers are on same network
- Check Docker network: `docker network inspect multi-cloud-dashboard_cloud-network`
- Verify service names in docker-compose.yml

## 📚 Additional Resources

- **[SETUP.md](SETUP.md)** - Comprehensive setup and configuration guide
- **Prometheus**: http://localhost:9090 - Metrics visualization
- **Docker Compose Docs**: https://docs.docker.com/compose/

## 📝 License

MIT

## 🎯 What's New in v2.0

✨ **Real Cloud Infrastructure**: Actual cloud servers running in Docker containers  
⚖️ **Intelligent Load Balancing**: Health-based routing with automatic failover  
📊 **Prometheus Integration**: Time-series metrics collection  
🐳 **Full Docker Compose Setup**: One-command deployment  
🚀 **Auto-scaling Ready**: Easy to add more cloud server instances  
📈 **Real-time Metrics**: Genuine performance data from live services  

## 👨‍💻 Author

Created with ❤️ for multi-cloud monitoring and load balancing

---

**Enjoy monitoring your multi-cloud infrastructure!** 🎉
