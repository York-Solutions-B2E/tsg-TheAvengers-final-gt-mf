# Provider Feedback Portal

## Overview
The Provider Feedback Portal is a full-stack microservices application consisting of:

- **Frontend (React + Vite)**  
  A simple UI that allows users to submit provider feedback and retrieve feedback records.

- **Backend Feedback API (Spring Boot + Postgres + Kafka Producer)**  
  Handles feedback submissions, stores them in Postgres, and publishes feedback events to Kafka.

- **Analytics Consumer (Spring Boot + Kafka Consumer)**  
  Listens for feedback-submitted events and logs or processes them for analytics purposes.

This repository serves as the **master orchestration repo**. It does not contain code for the three applications directly.  
Instead, it references each service as a **Git submodule** and provides a unified `docker-compose.yml` file to bring all services up together.

---
## Cloning & Opening This Project
## 1. Clone the Repository (Important: Includes Submodules)

This project uses Git submodules to reference:

- `feedback-api/`
- `feedback-analytics-consumer/`
- `frontend-feedback-ui/`

To ensure the full project is downloaded, clone **with submodules enabled**:

```angular2html  
git clone --recursive https://github.com/York-Solutions-B2E/tsg-TheAvengers-final-gt-mf.git  
  
```  

If you already cloned it without --recursive, run:
```angular2html  
git submodule update --init --recursive  
```  

## 2. Opening the Project (IntelliJ / VS Code)
To avoid issues where submodule folders appear empty, always open the project using the IDE's menu system.

IntelliJ IDEA
```angular2html  
File → Open… → Select the project folder (tsg-TheAvengers-final-gt-mf)  
```  

VS Code
```angular2html  
File → Open Folder… → Select the project folder  
```  

Avoid:

Dragging the folder from Finder into IntelliJ or VS Code

Using “Open Recent”

Using IntelliJ’s built-in Git clone dialog (it does not initialize submodules consistently)

## 3. Running the Entire Project with Docker Compose

Once the project has been cloned and opened correctly:

From the **root** of the repository, run:
```
docker-compose up --build
```
This will:

- Build and start the **frontend**

- Build and start the **feedback-api**

- Build and start the **analytics consumer**

- Start **Postgres**

- Start **Kafka + Zookeeper** (or Kraft, depending on your compose file)


After the build completes:

- Frontend will be available on: **http://localhost:5173**

- API will be available on: **http://localhost:8080/api/v1/feedback**

- Kafka UI (if included) will be available on its configured port

- Postgres will be running internally at the configured hostname


Once `docker-compose` is running without errors, the entire application stack should be fully operational.

## Features

The Provider Feedback Portal demonstrates a full, event-driven microservices architecture with the following capabilities:

#### Frontend (React + Vite)
- Submit provider feedback (member ID, provider name, rating, comment)
- Retrieve feedback by ID or member ID
- Clean, simple UI styled with Tailwind
- Error handling and form validation

#### Feedback API (Spring Boot + Postgres + Kafka Producer)
- Accepts new feedback submissions via REST
- Validates and persists feedback to Postgres
- Publishes a `feedback-submitted` event to Kafka
- Exposes `GET /feedback/{id}` and `GET /feedback?memberId=` endpoints

#### Analytics Consumer (Spring Boot + Kafka Consumer)
- Listens to `feedback-submitted` events
- Logs or processes them for analytics purposes
- Demonstrates proper Kafka consumer configuration and idempotent handling

#### Dockerized Architecture
- All services run via a single `docker-compose up --build`
- Containers include:
    - Frontend UI
    - Feedback API
    - Analytics Consumer
    - Postgres database
    - Kafka broker + Zookeeper (or KRaft, depending on your configuration)

---

## How It Works (Event Flow)

Below is the high-level flow of data through the system:

1. **User submits feedback through the frontend UI.**  
   The form sends a `POST` request to the Feedback API endpoint:  
   `POST /api/v1/feedback`

2. **Feedback API validates and saves the submission.**
    - Performs field validation
    - Writes feedback to Postgres
    - Constructs a `FeedbackEvent` DTO

3. **Feedback API publishes a Kafka event.**  
   After persisting the feedback, the API publishes a `feedback-submitted` event to Kafka.  
   This event contains:
    - feedback ID
    - member ID
    - provider name
    - rating
    - comment
    - timestamp

4. **Kafka receives and stores the event.**  
   The event lands on the configured topic (ex: `feedback-submitted`).

5. **Analytics Consumer listens for the event.**  
   The consumer service:
    - picks up the event from Kafka
    - deserializes it
    - logs/processes it as needed

6. **Frontend or API can later fetch stored feedback.**  
   Endpoints:
    - `GET /api/v1/feedback/{id}`
    - `GET /api/v1/feedback?memberId=123`

This demonstrates a complete producer → topic → consumer event lifecycle using Spring Boot + Kafka.

---

## Additional Documentation for Each Service

Each submodule in this repository represents a standalone application with its own codebase and responsibilities:

- `feedback-api/`
- `feedback-analytics-consumer/`
- `frontend-feedback-ui/`

Each of these service folders contains its own **README.md** with more detailed documentation, including:

- Local development instructions
- Technology stack details
- Endpoints and API behavior
- Kafka topics and message flow
- Environment variables and configuration
- Testing instructions

If you need deeper information about a particular microservice, refer directly to the README inside that module's folder.

---

## Updating Submodules (When Services Change)

Because this repository uses **Git submodules**, each service points to a specific commit from its own repository.  
If new code is pushed to any of the submodule repos, the master repo will not automatically update.  
You must manually update the submodule pointer.

#### Update a Single Submodule

Navigate into the submodule folder:
```
cd feedback-analytics-consumer
```
Pull the latest code from its main branch:
```
git pull --rebase origin main
```
Stage and commit the updated submodule pointer in the project root:
```
git add feedback-analytics-consumer
git commit -m "Update analytics consumer to latest commit"
git push

```
#### Update **All** Submodules at Once

From the root of the master repo:
```
git submodule update --remote --merge
```
Then commit the changes:
```
git add .
git commit -m "Update all submodules to latest versions"
git push
```

## 👥 Team

**The Avengers**
- Michael Files
- Gregg Trunnell