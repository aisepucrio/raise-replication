# RAISE: A Self-Hosted API for Efficient Data Mining from GitHub and Jira - Artifact

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains the **replication package** for the paper "_RAISE: A Self-Hosted API for Efficient Data Mining from GitHub and Jira_", including complete source code, example data, and detailed instructions for reproducing the experiments.

## 📄 Reference Paper

**Link to accepted paper**: [To be included when available]

## 📋 Artifact Overview

**RAISE** (Repository Analysis and Information System for Extraction) is a Django-based API designed for efficient mining and analysis of software development data from GitHub repositories and Jira projects. The system is designed for researchers and software engineers who need to extract, analyze, and visualize software development metrics.

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

The RAISE API provides comprehensive endpoints for mining and querying data from GitHub repositories and Jira projects. All endpoints return JSON responses and support pagination.

### GitHub Data Mining Endpoints

#### 1. Commit Collection (`POST /api/github/commits/collect/`)

**Purpose**: Collect commits from a GitHub repository with optional date filtering.

**Parameters**:
- `repo_name` (required): Repository in format `owner/repo`
- `start_date` (optional): Start date in ISO 8601 format
- `end_date` (optional): End date in ISO 8601 format
- `commit_sha` (optional): Specific commit SHA to fetch

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/commits/collect/" \
     -H "Content-Type: application/json" \
     -d '{
       "repo_name": "octocat/Hello-World",
       "start_date": "2023-01-01T00:00:00Z",
       "end_date": "2023-06-30T23:59:59Z"
     }'
```

**Response**:
```json
{
  "task_id": "abc123-def456-ghi789",
  "message": "Task successfully initiated",
  "status_endpoint": "http://localhost:8000/api/jobs/tasks/abc123-def456-ghi789/"
}
```

#### 2. Issue Collection (`POST /api/github/issues/collect/`)

**Purpose**: Collect issues and pull requests from a GitHub repository.

**Parameters**:
- `repo_name` (required): Repository in format `owner/repo`
- `start_date` (optional): Start date in ISO 8601 format
- `end_date` (optional): End date in ISO 8601 format
- `depth` (optional): "basic" or "complex" (default: "basic")

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/issues/collect/" \
     -H "Content-Type: application/json" \
     -d '{
       "repo_name": "octocat/Hello-World",
       "start_date": "2023-01-01T00:00:00Z",
       "end_date": "2023-06-30T23:59:59Z",
       "depth": "basic"
     }'
```

#### 3. Pull Request Collection (`POST /api/github/pull-requests/collect/`)

**Purpose**: Collect pull requests from a GitHub repository.

**Parameters**:
- `repo_name` (required): Repository in format `owner/repo`
- `start_date` (optional): Start date in ISO 8601 format
- `end_date` (optional): End date in ISO 8601 format
- `depth` (optional): "basic" or "complex" (default: "basic")

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/pull-requests/collect/" \
     -H "Content-Type: application/json" \
     -d '{
       "repo_name": "octocat/Hello-World",
       "start_date": "2023-01-01T00:00:00Z",
       "end_date": "2023-06-30T23:59:59Z",
       "depth": "basic"
     }'
```

#### 4. Branch Collection (`POST /api/github/branches/collect/`)

**Purpose**: Collect branch information from a GitHub repository.

**Parameters**:
- `repo_name` (required): Repository in format `owner/repo`

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/branches/collect/" \
     -H "Content-Type: application/json" \
     -d '{
       "repo_name": "octocat/Hello-World"
     }'
```

#### 5. Metadata Collection (`POST /api/github/metadata/collect/`)

**Purpose**: Collect repository metadata (stars, forks, languages, etc.).

**Parameters**:
- `repo_name` (required): Repository in format `owner/repo`

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/metadata/collect/" \
     -H "Content-Type: application/json" \
     -d '{
       "repo_name": "octocat/Hello-World"
     }'
