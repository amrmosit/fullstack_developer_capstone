# Car Dealerships Reviews Portal

A full-stack web application that allows users to browse car dealerships, read reviews, and submit their own experiences. Designed to improve customer transparency and trust for a national car dealership chain.

## Project Overview

This platform allows:
- Visitors to view dealerships by state and read reviews.
- Registered users to log in and submit reviews.
- Admins to manage car makes, models, and dealership data.

## Architecture Overview

This project uses a microservices-based architecture with containerization via Docker Compose. The major components include:

- **Frontend**: React.js + Bootstrap (User Interface)
- **Backend**: 
  - Django (API Gateway, Authentication, Static Pages)
  - Node.js + Express (Dealership & Review Microservice)
  - Python & Flask Microservice (Sentiment Analysis) 
- **Databases**: 
  - SQLite (Django - Cars)
  - MongoDB (Express - Dealers and Reviews)
- **DevOps**: Docker, CI/CD, Kubernetes (for final deployment)

## 🧪 Features

 Role          Capabilities                                                                 
--------------------------------------------------------------------------------------------
 Anonymous     View About, Contact, Dealerships by state, and Dealer Reviews               
 Authorized    Submit Reviews, See Submitted Review Sorted by Time                         
 Admin         Add Car Makes/Models, Dealership Records                                     

## Key Routes

### Django (Proxy & Core):
- `/get_dealers/` – Fetch all dealers
- `/get_dealers/:state` – Dealers filtered by state
- `/dealer/:id` – Dealer detail
- `/review/dealer/:id` – Get dealer reviews
- `/add_review/` – Submit review (with sentiment)

### Express (MongoDB API):
- `/fetchDealers`
- `/fetchDealer/:id`
- `/fetchReviews`
- `/fetchReview/dealer/:id`
- `/insertReview`

### Sentiment Analyzer:
- `/analyze/:text`

## 🐳 Docker Images

This project includes 4 Dockerized components:
1. Django Web App image: docker.io/amrmos/dealerships-django-app:1.0 link : https://hub.docker.com/r/amrmos/dealerships-django-appdealerships-django-app

2. Express API Server image: docker.io/amrmos/dealerships-backend:1.0 link : https://hub.docker.com/r/amrmos/dealerships-backend

3. MongoDB container image: docker.io/amrmos/dealerships-database:1.0 link : https://hub.docker.com/r/amrmos/dealerships-database

4. Sentiment Analyzer Microservice image: docker.io/amrmos/dealerships-microservice:1.0 link : https://hub.docker.com/r/amrmos/dealerships-microservice

Run all using:
after downloading all images and creating docker-compose.yml file 
```bash
docker-compose up --build

################################ docker-compose.yml##############################

services:
  mongo_db:
    image: mongo:latest
    container_name: dealerships_db
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db

  nodeapp:
    image: dealership_review_nodeapp
    container_name: node_app
    ports:
      - "3030:3030"
    depends_on:
      - mongo_db

  django:
    image: djangoapp
    container_name: django_app
    ports:
      - "8000:8000"
    depends_on:
      - nodeapp
      - mongo_db
  sentiment_analyzer:
    image: senti_analyzer  # Replace with your actual image name
    container_name: sentiment_analyzer
    ports:
      - "5001:5000"   # Example port if your analyzer exposes one
    # add volumes, env, etc. as needed

volumes:
  mongo_data: {}
###########################################################################################