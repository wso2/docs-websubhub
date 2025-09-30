# Apache Kafka connection configurations

## Overview

As outlined in the [Introduction](../get-started/introduction.md), **WSO2 Integrator: WebSubHub** supports using a message broker of choice as its persistence storage. Among the popular options, Apache Kafka is widely used. This guide explains the Apache Kafka–related connection configuration options available in **WSO2 Integrator: WebSubHub**.

## Basic configurations

### Connection configurations

To update the basic Apache Kafka connection settings in **WSO2 WebSubHub**, update the following configuration section in the `websubhub-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.config.kafka.connection]
    bootstrapServers = <kafka-bootstrap-nodes>
```

To update the basic Apache Kafka connection settings in **WSO2 WebSubHub Consolidator**, update the following configuration section in the `websubhub-consolidator-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.consolidator.config.kafka.connection]
    bootstrapServers = <kafka-bootstrap-nodes>
```

**Parameters**

* *bootstrapServers* - A comma-separated list of Kafka bootstrap nodes (eg: “localhost:9092,localhost:9093”)

### Consumer configurations

To update the Apache Kafka consumer settings in **WSO2 WebSubHub**, update the following configuration section in the `websubhub-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.config.kafka.consumer]
    maxPollRecords = <maximum-number-of-records-to-fetch-per-poll>
    pollingInterval = <polling-interval-in-seconds>
    gracefulClosePeriod = <graceful-close-period-in-seconds>
```

To update the Apache Kafka consumer settings in **WSO2 WebSubHub Consolidator**, update the following configuration section in the `websubhub-consolidator-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.consolidator.config.kafka.consumer]
    maxPollRecords = <maximum-number-of-records-to-fetch-per-poll>
    pollingInterval = <polling-interval-in-seconds>
    gracefulClosePeriod = <graceful-close-period-in-seconds>
```

**Parameters**

* *maxPollRecords* - Maximum number of records returned in a single poll action from Kafka (eg: 50)
* *pollingInterval* - Interval (in seconds) between consecutive poll requests (eg: 10.0)
* *gracefulClosePeriod* - The time period (in seconds) gracefully shutdown the consumer (eg: 5.0)

## Advance configurations

### Configuring SSL communication

Apache Kafka allows clients to use SSL encryption of traffic as well as authentication. **WSO2 Integrator: WebSubHub** can be configured to use SSL when communicating with Apache Kafka. Please follow the <a href = "https://kafka.apache.org/documentation/#security_ssl">official documentation</a> to understand how to configure SSL encryption of traffic in the broker level.

To set up SSL encryption of traffic for **WSO2 WebSubHub**, update the following configuration section in the `websubhub-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.config.kafka.connection]
    bootstrapServers = <kafka-bootstrap-nodes>
    securityProtocol = "SSL"

    # Option 1: Configure SSL using a truststore
    [websubhub.config.kafka.connection.secureSocket]
    cert.path = <path-to-kafka-client-truststore>
    cert.password = <truststore-password>
    protocol.name = "SSL"

    # Option 2: Configure SSL using the server certificate directly
    [websubhub.config.kafka.connection.secureSocket]
    cert = <path-to-kafka-server-cert>
    protocol.name = "SSL"
```

To set up SSL encryption of traffic for **WSO2 WebSubHub Consolidator**, update the following configuration section in the `websubhub-consolidator-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.consolidator.config.kafka.connection]
    bootstrapServers = <kafka-bootstrap-nodes>
    securityProtocol = "SSL"

    # Option 1: Configure SSL using a truststore
    [websubhub.consolidator.config.kafka.connection.secureSocket]
    cert.path = <path-to-kafka-client-truststore>
    cert.password = <truststore-password>
    protocol.name = "SSL"

    # Option 2: Configure SSL using the server certificate directly
    [websubhub.consolidator.config.kafka.connection.secureSocket]
    cert = <path-to-kafka-server-cert>
    protocol.name = "SSL"
```

**Kafka Connection Parameters**

* *securityProtocol* - Protocol used to communicate with Kafka brokers. Supported values are `PLAINTEXT`, `SSL`, `SASL_PLAINTEXT`, and `SASL_SSL`. Since this setup requires a secure connection, use `SSL`.

**Secure Socket Parameters**

* *cert* - Path to the Kafka broker certificate file (e.g., /home/user/broker.crt)
* *cert.path* - Path to the Kafka client truststore file (e.g., /home/user/client.truststore.jks).
* *cert.password* - Password to access the truststore file.
* *protocol.name* - The relevant security protocol.

**Note: Choose either the truststore-based approach or the certificate-based approach, depending on your setup. Ensure that only one approach is configured at a time.**

### Configuring SSL based authentication

In addition to encrypting traffic, Apache Kafka supports client authentication over SSL, ensuring that only trusted clients can connect to the cluster. **WSO2 Integrator: WebSubHub** can be configured to use SSL certificates for authentication when connecting to Kafka.

To set up SSL based authentication for **WSO2 WebSubHub**, update the following configuration section in the `websubhub-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.config.kafka.connection]
    bootstrapServers = <kafka-bootstrap-nodes>
    securityProtocol = "SSL"

    [websubhub.config.kafka.connection.secureSocket]
    cert.path = <path-to-kafka-client-truststore>
    cert.password = <truststore-password>
    key.keyStore.path = <path-to-kafka-client-keystore>
    key.keyStore.password = <keystore-password>
    protocol.name = "SSL"
```

To set up SSL based authentication for **WSO2 WebSubHub Consolidator**, update the following configuration section in the `websubhub-consolidator-<VERSION>/conf/Config.toml` file.

```toml
    [websubhub.consolidator.config.kafka.connection]
    bootstrapServers = <kafka-bootstrap-nodes>
    securityProtocol = "SSL"

    [websubhub.consolidator.config.kafka.connection.secureSocket]
    cert.path = <path-to-kafka-client-truststore>
    cert.password = <truststore-password>
    key.keyStore.path = <path-to-kafka-client-keystore>
    key.keyStore.password = <keystore-password>
    protocol.name = "SSL"
```

**Secure Socket Parameters**

* *key.keyStore.path* - Path to the Kafka client truststore file (e.g., /home/user/client.keystore.jks).
* *key.keyStore.password* - Password to access the keystore file.
* *key.keyPassword* (optional) - Optional configuration to set the password for the key.
