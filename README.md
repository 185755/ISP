# Weather Data Analytics for Disaster Management

## Project Overview
This project focuses on analyzing real-time and historical weather data to detect anomalies and predict potential disasters such as heatwaves. The system leverages **Apache Kafka** for data streaming, **Apache Spark** for distributed processing, and **AWS EMR** for scalable infrastructure. The goal is to provide actionable insights for disaster management.

---

## Technologies Used
- **Apache Kafka**: For real-time data streaming.
- **Apache Spark**: For batch/stream processing (MLlib integration was attempted but not completed).
- **AWS EMR**: Hosting Spark clusters and S3 for data storage.
- **Python**: Primary programming language.
- **Open-Meteo API**: Source of real-time and historical weather data.
- 

---

## Project Components Kafka

### 1. Data Ingestion
- **Kafka Producers** (`kafka_producer.py`, `kafka_producer_copy.py`):
  - Stream CSV data or real-time weather data from Open-Meteo to Kafka topics.
  - Example topic: `quickstart-events`.
- **Challenges**: Kafka integration with AWS EMR faced configuration issues (e.g., broker connectivity).

### 2. Data Processing
- **Spark Consumer** (`test.py`):
  - Consumes Kafka messages, calculates min/max/avg for temperature, windspeed, and winddirection.
  - Writes results to `min_max_avg_values.txt`.

### 3. Historical Data Analysis
- **Jupyter Notebooks** (`data_analysis.ipynb`, `data_download.ipynb`):
  - **Data Download**: Fetches hourly temperature data (2021–2025) and uploads to S3 (`sparkcalculations/data/weather_data.csv`).
  - **Heatwave Detection**:
    - Identifies consecutive days with temperatures >25°C using Spark window functions.
    - Groups heatwaves and calculates duration (e.g., a 9-day heatwave in August 2022).

### 4. Predictive Modeling (Not Implemented)
- **Spark MLlib**:
  - Initial data preparation was completed, but model training and deployment were not achieved due to time constraints and technical challenges (e.g., dataset compatibility issues, lack of ML expertise).

## Project Components code on your local device
### 1. Data Ingestion from OpenMeteo API
Retrieving and Utilizing Data from Open-Meteo API

The Open-Meteo API provides a reliable and efficient way to access weather data for various geographic locations. This report outlines the step-by-step process of retrieving, processing, and visualizing weather data using the Open-Meteo API.

1. Understanding the API

Before retrieving data, it is crucial to familiarize oneself with the Open-Meteo API documentation. This documentation provides an overview of the available endpoints, parameters, and types of data accessible through the API. Understanding these aspects ensures the correct construction of API requests.

2. Building the API Request URL

To retrieve weather data, one needs to construct an appropriate API request URL. The URL should include essential parameters such as latitude, longitude, and the specific weather variables required.

3. Making the API Request

Using a programming language such as Python, one can send an HTTP GET request to the Open-Meteo API. Python’s requests library is particularly suitable for this task. The request should be sent to the constructed API URL, and the response will typically be in JSON format.

4. Processing the Response Data

Once the response is received from the API, it is typically in JSON format. The next step involves parsing this JSON data to extract the relevant information. Using a data processing library like pandas, one can convert the data into a DataFrame for easier manipulation and analysis.

5. Displaying the Data

After processing the data, it is essential to present it in a user-friendly format. The pandas library can be used to display the data in a tabular form, and visualization libraries like matplotlib can create visual representations such as plots and charts.

6. Automating the Process

To continuously retrieve and update the weather data at regular intervals, automation is key. This can be achieved using a timer or scheduler, which ensures that the data is periodically fetched, processed, and displayed.

By following these steps, one can efficiently retrieve, process, and visualize weather data from the Open-Meteo API in real-time. This process ensures that the data is continuously updated and accurately reflects the current weather conditions.

---

## Key Results
1. **Real-time weather streaming**
   - Kafka producer can stream real-time weather data for given location
1. **Real-Time weather statistics**:
   - Consumes Kafka messages, calculates min/max/avg for temperature, windspeed, and winddirection.

2. **Heatwave Analysis**:
   - Identified 12 heatwave events (3–9 days long) between 2021–2024.
   - Example output:
     ```
     +----------+----------+----------+-------------+
     |wave_group|start_date|end_date  |duration_days|
     +----------+----------+----------+-------------+
     |19        |2022-08-12|2022-08-20|9            |
     |32        |2023-09-08|2023-09-13|6            |
     +----------+----------+----------+-------------+
     ```

---

## Challenges
1. **Kafka on AWS EMR**:
   - Broker configuration issues prevented seamless integration.
   - Workaround: Local Kafka cluster used for testing.
2. **Spark MLlib**:
   - Insufficient time and technical hurdles (e.g., feature engineering, hyperparameter tuning) delayed model implementation.
3. **Data Latency**:
   - Open-Meteo API delays affected real-time stream consistency.

---

## Future Improvements
1. **Spark MLlib Integration**:
   - Collaborate with data scientists to design regression models for temperature forecasting.
   - Expand datasets to include precipitation for flood prediction.
2. Deploy Kafka on EMR using custom bootstrap scripts.

---

## How to Run AWS EMR part
### Prerequisites
- AWS EMR cluster with Spark.
- Python 3.8+, `confluent-kafka`, `pyspark`, `boto3`.

### Steps
1. **Data Download**:
   ```bash
   # Run data_download.ipynb to populate S3 with weather data.

2. **Data Analysis**:
   ```bash
   # Run data_analysis.ipynb to analyse data set downloaded in previous step

## How to Run local Kafka park
### Prerequisites
- Apache Kafka and apache Spark
- Python 3.8+

### Steps
1. **Initialize Zookeper**
   ```bash
   # Run bin/zookeeper-server-start.sh config/zookeeper.properties in terminal inside folder with Kafka to initialize Zookeper


2. **Initialize Kafka**
   ```bash
   # Run bin/kafka-server-start.sh config/server.properties in terminal inside folder with Kafka to initialize Kafka

3. **Create topic for Kafka**
    ```bash
    # Run bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092 to create topic which will be used to comunicate betwean producer and consumer
4. **Initialize consumer and producer**
   ```bash
    # Run python kafka_producer_copy.py to initialize producer which stream real-time data from website or python kafka_producer.py to initialize producer which stram next samples from given csb
    # Run spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.4 test.py to initialize consumer


