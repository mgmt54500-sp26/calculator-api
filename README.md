# Calculator API - Getting Started

This is a simple calculator API built with FastAPI that demonstrates how to create, test, and deploy a Python API to Google Cloud Run.

## Overview

This project provides a minimal REST API with the following endpoints:
- `GET /` - Health check endpoint
- `GET /add` - Add two numbers together

The API is built using:
- **FastAPI** - Modern Python web framework
- **Poetry** - Dependency management
- **Docker** - Containerization for cloud deployment
- **Google Cloud Run** - Serverless deployment platform

## Prerequisites

Before you begin, make sure you have the following installed:
- Python 3.11 or higher
- Poetry (for dependency management)
- Git (for version control)
- A GitHub account
- A Google Cloud account (for deployment)

## Getting Started

### Step 1: Fork the Repository

1. Click the **Fork** button at the top right of this repository
2. This creates a copy of the repository under your GitHub account

### Step 2: Clone Your Fork

You will be using **Google Cloud Shell** as your development environment throughout this course. Open Google Cloud Shell and clone your forked repository.

**Option 1: HTTPS (simpler, but may require authentication):**
```bash
git clone https://github.com/YOUR-USERNAME/calculator-api.git
cd calculator-api
```

**Option 2: SSH (recommended if you have 2FA enabled on GitHub):**
```bash
git clone git@github.com:YOUR-USERNAME/calculator-api.git
cd calculator-api
```

Replace `YOUR-USERNAME` with your actual GitHub username.

