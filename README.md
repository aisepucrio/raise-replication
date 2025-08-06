# RAISE: A Self-Hosted API for Efficient Data Mining from GitHub and Jira - Artifact

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains the **replication package** for the paper "_RAISE: A Self-Hosted API for Efficient Data Mining from GitHub and Jira_", including complete source code, example data, and detailed instructions for reproducing the experiments.

## 📄 Reference Paper

**Link to accepted paper**: [To be included when available]

## 📋 Artifact Overview

**RAISE** (Repository Analysis and Information System for Extraction) is a Django-based API designed for efficient mining and analysis of software development data from GitHub repositories and Jira projects. The system is designed for researchers and software engineers who need to extract, analyze, and visualize software development metrics.

## 📚 Documentation Structure

This repository contains comprehensive documentation organized by component:

- **[Main README.md](README.md)** (this file): Setup, installation, and configuration
- **[tool_src/README.md](tool_src/README.md)**: Complete API documentation, usage instructions, and troubleshooting
- **[web_src/README.md](web_src/README.md)**: Frontend usage guide, UI navigation, and web interface features

### Key Features

- **GitHub Mining**: Extraction of commits, pull requests, issues, branches, and repository metadata
- **Jira Mining**: Extraction of issues, comments, sprints, and activity history
- **Temporal Analysis**: Monitoring project evolution over time
- **Documented API**: RESTful endpoints documented with DRF Spectacular/OpenAPI
- **Web Interface**: Modern Next.js dashboard for data visualization
- **Asynchronous Processing**: Task system with Celery and Redis for long-running operations
- **Robust Storage**: PostgreSQL database with optimized data model

## 🏗️ Repository Organization

```
raise-replication/
├── tool_src/                    # RAISE API Backend (Django)
│   ├── dataminer_api/          # Main Django configuration
│   ├── github/                 # GitHub mining module
│   ├── jira/                   # Jira mining module
│   ├── jobs/                   # Task management system
│   ├── utils/                  # Shared utilities
│   ├── requirements.txt        # Python dependencies
│   ├── docker-compose.yaml     # Docker configuration
│   └── README.md              # Detailed backend instructions
├── web_src/                    # Web Frontend (Next.js)
│   ├── src/                   # React/TypeScript source code
│   ├── package.json           # Node.js dependencies
│   └── README.md             # Frontend instructions
└── README.md                 # This file (artifact overview)
```

## 🔧 System Requirements

### Required Components

- **Docker Desktop** ≥ 4.0
  - Windows: WSL 2 enabled
  - macOS: Intel or Apple Silicon
  - Linux: Docker Engine + Docker Compose
- **Git** ≥ 2.0
- **RAM**: Minimum 8GB (recommended 16GB for large datasets)
- **Storage**: Minimum 10GB free space
- **Connectivity**: Internet access for GitHub/Jira APIs

### Optional Components

- **Node.js** ≥ 18 (only if running frontend separately)
- **PostgreSQL Client** (TablePlus, pgAdmin) for direct data exploration

### Tested Platforms

- ✅ Windows 10/11 with Docker Desktop
- ✅ macOS Monterey+ with Docker Desktop
- ✅ Ubuntu 20.04+ with Docker Engine
- ✅ CentOS 8+ with Docker Engine

## 🚀 Installation and Configuration

### Step 1: Install Docker

#### Windows Installation

