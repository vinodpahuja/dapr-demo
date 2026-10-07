# Dapr Demo

This repository contains a small Dapr-based demo that shows how to trigger periodic processing with a cron binding and process batches from a JSON file using both Java and JavaScript implementations.

It demonstrates the core idea of using Dapr bindings to integrate applications with external systems without hard-coding infrastructure details into the application logic.

## Overview

The project includes:

- A Dapr cron input binding configured in `components/binding-cron.yaml`
- A Java Spring Boot sample in `dapr-java-demo`
- A JavaScript sample in `dapr-js-demo`
- A sample order payload file used by the batch processing logic

## Repository Structure

```text
.
├── LICENSE
├── README.md
├── dapr-demo.txt
├── components
│   └── binding-cron.yaml
├── dapr-java-demo
│   ├── pom.xml
│   └── src
│       └── main
│           ├── java
│           │   └── rnd
│           │       └── dapr
│           │           ├── App.java
│           │           └── Controller.java
│           └── resources
│               └── orders.json
└── dapr-js-demo
    ├── index.js
    ├── orders.json
    └── package.json
```

## What the Demo Does

The cron binding runs on a schedule and triggers the application endpoint periodically. The application then reads order data from a JSON file and prepares SQL insert statements for each order.

This is a basic demonstration of the Dapr input binding pattern:

- `cron` triggers the flow on a recurring schedule
- The application loads `orders.json`
- It generates SQL commands for each order
- The same pattern can be extended to call Dapr output bindings or services

## Dapr Components

The component configuration is defined in `components/binding-cron.yaml`:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: cron
  namespace: quickstarts
spec:
  type: bindings.cron
  version: v1
  metadata:
  - name: schedule
    value: "@every 10s"
  - name: direction
    value: "input"
```

This schedules the binding to trigger every 10 seconds.

## Java Demo

The Java project is a Spring Boot application that exposes the Dapr binding endpoint and processes incoming cron events.

### Java project details

- Framework: Spring Boot 2.6.x
- Java version: 17
- Dapr SDK: `io.dapr:dapr-sdk-springboot` and `io.dapr:dapr-sdk`

### Run Java application

From the repository root:

```bash
cd dapr-java-demo
mvn spring-boot:run
```

Then run Dapr with the component path:

```bash
dapr run --app-id batch-sdk --app-port 8080 --resources-path ../components
```

## JavaScript Demo

The JavaScript sample shows a Dapr server using the Node.js SDK with a cron binding.

### JavaScript project details

- Runtime: Node.js
- Dapr SDK: `@dapr/dapr`
- Additional dependency: `axios`

### Run JavaScript application

From the repository root:

```bash
cd dapr-js-demo
npm install
node index.js
```

Then run Dapr with:

```bash
dapr run --app-id batch-sdk --app-port 5005 --resources-path ../components
```

## Sample Data

The sample order list is stored in `orders.json` and contains example records such as order IDs, customer names, and prices.

## Prerequisites

Before running the demo, make sure you have:

- Java 17+
- Maven
- Node.js and npm
- Dapr installed and initialized locally
- A running Dapr sidecar setup for self-hosted mode

## Notes

This repo is intended as a lightweight reference implementation for experimenting with:

- Dapr bindings
- periodic scheduled input triggers
- Java and JavaScript integration patterns
- sample batch processing logic

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.