```

#### 6. Bulk Collection (`POST /api/github/collect-all/`)

**Purpose**: Collect multiple data types from multiple repositories in a single request.

**Parameters**:
- `repositories` (required): Array of repository names
- `collect_types` (required): Array of data types to collect
- `start_date` (optional): Start date in ISO 8601 format
- `end_date` (optional): End date in ISO 8601 format
- `depth` (optional): "basic" or "complex" (default: "basic")

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/collect-all/" \
     -H "Content-Type: application/json" \
     -d '{
       "repositories": ["octocat/Hello-World", "facebook/react"],
       "collect_types": ["commits", "issues", "pull_requests", "metadata"],
       "start_date": "2023-01-01T00:00:00Z",
       "end_date": "2023-06-30T23:59:59Z"
     }'
```

### GitHub Data Query Endpoints

#### 1. Query Commits (`GET /api/github/commits/`)

**Purpose**: Retrieve collected commits with filtering and pagination.

**Query Parameters**:
- `repository`: Filter by repository name
- `created_after`: Filter by creation date (ISO 8601)
- `created_before`: Filter by creation date (ISO 8601)
- `message`: Search in commit messages
- `author_name`: Filter by author name
- `sha`: Filter by specific commit SHA

**Example**:
```bash
curl "http://localhost:8000/api/github/commits/?repository=octocat/Hello-World&created_after=2023-01-01"
```

#### 2. Query Issues (`GET /api/github/issues/`)

**Purpose**: Retrieve collected issues with filtering and pagination.

**Query Parameters**:
- `repository`: Filter by repository name
- `created_after`: Filter by creation date
- `created_before`: Filter by creation date
- `updated_after`: Filter by update date
- `updated_before`: Filter by update date
- `title`: Search in issue titles
- `creator`: Filter by creator
- `state`: Filter by state (open, closed)
- `has_label`: Filter by label

**Example**:
```bash
curl "http://localhost:8000/api/github/issues/?repository=octocat/Hello-World&state=open"
```

#### 3. Query Pull Requests (`GET /api/github/pull-requests/`)

**Purpose**: Retrieve collected pull requests with filtering and pagination.

**Query Parameters**: Same as issues endpoint

**Example**:
```bash
curl "http://localhost:8000/api/github/pull-requests/?repository=octocat/Hello-World&state=merged"
```

#### 4. Query Branches (`GET /api/github/branches/`)

**Purpose**: Retrieve collected branch information.

**Query Parameters**:
- `repository`: Filter by repository name
- `name`: Filter by branch name

**Example**:
```bash
curl "http://localhost:8000/api/github/branches/?repository=octocat/Hello-World"
```

#### 5. Query Metadata (`GET /api/github/metadata/`)

**Purpose**: Retrieve repository metadata.

**Query Parameters**:
- `repository`: Filter by repository name

**Example**:
```bash
curl "http://localhost:8000/api/github/metadata/?repository=octocat/Hello-World"
```

#### 6. Dashboard Statistics (`GET /api/github/dashboard/`)

**Purpose**: Get statistical summaries of mined data.

**Query Parameters**:
- `repository_id`: Specific repository ID for detailed stats
- `start_date`: Filter from this date
- `end_date`: Filter up to this date

**Example**:
```bash
curl "http://localhost:8000/api/github/dashboard/?start_date=2023-01-01T00:00:00Z"
```

### Jira Data Mining Endpoints

#### 1. Issue Collection (`POST /api/jira/issues/collect/`)

**Purpose**: Collect issues from a Jira project.

**Parameters**:
- `jira_domain` (required): Jira domain (e.g., "company.atlassian.net")
- `project_key` (required): Project key (e.g., "PROJ")
- `issuetypes` (optional): Array of issue types to filter
- `start_date` (optional): Start date (YYYY-MM-DD format)
- `end_date` (optional): End date (YYYY-MM-DD format)

