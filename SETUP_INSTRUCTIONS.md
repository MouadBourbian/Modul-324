# Setup Instructions for CI/CD Pipeline

## What Has Been Implemented

A complete CI/CD pipeline has been created at `.github/workflows/ci.yml` with the following features:

### ✅ Completed Features

1. **Linting with ESLint** (`npm run lint`)
   - Automatically checks code quality and style
   - Runs on every push and pull request
   - Uses the existing ESLint configuration

2. **Testing with Jest** (`npm run test`)
   - Runs all tests automatically
   - Uses a MongoDB service container for database tests
   - No external MongoDB setup required for CI

3. **Deployment Planning**
   - Deployment section is commented out and ready for future implementation
   - Contains detailed instructions for when you're ready to deploy
   - Supports multiple deployment options (Heroku, Railway, Render, AWS, etc.)

## Required Actions

### Step 1: Approve the Workflow Run (Required)

Since this is a first-time workflow on your repository, GitHub requires approval:

1. Go to your repository on GitHub: https://github.com/MouadBourbian/Modul-324
2. Click on the "Actions" tab
3. You should see the "CI Pipeline" workflow waiting for approval
4. Click "Approve and run" to allow the workflow to execute

This approval is only needed once. Future runs will execute automatically.

### Step 2: Verify the Pipeline Works

Once approved, the pipeline should:
1. ✅ Pass linting (ESLint will show 1 warning which is expected)
2. ✅ Pass testing (if MongoDB service starts correctly)

Check the workflow run details to see the results.

## Local Development Setup

### Prerequisites
- Node.js 20 or higher
- MongoDB (local installation or MongoDB Atlas account)

### Setup Steps

1. **Navigate to the backend directory:**
   ```bash
   cd ticketsystem/backend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Create a `.env` file:**
   
   Copy the example file:
   ```bash
   cp .env.example .env
   ```
   
   Then edit `.env` and add your MongoDB connection string:
   ```env
   MONGO_URL=your_mongodb_connection_string_here
   PORT=6001
   NODE_ENV=development
   ```

4. **Run the application:**
   ```bash
   # Development mode (with auto-reload)
   npm run dev

   # Production mode
   npm start
   ```

5. **Run tests:**
   ```bash
   npm test
   ```

6. **Run linting:**
   ```bash
   # Check for issues
   npm run lint

   # Auto-fix issues
   npm run lint:fix
   ```

## MongoDB Setup Options

### Option 1: MongoDB Atlas (Recommended for Development)

1. Sign up at https://www.mongodb.com/cloud/atlas
2. Create a free M0 cluster
3. Create a database user
4. Get the connection string
5. Add it to your `.env` file

**Connection String Format:**
```
mongodb+srv://username:password@cluster.mongodb.net/ticketsystem?retryWrites=true&w=majority
```

### Option 2: Local MongoDB with Docker

```bash
docker run -d -p 27017:27017 --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo:7.0
```

**Connection String:**
```
mongodb://admin:password@localhost:27017/ticketsystem?authSource=admin
```

### Option 3: Local MongoDB Installation

Install MongoDB locally and use:
```
mongodb://localhost:27017/ticketsystem
```

## Future: Deployment Setup

When you're ready to deploy to production:

### Step 1: Choose a Deployment Platform

Consider these options:
- **Heroku**: Easy setup, great for beginners
- **Railway**: Modern platform, excellent developer experience
- **Render**: Free tier with automatic SSL
- **AWS/Azure/Google Cloud**: More control, better for scaling
- **Docker + VPS**: Full control, learn DevOps

### Step 2: Add GitHub Secret for Production Database

1. Go to your repository settings: https://github.com/MouadBourbian/Modul-324/settings/secrets/actions
2. Click "New repository secret"
3. Add a secret named `MONGO_URL` with your MongoDB Atlas production connection string

### Step 3: Enable Deployment in Workflow

Uncomment the `deploy` job section in `.github/workflows/ci.yml` and configure it for your chosen platform.

Detailed deployment instructions are available in `.github/workflows/README.md`.

## Troubleshooting

### Workflow not starting?
- Check if you approved the workflow in the Actions tab
- Ensure the workflow file is on the correct branch

### Tests failing?
- Check if MongoDB service is starting correctly in the workflow
- View the workflow logs for detailed error messages

### Linting errors?
- Run `npm run lint:fix` to auto-fix many issues
- Check ESLint configuration in `eslint.config.js`

### Local tests failing?
- Ensure MongoDB is running
- Check if `MONGO_URL` is set correctly in `.env`
- Verify the connection string format

## Resources

- **Workflow Documentation**: `.github/workflows/README.md`
- **GitHub Actions**: https://docs.github.com/en/actions
- **MongoDB Atlas**: https://www.mongodb.com/docs/atlas/
- **Express.js**: https://expressjs.com/
- **Jest Testing**: https://jestjs.io/

## Summary

✅ **Pipeline is ready to use!**
- Linting and testing are fully configured
- MongoDB is handled automatically in CI
- Deployment is planned and documented for future implementation

🔧 **Next steps:**
1. Approve the workflow in GitHub Actions
2. Set up your local `.env` file with MongoDB connection
3. Start developing!
4. When ready, configure deployment using the provided documentation
