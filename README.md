
# Stock Market Real-Time Data Analysis Using Kafka

## Overview
This project demonstrates an end-to-end data engineering solution for real-time stock market data analysis using Apache Kafka, Python, and Amazon Web Services (AWS). It leverages Kafka to stream stock market data, processes the data, and stores it in AWS S3 for further analysis using AWS Glue, AWS Athena, and SQL queries. The project is ideal for learning real-time data pipelines, distributed messaging systems, and cloud-based data processing.

## Features
- Real-time streaming of stock market data using Apache Kafka.
- Data ingestion and production using Python and the `kafka-python` library.
- Data consumption and storage in AWS S3 using `s3fs`.
- Data cataloging and querying using AWS Glue Crawler, Glue Data Catalog, and Amazon Athena.
- Scalable architecture hosted on AWS EC2.

## Project Architecture
The architecture of this project is depicted below:

![alt](/architecture.png)

### Flow Description:
1. **Producer**: A Python script reads stock market data (e.g., from a CSV file) and sends it to a Kafka topic using `kafka-python`.
2. **Kafka on EC2**: Apache Kafka, hosted on an AWS EC2 instance, manages the real-time data stream, distributing messages to consumers.
3. **Consumer**: Another Python script consumes the Kafka messages, processes them, and stores them as JSON files in an AWS S3 bucket using `s3fs`.
4. **AWS Services**: 
   - **AWS Glue Crawler**: Automatically discovers and catalogs the data stored in S3.
   - **AWS Glue Data Catalog**: Maintains metadata for the data, enabling querying.
   - **Amazon Athena**: Allows SQL-based querying of the S3 data for analysis.

## Technologies Used
- **Programming Language**: Python
- **Amazon Web Services (AWS)**:
  - S3 (Simple Storage Service)
  - Athena
  - Glue Crawler
  - Glue Data Catalog
  - EC2 (Elastic Compute Cloud)
- **Apache Kafka**: For real-time data streaming and messaging.
- **Libraries**:
  - `kafka-python`: For Kafka producer and consumer implementations.
  - `s3fs`: For interacting with AWS S3.
  - `pandas`: For data manipulation and CSV handling.


## Installation

### 1. Set Up Your Environment
Clone this repository to your local machine or EC2 instance:
```bash
git clone <your-repo-url>
cd Stock-Market-Real-Time-Data-Analysis-Using-Kafka
```

### 2. Install Required Python Packages
Install the necessary Python libraries using `pip`:
```bash
pip install kafka-python s3fs pandas
```

### 3. Set Up AWS Credentials
Configure your AWS credentials to interact with S3, EC2, Glue, and Athena:
- Use the AWS CLI to configure credentials:
  ```bash
  aws configure
  ```
- Provide your AWS Access Key ID, Secret Access Key, region, and output format.

### 4. Set Up Apache Kafka on AWS EC2
Follow these steps to install and configure Kafka on your EC2 instance:

#### a. Install Java (Required for Kafka)
Ensure Java 17 is installed on your EC2 instance:
```bash
sudo yum install java-17-amazon-corretto
java -version
```

#### b. Download and Extract Kafka
Download and extract Kafka on your EC2 instance:
```bash
wget https://downloads.apache.org/kafka/3.7.2/kafka_2.13-3.7.2.tgz
tar -xvf kafka_2.13-3.7.2.tgz
cd kafka_2.13-3.7.2
```

#### c. Start ZooKeeper
Open a terminal session on your EC2 instance and start ZooKeeper:
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

#### d. Start Kafka Server
Open a new terminal session (or duplicate the session) and start the Kafka server:
```bash
export KAFKA_HEAP_OPTS="-Xmx512M -Xms128M"
bin/kafka-server-start.sh config/server.properties
```

#### e. Update `server.properties` for Public IP
Edit the `config/server.properties` file to use your EC2 instance's public IP:
```bash
sudo nano config/server.properties
```
Update the `advertised.listeners` property to:
```
advertised.listeners=PLAINTEXT://<Your-EC2-Public-IP>:9092
```
Save and exit the file.

#### f. Create a Kafka Topic
Create a Kafka topic named `demo-test`:
```bash
bin/kafka-topics.sh --create --topic demo-test --bootstrap-server <Your-EC2-Public-IP>:9092 --replication-factor 1 --partitions 1
```

### 5. Prepare Your Data
Place your stock market data (e.g., `indexProcessed.csv`) in the project directory or update the path in the producer script.

### 6. Configure AWS S3 Bucket
- Create an S3 bucket (e.g., `bucket-name-here`) in your AWS account.
- Update the `s3fs` connection in the consumer script with your bucket name:
  ```python
  s3 = S3FileSystem()
  ```
- Ensure the bucket has appropriate permissions for your AWS credentials.

## Usage

### 1. Run the Producer
Start the producer script to send stock market data to the Kafka topic:
```bash
python producer.py
```

### 2. Run the Consumer
Start the consumer script to consume data from Kafka and store it in S3:
```bash
python consumer.py
```

## Troubleshooting
- Ensure your EC2 instance’s security group allows inbound traffic on ports 2181 (ZooKeeper) and 9092 (Kafka).
- Verify your AWS credentials have the necessary permissions for S3, Glue, and Athena.
- Check Kafka logs in the `logs` directory if the server fails to start.

## Why I Built This
I created this project to demonstrate my expertise in real-time data pipelines, cloud computing, and big data processing. It’s a practical application of Kafka and AWS for financial data analysis, showcasing my ability to handle complex, scalable systems.

## Contributing
Contributions are welcome! Please fork this repository, make your changes, and submit a pull request.

## License
This project is licensed under the MIT License.