**Example**:
```bash
curl -X POST "http://localhost:8000/api/jira/issues/collect/" \
     -H "Content-Type: application/json" \
     -d '{
       "jira_domain": "ecosystem.atlassian.net",
       "project_key": "AO",
       "start_date": "2023-01-01",
       "end_date": "2023-06-30"
     }'
```

### Jira Data Query Endpoints

#### 1. Query Issues (`GET /api/jira/issues/`)

**Purpose**: Retrieve collected Jira issues with filtering.

**Query Parameters**:
- `created_after`: Filter by creation date
- `created_before`: Filter by creation date
- `updated_after`: Filter by update date
- `updated_before`: Filter by update date
- `summary`: Search in issue summaries
- `description`: Search in issue descriptions
- `creator`: Filter by creator
- `assignee`: Filter by assignee
- `status`: Filter by status
- `project`: Filter by project key
- `priority`: Filter by priority
- `issuetype`: Filter by issue type

**Example**:
```bash
curl "http://localhost:8000/api/jira/issues/?project=AO&status=Open"
```

#### 2. Jira Dashboard (`GET /api/jira/dashboard/`)

**Purpose**: Get statistical summaries of Jira data.

**Query Parameters**:
- `project_name`: Specific project for detailed stats
- `start_date`: Filter from this date
- `end_date`: Filter up to this date

**Example**:
```bash
curl "http://localhost:8000/api/jira/dashboard/?project_name=AO"
```

### Task Management Endpoints

#### 1. List All Tasks (`GET /api/jobs/`)

**Purpose**: View all mining tasks and their status.

**Example**:
```bash
curl "http://localhost:8000/api/jobs/"
```

#### 2. Check Task Status (`GET /api/jobs/tasks/{task_id}/`)

**Purpose**: Check the status of a specific mining task.

**Example**:
```bash
curl "http://localhost:8000/api/jobs/tasks/abc123-def456-ghi789/"
```

#### 3. Cancel Task (`DELETE /api/jobs/tasks/{task_id}/`)

**Purpose**: Cancel a running mining task.

**Example**:
```bash
curl -X DELETE "http://localhost:8000/api/jobs/tasks/abc123-def456-ghi789/"
```

### Data Export Endpoints

#### 1. Export Data (`POST /api/github/export/`)

**Purpose**: Export collected data in JSON format.

**Parameters**:
- `table`: Table name to export
- `ids` (optional): Array of specific IDs to export
- `format`: Export format (currently only "json")
- `data_type` (optional): For githubissuepullrequest table

**Example**:
```bash
curl -X POST "http://localhost:8000/api/github/export/" \
     -H "Content-Type: application/json" \
     -d '{
       "table": "githubcommit",
       "format": "json"
     }'
```

### Web Interface

The RAISE system includes a modern web dashboard built with Next.js that provides a graphical interface for data mining and visualization.

#### Step 1: Install Frontend Dependencies

```bash
# Open a new terminal
cd ../web_src/

# Create environment file
echo 'NEXT_PUBLIC_API_URL="http://localhost:8000"' > .env

# Install dependencies
npm install
```

#### Step 2: Start the Frontend Server

```bash
# Start development server
npm run dev
```

#### Step 3: Access the Dashboard

Open your browser and navigate to `http://localhost:3000`

#### Step 4: Using the Web Interface

**Main Features:**

1. **Dashboard Overview**
   - View mining statistics and summaries
   - Monitor active mining tasks
   - Access quick mining actions

2. **Data Collection**
   - Repository mining interface
   - Date range selection
   - Real-time progress tracking

3. **Data Visualization**
   - Charts and graphs for mined data
   - Repository activity timelines
   - Issue and commit analytics

4. **Task Management**
   - View all mining tasks
   - Monitor task status and progress
   - Cancel running tasks if needed

**Navigation:**
- **Dashboard**: Overview and statistics
- **Collect**: Start new mining operations
- **Jobs**: Monitor and manage mining tasks
- **Preview**: View and explore collected data