> **Note:** If you're using SSH and haven't set up SSH keys with GitHub yet, follow [GitHub's SSH key setup guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

### Step 3: Install Dependencies

In the **Google Cloud Shell terminal**, use Poetry to install the project dependencies:

```bash
poetry install
```

This command:
- Creates a virtual environment
- Installs FastAPI and Uvicorn
- Generates a `poetry.lock` file (if not already present)

## Running the API in Google Cloud Shell

### Step 4: Start the Development Server

In the **Google Cloud Shell terminal**, start the development server with auto-reload enabled:

```bash
poetry run uvicorn main:app --reload --port 8080
```

You should see output like:
```
INFO:     Uvicorn running on http://127.0.0.1:8080 (Press CTRL+C to quit)
INFO:     Started reloader process
```

### Step 5: Access the API Using Web Preview

Since you're using Google Cloud Shell, you'll access your API through the **Web Preview** feature:

1. In the Google Cloud Shell toolbar, click the **Web Preview** button (looks like a square with an arrow)
2. Select **"Preview on port 8080"**
3. A new browser tab will open with a URL like: `https://8080-cs-XXXXXXXXXX-default.cs-us-central1-pits.cloudshell.dev/`

This is your API's base URL while running in Cloud Shell.

### Step 6: View Interactive API Documentation

FastAPI automatically generates interactive API documentation. Using the Web Preview URL from Step 5:

- **Swagger UI**: Add `/docs` to your Web Preview URL (e.g., `https://8080-cs-XXXXXXXXXX-default.cs-us-central1-pits.cloudshell.dev/docs`)

The Swagger UI allows you to test your API endpoints directly in the browser!

## Testing the API

### Step 7: Test the Health Check Endpoint

**Using Web Preview:**
Open your Web Preview URL (e.g., `https://8080-cs-XXXXXXXXXX-default.cs-us-central1-pits.cloudshell.dev/`) in your browser.

**Using curl in Google Cloud Shell:**
```bash
curl http://localhost:8080/
```

**Expected response:**
```json
{"status": "healthy"}
```

### Step 8: Test the Add Endpoint

**Using Web Preview:**
Add `/add/10/5` to your Web Preview URL (e.g., `https://8080-cs-XXXXXXXXXX-default.cs-us-central1-pits.cloudshell.dev/add/10/5`).

**Using curl in Google Cloud Shell:**
```bash
curl "http://localhost:8080/add/10/5"
```

**Expected response:**
```json
{"result": 15.0}
```

**Try different numbers:**
```bash
curl "http://localhost:8080/add/2.5/3.7"
```

**Expected response:**
```json
{"result": 6.2}
```

## Making Changes

### Step 9: Modify the Code

1. In Google Cloud Shell, open `main.py` in the built-in editor (click the pencil icon or use `cloudshell edit main.py`)
2. Make your changes (e.g., add a new endpoint for subtraction)
3. Save the file

If the development server is running with `--reload`, it will automatically restart with your changes.

### Step 10: Test Your Changes

Use the interactive documentation at your Web Preview URL with `/docs` (e.g., `https://8080-cs-XXXXXXXXXX-default.cs-us-central1-pits.cloudshell.dev/docs`) to test your modifications.

### Step 11: Commit and Push

Once you're satisfied with your changes, use the **Google Cloud Shell terminal** to commit and push:

```bash
git add .
git commit -m "Description of your changes"
git push origin main
```

This pushes your changes to your GitHub repository.

## Cloud Run Deployment

### Step 12: Prepare Your Repository

In the **Google Cloud Shell terminal**, make sure all your changes are committed and pushed to GitHub:

```bash
git status  # Check for uncommitted changes
git add .
git commit -m "Ready for deployment"
git push origin main
```

### Step 13: Connect to Google Cloud Run

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to **Cloud Run** (use the search bar if needed)
3. Click **Create Service**

### Step 14: Configure the Service

**Source:**
- Select **"Continuously deploy from a repository"**
- Click **"Set up with Cloud Build"**
- Connect your GitHub account and select your forked repository
- Branch: `main`
- Build type: **Dockerfile**
- Click **"Save"**

**Configure service settings:**
- **Region**: Choose a region close to you (e.g., `us-central1`)
- **Authentication**: Select **"Allow unauthenticated invocations"** (for testing)
- **Container port**: Leave as `8080` (default)
- Click **"Create"**

### Step 15: Wait for Deployment

Cloud Build will:
1. Clone your repository
2. Build a Docker image using your Dockerfile
3. Deploy the image to Cloud Run

This takes 2-5 minutes. You'll see the build progress in the console.

### Step 16: Test Your Deployed API

Once deployment completes, you'll receive a URL like:
```
https://calculator-api-XXXXXXXX-uc.a.run.app
```

**Test the endpoints using Google Cloud Shell or your browser:**

Health check:
```bash
curl https://YOUR-SERVICE-URL/
```

Add endpoint:
```bash
curl "https://YOUR-SERVICE-URL/add/20/30"
```

You should see the same responses as when testing in Cloud Shell!

### Step 17: Understand Automatic Deployments

Now, whenever you push changes to your GitHub repository's `main` branch, Cloud Run will automatically:
1. Detect the changes
2. Rebuild your Docker image
3. Deploy the new version

## Troubleshooting

### Poetry Issues

**Problem:** `poetry: command not found`
- **Solution:** Poetry is not installed. This course assumes Poetry is pre-installed on your system. Check with your instructor.

**Problem:** `poetry install` fails with dependency errors
- **Solution:** Make sure you're using Python 3.11 or higher in Google Cloud Shell:
  ```bash
  python --version
  ```

### Development Issues

**Problem:** Port 8080 is already in use
- **Solution:** Either stop the other application using port 8080, or run on a different port in Google Cloud Shell:
  ```bash
  poetry run uvicorn main:app --reload --port 8081
  ```

**Problem:** `422 Unprocessable Entity` error when testing `/add`
- **Solution:** This means you're missing a required parameter or using the wrong type. The `/add` endpoint requires both `a` and `b` path parameters:
  ```bash
  # Wrong (missing b):
  curl "http://localhost:8080/add/5"
  
  # Correct:
  curl "http://localhost:8080/add/5/3"
  ```

**Problem:** Changes not reflecting in the browser
- **Solution:** Hard refresh your browser (Ctrl+Shift+R or Cmd+Shift+R) or check that `--reload` flag is enabled.

### Docker Build Issues

**Problem:** `poetry.lock` not found during Docker build
- **Solution:** Run `poetry install` in Google Cloud Shell first to generate the lock file, then commit it:
  ```bash
  poetry install
  git add poetry.lock
  git commit -m "Add poetry.lock"
  git push
  ```

**Problem:** Docker build fails with permission errors
- **Solution:** This is usually a Cloud Build issue. Check the build logs in the Google Cloud Console for specific errors.

### Cloud Run Deployment Issues

**Problem:** Build fails with "Could not find a version that matches..."
- **Solution:** Your `poetry.lock` file might be out of sync. In Google Cloud Shell, delete it and regenerate:
  ```bash
  rm poetry.lock
  poetry install
  git add poetry.lock
  git commit -m "Regenerate poetry.lock"
  git push
  ```

**Problem:** Service is deployed but returns 404 for all endpoints
- **Solution:** Check that your Dockerfile is correctly specifying `main:app` in the CMD instruction. The format is `filename:app_variable`.

**Problem:** Service health check fails / service won't start
- **Solution:** Cloud Run expects your app to listen on the port specified by the `PORT` environment variable. Check that your Dockerfile uses `--port $PORT` (not a hardcoded port).

**Problem:** Getting authentication errors when accessing the deployed API
- **Solution:** In Cloud Run service settings, make sure "Allow unauthenticated invocations" is enabled under Authentication.

**Problem:** Changes pushed to GitHub aren't deploying
- **Solution:** Check Cloud Build history in the Google Cloud Console. You might need to manually trigger a deployment or check for build errors.

### Getting Help

If you encounter issues not covered here:
1. Check the error message carefully - it often tells you exactly what's wrong
2. Search for the specific error message online
3. Ask your instructor or teaching assistant
4. Review the FastAPI documentation: https://fastapi.tiangolo.com/
5. Review the Cloud Run documentation: https://cloud.google.com/run/docs

## Next Steps

Once you have the basic calculator working, try these extensions:
- Add a `/subtract` endpoint
- Add a `/multiply` endpoint
- Add a `/divide` endpoint (bonus: handle division by zero!)
- Add input validation to ensure numbers are within a reasonable range
- Add logging to track API usage
- Add unit tests with pytest

## Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Poetry Documentation](https://python-poetry.org/docs/)
- [Google Cloud Run Documentation](https://cloud.google.com/run/docs)
- [Docker Documentation](https://docs.docker.com/)
