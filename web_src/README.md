# RAISE Web Frontend Documentation

This directory contains the Next.js-based web frontend for the RAISE system, providing a modern graphical interface for data mining and visualization.

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 18.0
- **npm** or **yarn** package manager
- **RAISE Backend** running on `http://localhost:8000`

### Installation

```bash
# Navigate to the frontend directory
cd web_src/

# Install dependencies
npm install

# Create environment file
echo 'NEXT_PUBLIC_API_URL="http://localhost:8000"' > .env

# Start development server
npm run dev
```

### Access the Dashboard

Open your browser and navigate to `http://localhost:3000`

## 🎯 Main Features

### 1. Dashboard Overview

The main dashboard provides a comprehensive overview of your mining activities:

- **Mining Statistics**: View summaries of collected data
- **Active Tasks**: Monitor ongoing mining operations
- **Quick Actions**: Access common mining functions
- **System Status**: Check API connectivity and service health

**Navigation**: Click on "Dashboard" in the main menu

### 2. Data Collection Interface

The collection interface allows you to mine data from GitHub repositories and Jira projects:

#### GitHub Repository Mining

1. **Navigate to Collect Page**
   - Click "Collect" in the main menu
   - Select "GitHub" as the data source

2. **Configure Mining Parameters**
   - **Repository Name**: Enter in format `owner/repo` (e.g., "octocat/Hello-World")
   - **Data Types**: Select what to collect:
     - ✅ Commits (code changes and metrics)
     - ✅ Issues (bug reports and feature requests)
     - ✅ Pull Requests (code reviews and merges)
     - ✅ Branches (repository structure)
     - ✅ Metadata (stars, forks, languages)
   - **Date Range**: Optional filtering by time period
   - **Depth**: Choose "Basic" or "Complex" data collection

3. **Start Mining**
   - Click "Start Mining" button
   - Monitor progress in real-time
   - View task status and completion

#### Jira Project Mining

1. **Select Jira Source**
   - Choose "Jira" as the data source
   - Enter your Jira domain (e.g., "company.atlassian.net")

2. **Configure Project Mining**
   - **Project Key**: Enter the project identifier (e.g., "PROJ")
   - **Issue Types**: Filter by specific issue types
   - **Date Range**: Optional time filtering
   - **Advanced Options**: Configure additional parameters

3. **Execute Mining**
   - Click "Start Mining"
   - Track progress through the task system

### 3. Task Management

The Jobs page provides comprehensive task management capabilities:

#### View All Tasks
- **Active Tasks**: Currently running mining operations
- **Completed Tasks**: Successfully finished operations
- **Failed Tasks**: Operations that encountered errors
- **Task Details**: Click on any task for detailed information

#### Task Information Displayed
- **Task ID**: Unique identifier for the operation
- **Status**: Running, Completed, Failed, or Cancelled
- **Progress**: Percentage completion and current step
- **Start Time**: When the task began
- **Duration**: How long the task has been running
- **Repository/Project**: What data source is being mined
- **Data Types**: What information is being collected

#### Task Actions
- **Cancel Task**: Stop a running mining operation
- **View Logs**: Access detailed execution logs
- **Retry Task**: Restart a failed operation
- **Export Results**: Download collected data

### 4. Data Preview and Exploration

The Preview page allows you to explore and analyze collected data:

#### Data Type Selection
- **Commits**: View code changes and metrics
- **Issues**: Browse bug reports and feature requests
- **Pull Requests**: Examine code reviews and merges
- **Branches**: Explore repository structure
- **Metadata**: View repository statistics

#### Filtering and Search
- **Repository Filter**: Select specific repositories
- **Date Range**: Filter by creation or update dates
- **Text Search**: Search in titles, descriptions, or messages
- **Status Filter**: Filter by state (open, closed, merged)
- **Author Filter**: Filter by contributor

#### Data Visualization
- **Charts**: Interactive graphs showing activity trends
- **Timelines**: Visual representation of project evolution
- **Statistics**: Summary metrics and analytics
- **Export Options**: Download data in various formats

