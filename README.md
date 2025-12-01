# WeatherBoard 
**Educational .NET 10 + Docker micro-architecture demonstrating Blazor, APIs, event-driven workers and container orchestration.**

WeatherBoard is a compact but realistic learning project showing how small distributed systems can be built with modern .NET and Docker.  
It ingests weather updates, processes them asynchronously and visualizes trends in a clean Blazor dashboard.

---

## Goals of the Project

- Demonstrate modern **.NET 10** architecture  
- Show **event-driven communication** using a lightweight message bus  
- Practice **containerized development** with Docker  
- Provide a simple, understandable example of  
  - Web client  
  - API  
  - Background worker  
  - Message queue  
  - Persistent storage  
- Serve as a strong GitHub portfolio project

---
## High-Level Architecture

       +---------------------+
       |  Blazor WASM UI     |
       | WeatherBoard.Client |
       +----------+----------+
                  |
                  | HTTP
                  v
          +--------------+
          |  API Layer   |
          | WeatherBoard |
          |     .Api     |
          +------+-------+
                 | Publishes events
                 v
         +------------------+
         |   Redis Bus      |
         |  (Pub / Sub)     |
         +--------+---------+
                  |
                  | Subscribes
                  v
       +-----------------------+
       | Background Worker     |
       | WeatherBoard.Worker   |
       +-----------+-----------+
                   |
                   v
         +--------------------+
         |  PostgreSQL DB     |
         +--------------------+

---

## Event-Driven Flow

1. Client (Blazor) calls API to submit or request weather updates  
2. API validates and stores incoming data  
3. API publishes an **UpdateReceived** event to Redis  
4. Worker subscribes, processes updates and computes derived trends  
5. Worker writes aggregated data to PostgreSQL  
6. Client fetches processed results from the API  

---

## Components

### **WeatherBoard.Client (Blazor WebAssembly)**  
- Renders dashboard UI  
- Fetches processed weather data from API  
- Optional SignalR auto-refresh  
- Lightweight static client  
- Written in .NET 10, C# 14

### **WeatherBoard.Api (ASP.NET Core Minimal API)**  
- Receives raw weather updates  
- Performs input-level validation  
- Stores initial data  
- Publishes events to Redis  
- Serves the Blazor WASM

### **WeatherBoard.Worker (Background Service)**  
- Listens to Redis Pub/Sub  
- Processes weather events  
- Detects trends (temp rise, humidity spikes, storm warnings)  
- Writes results to PostgreSQL  

### **Message Bus (Redis)**  
- Lightweight pub/sub  
- Ideal for demos  
- Zero-config

### **Database (PostgreSQL)**  
- Stores raw + processed weather data  
- Accessed by API + Worker

---

## Docker & Orchestration

Run everything with one command: 
docker compose up --build

Stop:
docker compose down