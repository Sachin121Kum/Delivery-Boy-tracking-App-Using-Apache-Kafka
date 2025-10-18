# Real-Time Location Tracking App

A microservices-based real-time location tracking application built with Spring Boot and Apache Kafka. This project consists of two main services: a **Delivery Boy Service** that produces location updates and an **End User Service** that consumes and displays these updates.

**Author**: Sachin Kumar

## 🏗️ Architecture Overview

The application follows a microservices architecture with event-driven communication:

```
┌─────────────────┐    Kafka     ┌─────────────────┐
│  Delivery Boy   │ ──────────► │   End User      │
│   Service       │   Topic     │   Service       │
│   (Producer)    │             │  (Consumer)     │
└─────────────────┘             └─────────────────┘
     Port: 8090                      Port: 8091
```

## 🚀 Features

- **Real-time Location Updates**: Delivery boys can send location coordinates
- **Event-Driven Architecture**: Uses Apache Kafka for reliable message streaming
- **Microservices Design**: Separate services for producers and consumers
- **Scalable**: Can handle multiple delivery boys and end users
- **RESTful API**: Simple HTTP endpoints for location updates

## 📁 Project Structure

```
tracking-app/
├── deliveryboy/                 # Delivery Boy Service (Producer)
│   └── deliveryboy/
│       ├── src/main/java/com/deliveryboy/
│       │   ├── config/
│       │   │   ├── AppConstants.java      # Kafka topic configuration
│       │   │   └── KafkaConfig.java       # Kafka producer setup
│       │   ├── controller/
│       │   │   └── LocationController.java # REST API endpoints
│       │   ├── service/
│       │   │   └── KafkaService.java      # Kafka message publishing
│       │   └── DeliveryboyApplication.java # Main application class
│       ├── src/main/resources/
│       │   └── application.properties     # Service configuration
│       └── pom.xml                        # Maven dependencies
└── enduser/                     # End User Service (Consumer)
    └── enduser/
        ├── src/main/java/com/enduser/
        │   ├── AppConstant.java           # Consumer constants
        │   ├── KafkaConfig.java           # Kafka consumer setup
        │   ├── kafkaConsumer.java         # Message consumer logic
        │   └── EnduserApplication.java    # Main application class
        ├── src/main/resources/
        │   └── application.properties     # Service configuration
        └── pom.xml                        # Maven dependencies
```

## 🛠️ Technology Stack

- **Java 21**
- **Spring Boot 3.5.6**
- **Apache Kafka** (Message Broker)
- **Maven** (Build Tool)
- **Spring Kafka** (Kafka Integration)

## 📋 Prerequisites

Before running the application, ensure you have:

- **Java 21** or higher
- **Apache Kafka** running on localhost:9092
- **Maven 3.6+** (for building the project)

## 🚀 Getting Started

### 1. Start Apache Kafka

First, start your Kafka server:

```bash
# Navigate to Kafka directory
cd kafka_2.13-2.8.0

# Start Zookeeper
.\bin\windows\zookeeper-server-start.bat config\zookeeper.properties

# Start Kafka Server (in a new terminal)
.\bin\windows\kafka-server-start.bat config\server.properties
```

### 2. Build and Run the Services

#### Delivery Boy Service (Producer)

```bash
cd deliveryboy/deliveryboy
mvn clean install
mvn spring-boot:run
```

The service will start on **http://localhost:8090**

#### End User Service (Consumer)

```bash
cd enduser/enduser
mvn clean install
mvn spring-boot:run
```

The service will start on **http://localhost:8091**

## 🔄 Application Flow

### Complete Workflow

1. **Service Startup**
   - Both services start and connect to Kafka
   - Kafka topic `location-update-topic` is automatically created
   - End User service subscribes to the topic

2. **Location Update Process**
   ```
   Delivery Boy App → LocationController → KafkaService → Kafka Topic
                                                           ↓
   End User App ← KafkaConsumer ← KafkaConfig ← Kafka Topic
   ```

3. **Real-time Tracking**
   - Delivery boy sends location updates via REST API
   - Location data is published to Kafka topic
   - End user service consumes and processes location updates
   - Real-time location tracking is achieved

### API Endpoints

#### Delivery Boy Service

- **POST** `/location/update`
  - **Description**: Sends location updates to Kafka
  - **Response**: `{"Message": "Location Updated"}`
  - **Behavior**: Generates 10,000,000 random location coordinates and publishes them to Kafka

#### End User Service

- **Consumer**: Automatically consumes messages from `location-update-topic`
- **Output**: Prints received location coordinates to console

## 🔧 Configuration

### Delivery Boy Service Configuration

```properties
spring.application.name=deliveryboy
server.port=8090
spring.kafka.producer.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

### End User Service Configuration

```properties
spring.application.name=enduser
server.port=8091
spring.kafka.consumer.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=group-1
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.consumer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

## 🧪 Testing the Application

1. **Start both services** as described above
2. **Send a location update**:
   ```bash
   curl -X POST http://localhost:8090/location/update
   ```
3. **Check the End User service console** to see the consumed location updates

## 📊 Monitoring

- **Delivery Boy Service**: Check console for published message count
- **End User Service**: Monitor console for consumed location coordinates
- **Kafka**: Use Kafka tools to monitor topic activity

## 🔍 Key Components

### Delivery Boy Service

- **LocationController**: REST endpoint for location updates
- **KafkaService**: Handles message publishing to Kafka
- **AppConstants**: Defines Kafka topic name
- **KafkaConfig**: Configures Kafka producer

### End User Service

- **KafkaConfig**: Configures Kafka consumer with `@KafkaListener`
- **AppConstant**: Defines topic name and consumer group
- **Consumer Logic**: Processes incoming location updates

## 🚀 Future Enhancements

- [ ] Add database persistence for location history
- [ ] Implement WebSocket for real-time UI updates
- [ ] Add authentication and authorization
- [ ] Create web dashboard for tracking
- [ ] Add location validation and filtering
- [ ] Implement delivery status tracking
- [ ] Add mobile app integration

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🆘 Troubleshooting

### Common Issues

1. **Kafka Connection Failed**
   - Ensure Kafka is running on localhost:9092
   - Check if Zookeeper is running

2. **Port Already in Use**
   - Change ports in application.properties
   - Kill existing processes using the ports

3. **Maven Build Issues**
   - Ensure Java 21 is installed
   - Check Maven version compatibility

## 📞 Support

For support and questions, please contact:
- **Email**: sk31817@gmail.com
- **Issues**: Open an issue in the repository

---

**Happy Tracking! 🚚📍**