1. **Download Docker Desktop for Windows**
   - Go to [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
   - Download the installer for your Windows version
   - Run the installer as Administrator

2. **Enable WSL 2 (if prompted)**
   - Open PowerShell as Administrator
   - Run: `wsl --install`
   - Restart your computer when prompted

3. **Verify Installation**
   ```powershell
   docker --version
   docker-compose --version
   ```

#### macOS Installation

1. **Download Docker Desktop for Mac**
   - Go to [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/)
   - Download the appropriate version (Intel or Apple Silicon)
   - Drag Docker to Applications folder and open it

2. **Verify Installation**
   ```bash
   docker --version
   docker-compose --version
   ```

#### Linux Installation (Ubuntu)

```bash
# Update package index
sudo apt update

# Install Docker Engine
sudo apt install docker.io

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group
sudo usermod -aG docker $USER

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Logout and login again, then verify
docker --version
docker-compose --version
```

### Step 2: Install Git

#### Windows
1. Download [Git for Windows](https://git-scm.com/download/win)
2. Run the installer with default options
3. Verify: `git --version`

#### macOS
```bash
brew install git
git --version
```

#### Linux
```bash
sudo apt update
sudo apt install git
git --version
```

### Step 3: Install Database Client (Optional but Recommended)

#### TablePlus (Recommended)
1. Download from [tableplus.com](https://tableplus.com/)
2. Install and launch
3. Create new connection → PostgreSQL
4. Use connection details from your `.env` file

#### pgAdmin (Alternative)
1. Download from [pgadmin.org](https://www.pgadmin.org/download/)
2. Install and launch
3. Add new server with connection details

### Step 4: Clone and Configure

```bash
# Clone the repository
git clone https://github.com/aisepucrio/raise-replication.git
cd raise-replication

# Navigate to backend directory
cd tool_src
```

### Step 5: Generate API Tokens

#### GitHub Token Generation

1. **Access GitHub Settings**
   - Go to [GitHub Settings > Developer Settings > Personal Access Tokens](https://github.com/settings/tokens)
   - Click "Generate new token (classic)"

2. **Configure Token Permissions**
   - **Note**: "RAISE API Access"
   - **Expiration**: Choose appropriate duration (90 days recommended)
   - **Scopes**: Select the following:
     - ✅ `repo` (Full control of private repositories)
     - ✅ `read:org` (Read org and team membership)
     - ✅ `read:user` (Read user profile data)

3. **Generate and Save**
   - Click "Generate token"
   - **IMPORTANT**: Copy the token immediately - you won't see it again!
   - Save it securely for the next step

#### Jira Token Generation

1. **Access Jira API Tokens**
   - Go to [Jira API Tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
   - Click "Create API token"

2. **Configure Token**
   - **Label**: "RAISE Mining Tool"
   - Click "Create"

3. **Save Token**
   - Click "Copy to clipboard"
   - Save it securely for the next step

### Step 6: Configure Environment

Create a `.env` file in the `tool_src/` directory:

```bash
# Create the environment file
touch .env
```

Add the following content to your `.env` file:

```env
# ====== REQUIRED: API TOKENS ======
GITHUB_TOKENS="your_github_token_here"
JIRA_API_TOKEN="your_jira_token_here"
JIRA_EMAIL="your_jira_email@company.com"

# ====== REQUIRED: DATABASE CONFIGURATION ======
POSTGRES_DB=your_db_name
POSTGRES_USER=your_db_username
POSTGRES_PASSWORD=your_secure_password_here
POSTGRES_HOST=db
POSTGRES_PORT=5432

# ====== REQUIRED: DJANGO CONFIGURATION ======
DJANGO_SUPERUSER_USERNAME=your_username
DJANGO_SUPERUSER_PASSWORD=your_admin_password_here
DJANGO_SUPERUSER_EMAIL=your_email@example.com
DJANGO_SECRET_KEY=your_very_long_random_django_secret_key_here

# ====== OPTIONAL: ADVANCED SETTINGS ======
IS_DEBUG=True
```

**Important Notes:**
- Replace all placeholder values with your actual tokens and passwords
- Use strong passwords for database and admin accounts
- The Django secret key should be a long random string (you can generate one online)

### Step 7: Start the System

```bash
# Make sure you're in the tool_src directory
cd tool_src

# Start all services
docker-compose up --build
```

**Wait for the following messages:**
```
web_1    | Starting development server at http://0.0.0.0:8000/
worker_1 | [2024-XX-XX XX:XX:XX,XXX: INFO/MainProcess] Connected to redis://redis:6379//
```

### Step 8: Verify Installation

1. **Test API Access**
   - Open your browser and go to: `http://localhost:8000/api/`
   - You should see the Django REST Framework interface

2. **Test Basic Endpoint**
   ```bash
   curl -X GET "http://localhost:8000/api/github/commits/" -H "accept: application/json"
   ```
   **Expected response**: `{"count": 0, "next": null, "previous": null, "results": []}`

## 📖 API Reference and Usage Instructions

> **📚 Complete API Documentation**: For detailed API usage instructions, request/response structures, endpoint examples, and troubleshooting tips, see [tool_src/README.md](tool_src/README.md).

The RAISE API provides comprehensive endpoints for mining and querying data from GitHub repositories and Jira projects. All endpoints return JSON responses and support pagination.

### Available Endpoints Overview

#### GitHub Data Mining
- **Commit Collection**: Extract commits with code metrics and metadata
- **Issue Collection**: Collect issues and pull requests with comments
- **Branch Collection**: Gather repository branch information
- **Metadata Collection**: Extract repository statistics (stars, forks, languages)
- **Bulk Collection**: Mine multiple repositories and data types simultaneously

#### GitHub Data Query
- **Commits**: Filter and search collected commits
- **Issues**: Query issues with advanced filtering
- **Pull Requests**: Retrieve and filter pull request data
- **Branches**: Access repository branch information
- **Metadata**: Get repository statistics and metrics
- **Dashboard**: Statistical summaries and analytics

#### Jira Data Mining
- **Issue Collection**: Extract issues from Jira projects with complete history

#### Jira Data Query
- **Issues**: Filter and search Jira issues
- **Dashboard**: Jira project statistics and summaries

#### Task Management
- **Task Monitoring**: View and manage mining operations
- **Status Tracking**: Monitor task progress and completion
- **Task Control**: Cancel or retry mining operations

#### Data Export
- **JSON Export**: Download collected data in structured format
- **Filtered Export**: Export specific data subsets

### Web Interface

> **🎨 Complete Web Interface Guide**: For detailed instructions on using the frontend, navigating the UI, and accessing features, see [web_src/README.md](web_src/README.md).

The RAISE system includes a modern web dashboard built with Next.js that provides a graphical interface for data mining and visualization.

#### Quick Start

```bash
# Navigate to frontend directory
cd web_src/

# Install dependencies
npm install

# Create environment file
echo 'NEXT_PUBLIC_API_URL="http://localhost:8000"' > .env

# Start development server
npm run dev
```

#### Access the Dashboard

Open your browser and navigate to `http://localhost:3000`

**Main Features:**
- **Dashboard Overview**: Mining statistics and summaries
- **Data Collection**: Repository and project mining interface
- **Task Management**: Monitor and manage mining operations
- **Data Visualization**: Charts and analytics for mined data
- **Data Preview**: Explore and export collected data

**Quick Navigation:**
- **Dashboard**: Overview and statistics
- **Collect**: Start new mining operations
- **Jobs**: Monitor and manage mining tasks
- **Preview**: View and explore collected data

## 📊 Data Structure

The system stores data in PostgreSQL with the following model:

- **`github_commit`**: Commits with code metrics (insertions, deletions, complexity)
- **`github_issue_pull_request`**: Unified issues and PRs with comments and timeline
- **`github_metadata`**: Repository metadata (stars, forks, languages)
- **`jira_issue`**: Jira issues with complete history and relationships
- **`jobs_task`**: Mining task execution history

### Database Connection

Use these credentials (from your `.env` file):

```
Host: localhost
Port: 5432
Database: your_db_name
Username: your_db_username
Password: [as configured in your .env file]
```

## 📋 Storage Requirements

- **Base system**: ~2GB (Docker containers)
- **Data per large repository**: ~500MB-2GB
- **Logs and cache**: ~100MB-500MB
- **Total recommended**: 10GB+ free space

## ⚖️ Legal and Ethical Considerations

### License

This project is distributed under the **MIT License**. See `LICENSE` for complete details.

### Responsible Use

- Respect API rate limits (GitHub: 5000 req/hour, Jira: variable)
- Configure multiple tokens for intensive use
- Only mine repositories/projects you have permission to access
- Private data requires tokens with specific permissions

### GDPR/LGPD Compliance

- The system does not collect personal data beyond what is publicly available
- User data (names, emails) is collected as available through APIs
- For full compliance, implement appropriate retention policies


## 🏆 Citation

If you use this artifact in your research, please cite:

```bibtex
@inproceedings{raise2024,
  title={RAISE: A Self-Hosted API for Efficient Data Mining from GitHub and Jira},
  author={[Authors to be included]},
  booktitle={[Conference to be included]},
  year={2024}
}
```

---