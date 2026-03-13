# PlantPilot – Smart Irrigation Monitoring Stack (Docker)

## Command to start the project

```bash
chmod +x setup.sh
./setup.sh

WE NEED TO PUT A GENERIC DESCRIPTION HERE

System Overview

The Docker stack contains the following components:

Component	Purpose
Mosquitto MQTT Broker	Message bus for sensor data
Flask API Server	Backend application handling MQTT messages and API endpoints
MariaDB	Database storing irrigation and sensor data
Prometheus	Scrapes metrics from Flask
Grafana	Visualizes metrics in dashboards
Docker Compose	Runs the entire stack
Architecture
Sensor Publisher
        │
        ▼
   MQTT Broker (Mosquitto)
        │
        ▼
   Flask Backend
   ├─ stores readings in MariaDB
   ├─ exposes metrics (/metrics)
   └─ serves web dashboard
        │
        ▼
    Prometheus
        │
        ▼
     Grafana
        │
        ▼
 Embedded in Flask Web UI
Project Layout

From the project root (~/Capstone_Project):

Capstone_Project/
│
├── setup.sh                # Main setup + run script
├── docker-compose.yml      # GENERATED – do not edit manually
├── prometheus.yml          # GENERATED – do not edit manually
├── .env                    # Environment variables for services (Use this for things we want to be secured.)
│
├── config/
│   └── stack_config.py     # Central configuration for services (Use this for general configuration.)
│
├── docker/
│   └── make_yaml.py        # Generates docker-compose.yml & prometheus.yml
│
├── mosquitto/
│   ├── config/
│   │   └── mosquitto.conf  # MQTT broker configuration
│   ├── data/               # Persistent MQTT storage
│   └── log/                # MQTT logs
│
├── database/
│   └── init/
│       ├── schema.sql      # Database schema
│       └── setup_db.sh     # Database initialization script
│
├── backend/
    ├── app.py              # Flask server (This is just the test script)
    ├── sensor_simulator.py # Generates random sensor data (This is for testing stuff without hardware)
    ├── requirements.txt    # Python dependencies
    └── Dockerfile          # Don't change this only change requirements.txt
Services
Flask API

Provides:

Web interface

REST API endpoints

Prometheus metrics (/metrics)

MQTT subscriber for receiving sensor data

Database interaction

Default address:

http://localhost:5000
http://localhost:5000/metrics

Use /metrics to view the scraped Prometheus metrics.

MariaDB

Stores system data including:

Users

Gardens

Sensors

Sensor readings

Irrigation schedules

AI agent decisions

Irrigation events

Port:

3306
Mosquitto MQTT Broker

Handles sensor message delivery to the Flask backend.

Port:

1883

Example topic:

plantpilot/sensors
Prometheus

Collects metrics exposed by Flask.

Example metrics:

soil_moisture_percent
temperature_fahrenheit
humidity_percent

Prometheus UI:

http://localhost:9090
http://localhost:9090/targets

Use /targets to view the targets of web scraping.

Grafana

Visualizes metrics collected by Prometheus.

Dashboards include:

Soil moisture gauge

Temperature time series

Humidity time series

Grafana UI:

http://localhost:3000
Setup

Go to Connections

Search for Prometheus

Add a new datasource

Set the URL to:

http://prometheus:9090

Then create dashboards using the datasource.

Once you finish the dashboard:

Click the three dots

Click Share

Select Embed

Copy the iframe

Paste it into your HTML page

You may need to adjust:

Refresh rate

Time range

Default login:

username: admin
password: admin
Sensor Simulation

The sensor_simulator.py script simulates sensors using random values.

Example data published to MQTT:

{
  "soil_moisture": 47.2,
  "temperature": 78.1,
  "humidity": 55.4
}

Published every few seconds to:

plantpilot/sensors
Prometheus Metrics

Flask exposes metrics at:

http://localhost:5000/metrics

Example output:

soil_moisture_percent 47.2
temperature_fahrenheit 78.1
humidity_percent 55.4

Prometheus scrapes these values automatically.

Starting the System

Run:

chmod +x setup.sh
./setup.sh

The setup script will:

Install Docker (if needed)

Install Python dependencies onto the flask-server container

Generate docker-compose.yml

Generate prometheus.yml

Create default configuration files if missing

Start all containers

Initialize the database

Stopping the System

To stop all services:

sudo docker compose -f docker-compose.yml down

To remove volumes (the data stored by the containers) as well:

sudo docker compose -f docker-compose.yml down -v
Development Notes
Regenerating configuration

If the configuration changes:

python3 docker/make_yaml.py

or bring down the services and rerun the setup script.

Viewing container logs
docker logs flask-server
docker logs mariadb
docker logs mqtt-broker
docker logs prometheus
docker logs grafana
Viewing running containers
docker ps -a
Technologies Used

Python

Flask

MariaDB

Docker

Mosquitto MQTT

Prometheus

Grafana

OpenAI API (for decision agent)