## 🔧 Configuration

### Environment Variables

Create a `.env` file in the `web_src/` directory:

```env
# API Configuration
NEXT_PUBLIC_API_URL="http://localhost:8000"

# Optional: Custom API timeout (default: 30000ms)
NEXT_PUBLIC_API_TIMEOUT=30000

# Optional: Enable debug mode
NEXT_PUBLIC_DEBUG=true
```

### API Communication

The frontend communicates with the RAISE backend through RESTful APIs:

#### Authentication
- The frontend uses token-based authentication
- Tokens are managed through the backend configuration
- No additional authentication is required for the web interface

#### Data Flow
1. **User Input**: Forms and configuration in the UI
2. **API Requests**: Frontend sends requests to backend endpoints
3. **Task Creation**: Backend creates asynchronous mining tasks
4. **Progress Monitoring**: Frontend polls task status endpoints
5. **Data Retrieval**: Results are fetched and displayed
6. **Visualization**: Data is processed and rendered in charts

#### Error Handling
- **Network Errors**: Automatic retry with exponential backoff
- **API Errors**: User-friendly error messages displayed
- **Validation Errors**: Real-time form validation
- **Timeout Handling**: Graceful degradation for slow operations

## 🎨 User Interface Features

### Responsive Design
- **Desktop**: Full-featured interface with advanced controls
- **Tablet**: Optimized layout for touch interaction
- **Mobile**: Simplified view for on-the-go access

### Theme Support
- **Light Mode**: Default clean interface
- **Dark Mode**: Eye-friendly dark theme
- **Auto-Detect**: Follows system preference

### Accessibility
- **Keyboard Navigation**: Full keyboard support
- **Screen Reader**: ARIA labels and semantic HTML
- **High Contrast**: Enhanced visibility options
- **Font Scaling**: Adjustable text sizes

## 📊 Data Visualization

### Charts and Graphs
- **Activity Timeline**: Shows project activity over time
- **Commit Frequency**: Displays code change patterns
- **Issue Distribution**: Visualizes bug and feature distribution
- **Contributor Activity**: Shows team member contributions
- **Repository Health**: Overall project metrics

### Interactive Features
- **Zoom and Pan**: Explore detailed time periods
- **Filtering**: Real-time data filtering
- **Export**: Download charts as images or data
- **Drill-Down**: Click to explore specific data points

## 🔍 Advanced Usage

### Bulk Operations
- **Multiple Repositories**: Mine several repositories simultaneously
- **Batch Processing**: Queue multiple mining tasks
- **Scheduled Mining**: Set up recurring data collection

### Data Export
- **JSON Format**: Structured data export
- **CSV Format**: Spreadsheet-compatible export
- **Custom Filters**: Export specific data subsets
- **Bulk Download**: Download multiple datasets

### Performance Optimization
- **Lazy Loading**: Load data on demand
- **Caching**: Intelligent data caching
- **Pagination**: Handle large datasets efficiently
- **Background Processing**: Non-blocking operations

## 🐛 Troubleshooting

### Common Issues

#### 1. Frontend Cannot Connect to Backend

**Symptoms**:
- "API Error" messages
- Loading spinners that never complete
- Empty dashboard

**Solutions**:
```bash
# Check if backend is running
curl http://localhost:8000/api/

# Verify environment configuration
cat .env
# Should contain: NEXT_PUBLIC_API_URL="http://localhost:8000"

# Restart frontend with fresh environment
npm run dev
```

#### 2. Slow Performance

**Symptoms**:
- Slow page loading
- Unresponsive interface
- Timeout errors

**Solutions**:
- Check network connectivity
- Clear browser cache

#### 3. Missing Data

**Symptoms**:
- Empty charts and tables
- "No data available" messages

**Solutions**:
- Verify mining tasks completed successfully
- Check repository permissions
- Ensure API tokens are valid

## 📞 Support

For frontend-related questions:

1. **Backend Documentation**: See [../tool_src/README.md](../tool_src/README.md) for API details
2. **Main Documentation**: See the main README.md for setup instructions
