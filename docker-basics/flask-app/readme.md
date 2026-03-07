# Simple Flask To-Do App

This is a basic To-Do list application built with Flask. It's designed as a simple, stateless service perfect for learning Docker and containerization concepts.

## How to Run Locally (Without Docker)

To run the application directly on your local machine, you'll need Python 3 installed.

### 1. Prepare the Directory Structure

Flask expects HTML templates to be in a `templates` folder. If it's not already done, ensure your `index.html` file is inside a `templates` subdirectory.

```
flask-app/
├── app.py
├── requirements.txt
└── templates/
    └── index.html
```

### 2. Set Up a Virtual Environment

It's a best practice to use a virtual environment to manage project dependencies.

```bash
# Create the virtual environment
python3 -m venv venv

# Activate it
# On macOS/Linux:
source venv/bin/activate
# On Windows:
# venv\Scripts\activate
```

### 3. Install Dependencies

Install Flask using the `requirements.txt` file.

```bash
pip install -r requirements.txt
```

### 4. Run the Application

Start the Flask development server. The app is configured to run on port 5001.

```bash
python app.py
```

You can now access the application by navigating to `http://127.0.0.1:5001` in your web browser.

---

## How to Run with Docker

### 1. The Dockerfile

This `Dockerfile` containerizes the application. It sets up a Python environment, installs dependencies, copies the application code, and defines the command to run the app.

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the requirements file into the container at /app
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy the current directory contents into the container at /app
COPY . .

# Make port 5001 available to the world outside this container
EXPOSE 5001

# Run app.py when the container launches
CMD ["python", "app.py"]
```

### 2. Build and Run the Container

Use the following commands to build the Docker image and run it as a container.

```bash
# 1. Build the Docker image and tag it as 'flask-todo-app'
docker build -t flask-todo-app .

# 2. Run the container
# -p 5001:5001 maps port 5001 on your host to port 5001 in the container
# -d runs the container in detached mode (in the background)
docker run -d -p 5001:5001 flask-todo-app
```

After running the container, access the app at `http://127.0.0.1:5001`.

---

## How to Run with Docker Compose

Docker Compose is the recommended way to run multi-service applications in development. It uses a YAML file to configure the application's services.

### 1. The `docker-compose.yml` File

This file defines the `flask-app` service.

```yaml
version: '3.8'

services:
  flask-app:
    build: .
    ports:
      - '5001:5001'
    volumes:
      - .:/app
    restart: always
```

- **`build: .`**: Builds the image from the `Dockerfile` in the current directory.
- **`ports`**: Maps the host port to the container port.
- **`volumes`**: Mounts the current directory into the container's `/app` directory. This enables live-reloading of your code without rebuilding the image.

### 2. Run with Compose

Navigate to your project directory in the terminal and run:

```bash
# Build the image and start the container
docker compose up --build

# To run in the background, add the -d flag
# docker compose up --build -d
```

Access the app at `http://127.0.0.1:5001`.

To stop and remove the containers defined in the compose file, run:

```bash
docker compose down
```