**Quick Start with Web Interface:**

1. **Mine a Repository:**
   - Go to the "Collect" page
   - Enter repository name (e.g., "octocat/Hello-World")
   - Select data types (commits, issues, pull requests)
   - Choose date range
   - Click "Start Mining"

2. **Monitor Progress:**
   - Go to "Jobs" page
   - View task status and progress
   - Check for any errors or warnings

3. **View Results:**
   - Go to "Preview" page
   - Select data type to explore
   - Apply filters and search
   - Export data if needed

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

## 🐛 Troubleshooting

### Common Issues

#### 1. Port 5432 Already in Use

**Problem**: PostgreSQL port conflict when starting containers.

**Solutions**:

**Option A: Stop existing PostgreSQL service**
```bash
# Linux/macOS
sudo lsof -ti:5432 | xargs sudo kill -9

# Windows: Open Task Manager → Processes → End "postgres.exe" tasks
```

**Option B: Use different port (if your client supports it)**
```yaml
# Edit docker-compose.yaml, change the port mapping:
ports:
  - "5433:5432"  # Use port 5433 instead of 5432
```

**Option C: Connect to existing PostgreSQL instance**
- If you have PostgreSQL already running, you can connect to it directly
- Update your `.env` file to point to your existing PostgreSQL instance
- Some database clients allow multiple connections to the same port

#### 2. Invalid or Expired API Tokens

**Problem**: Authentication errors when accessing protected endpoints.

**Symptoms**:
- HTTP 401 Unauthorized errors
- "Invalid token" messages in logs
- Rate limit errors even with valid tokens

**Solutions**:

**Check GitHub Token**:
```bash
curl -H "Authorization: token YOUR_GITHUB_TOKEN" https://api.github.com/user
```
**Expected response**: Your GitHub user information

**Check Jira Token**:
```bash
curl -H "Authorization: Bearer YOUR_JIRA_TOKEN" \
     https://YOUR_DOMAIN.atlassian.net/rest/api/3/myself
```
**Expected response**: Your Jira user information

**Regenerate Tokens**:
- GitHub: Go to [GitHub Settings > Developer Settings > Personal Access Tokens](https://github.com/settings/tokens)
- Jira: Go to [Jira API Tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
- Create new tokens and update your `.env` file

#### 3. Insufficient Memory

**Problem**: Docker containers fail to start or mining tasks fail.

**Solution**:
```yaml
# Edit docker-compose.yaml, section worker:
deploy:
  resources:
    limits:
      memory: 2G  # Reduce from 4G to 2G or 1G
```

#### 4. Docker Permission Issues (Linux)

**Problem**: "Permission denied" when running Docker commands.

**Solution**:
```bash
# Add user to docker group
sudo usermod -aG docker $USER

# Logout and login again, or restart terminal
```

#### 5. Network Connectivity Issues

**Problem**: Cannot access APIs or download Docker images.

**Solutions**:
- Check internet connection
- Configure proxy if behind corporate firewall
- Try different DNS servers

### Debug Logs

```bash
# View real-time logs
docker-compose logs -f web
docker-compose logs -f worker

# View specific error logs
docker-compose logs worker | grep ERROR

# View all logs
docker-compose logs
```

### Performance Optimization

For large repositories, consider:
- Using multiple API tokens to avoid rate limits
- Mining smaller date ranges
- Increasing Docker memory limits
- Using SSD storage for better I/O performance

## 📞 Support

For artifact-related questions:

1. **GitHub Issues**: [github.com/aisepucrio/raise-replication/issues](https://github.com/aisepucrio/raise-replication/issues)
2. **Complete Documentation**: See `tool_src/README.md` and `web_src/README.md`
3. **Original Paper**: [Link to be included]

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

**Artifact Status**: ✅ Available | 🔄 Functional | 📖 Reusable