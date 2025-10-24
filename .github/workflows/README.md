# CI/CD Pipeline Documentation

## Overview

This repository uses GitHub Actions for continuous integration and planned continuous deployment.

## Current Pipeline Steps

### 1. Linting (ESLint)
- **Command**: `npm run lint`
- **Location**: `./ticketsystem/backend`
- **Description**: Validates code quality and style using ESLint with Prettier integration
- **Trigger**: On every push and pull request

### 2. Testing (Jest)
- **Command**: `npm run test`
- **Location**: `./ticketsystem/backend`
- **Description**: Runs unit and integration tests using Jest and Supertest
- **Database**: Uses MongoDB service container in GitHub Actions
- **Test Database URL**: `mongodb://testuser:testpassword@localhost:27017/ticketsystem-test?authSource=admin`
- **Trigger**: On every push and pull request

### 3. Deployment (Planned - On Hold)
- **Status**: Currently commented out in the workflow
- **When**: Will run only on pushes to `main` branch
- **Requirements**: See setup instructions below

## Environment Setup

### For CI/CD Pipeline (GitHub Actions)

The pipeline uses a MongoDB service container for running tests. No additional secrets are required for linting and testing.

### For Deployment (Future Implementation)

When you're ready to implement deployment, you'll need to configure the following:

#### Required GitHub Secrets

Go to: **Repository Settings → Secrets and variables → Actions → New repository secret**

Add the following secret:

| Secret Name | Description | Example Value |
|------------|-------------|---------------|
| `MONGO_URL` | MongoDB Atlas connection string | `mongodb+srv://username:password@cluster.mongodb.net/ticketsystem?retryWrites=true&w=majority` |

#### Optional GitHub Secrets (depending on deployment target)

| Secret Name | Description | When Needed |
|------------|-------------|-------------|
| `DEPLOYMENT_TOKEN` | API token for deployment service | For automated deployments to cloud platforms |
| `SSH_PRIVATE_KEY` | SSH key for server access | For deployments to VPS/dedicated servers |
| `DOCKER_USERNAME` | Docker Hub username | For Docker-based deployments |
| `DOCKER_PASSWORD` | Docker Hub password/token | For Docker-based deployments |

### MongoDB Atlas Setup

1. **Create a MongoDB Atlas Account**
   - Go to [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
   - Sign up for a free account

2. **Create a Cluster**
   - Create a free M0 cluster
   - Choose a cloud provider and region

3. **Configure Database Access**
   - Go to "Database Access" in the Atlas dashboard
   - Create a database user with a strong password
   - Save the username and password securely

4. **Configure Network Access**
   - Go to "Network Access" in the Atlas dashboard
   - Add IP Address: `0.0.0.0/0` (allows access from anywhere - for development/CI)
   - For production, restrict to specific IPs

5. **Get Connection String**
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user's password
   - Replace `<dbname>` with `ticketsystem` (or your preferred database name)

6. **Add to GitHub Secrets**
   - Go to repository settings → Secrets → Actions
   - Add new secret named `MONGO_URL`
   - Paste your connection string

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/MouadBourbian/Modul-324.git
   cd Modul-324/ticketsystem/backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create `.env` file**
   ```bash
   # Create .env file in ticketsystem/backend/
   touch .env
   ```

4. **Add environment variables to `.env`**
   ```env
   MONGO_URL=mongodb+srv://username:password@cluster.mongodb.net/ticketsystem?retryWrites=true&w=majority
   PORT=6001
   NODE_ENV=development
   ```

5. **Run the application**
   ```bash
   # Development mode with auto-reload
   npm run dev

   # Production mode
   npm start
   ```

6. **Run tests**
   ```bash
   # Make sure MONGO_URL is set in .env
   npm test
   ```

7. **Run linting**
   ```bash
   npm run lint

   # Auto-fix linting issues
   npm run lint:fix
   ```

## Deployment Options (Future)

When ready to implement deployment, consider these options:

### Option 1: Heroku
- Easy setup with Git integration
- Free tier available
- Automatic SSL certificates
- Add-ons for monitoring

### Option 2: Railway
- Modern platform with great DX
- Free tier with GitHub integration
- Easy environment variable management

### Option 3: Render
- Free tier for web services
- Native Docker support
- Auto-deploy from GitHub

### Option 4: AWS/Azure/Google Cloud
- More control and scalability
- Requires more configuration
- Cost-effective for larger applications

### Option 5: Docker + VPS
- Full control over infrastructure
- Requires manual server management
- Good for learning DevOps

## Troubleshooting

### Tests failing in CI but passing locally
- Check if MongoDB service is properly initialized
- Verify the MongoDB connection string format
- Check if there are timing issues with service startup

### Linting errors
- Run `npm run lint:fix` to auto-fix many issues
- Check ESLint configuration in `eslint.config.js`
- Ensure all files follow the project's coding standards

### Deployment issues
- Verify all required secrets are set
- Check application logs in the deployment platform
- Ensure MongoDB Atlas network access is configured correctly
- Verify the connection string format and credentials

## Pipeline Status Badge

Add this to your README to show pipeline status:

```markdown
![CI Pipeline](https://github.com/MouadBourbian/Modul-324/workflows/CI%20Pipeline/badge.svg)
```

## Contact

For questions or issues with the pipeline, please open an issue in this repository.
