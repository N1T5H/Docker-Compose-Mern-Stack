# A simple MERN stack application

This project is a simple MERN (MongoDB, Express, React, Node.js) stack application that uses Docker for containerization. The application consists of a frontend, backend, and MongoDB database, all running in separate containers.

## Prerequisites

- Docker
- Docker Compose

## Setup

### Network Creation

Create a network for the Docker containers:

```bash
docker network create demo
```

## Components

### Frontend

The frontend is a React application.

#### Building the Frontend

```bash
cd mern/frontend
docker build -t mern-frontend .
```

#### Running the Frontend

```bash
docker run --name=frontend --network=demo -d -p 5173:5173 mern-frontend
```

To verify the frontend is running, open your browser and navigate to `http://localhost:5173`[1].

### Backend

The backend is an Express.js application.

#### Building the Backend

```bash
cd mern/backend
docker build -t mern-backend .
```

#### Running the Backend

```bash
docker run --name=backend --network=demo -d -p 5050:5050 mern-backend
```

### MongoDB

#### Running MongoDB

```bash
docker run --network=demo --name mongodb -d -p 27017:27017 -v ~/opt/data:/data/db mongodb:latest
```

## Using Docker Compose

Instead of running each container separately, you can use Docker Compose to orchestrate the entire application:

```bash
docker compose up -d
```

This command will build and run all the services defined in the `docker-compose.yaml` file.

## Docker Compose Configuration

The `docker-compose.yaml` file defines the following services:

1. **Backend**:
   - Built from `./mern/backend`
   - Exposed on port 5050
   - Connected to the `mern_network`
   - Depends on the MongoDB service

2. **Frontend**:
   - Built from `./mern/frontend`
   - Exposed on port 5173
   - Connected to the `mern_network`

3. **MongoDB**:
   - Uses the latest MongoDB image
   - Exposed on port 27017
   - Connected to the `mern_network`
   - Data persisted using a local volume

The compose file also defines a custom network (`mern_network`) and a volume for MongoDB data persistence[2].
