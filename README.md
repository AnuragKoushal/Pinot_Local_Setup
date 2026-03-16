# Apache Pinot Local Setup Guide (Mac)

This README documents the commands required to install and run an **Apache Pinot local cluster** for development and testing.

---

# 1. Prerequisites

### Install Java 11

Pinot works best with Java 11.

```bash
brew install --cask temurin@11
```

Verify installation:

```bash
java -version
```

Expected output:

```
openjdk version "11.x"
```

---

# 2. Download Apache Pinot

```bash
cd ~/Downloads

curl -O https://downloads.apache.org/pinot/apache-pinot-1.4.0/apache-pinot-1.4.0-bin.tar.gz

tar -xvf apache-pinot-1.4.0-bin.tar.gz
```

Move into the Pinot directory:

```bash
cd apache-pinot-1.4.0-bin
```

---

# 3. Clean Start (Kill Old Processes)

Before starting Pinot, stop any running instances.

```bash
pkill -f pinot
```

---

# 4. Configure JVM Memory

Minimum safe heap settings for local development.

```bash
export JAVA_OPTS="-Xms128M -Xmx256M"
```

---

# 5. Start Pinot Components

Pinot cluster consists of:

* Zookeeper
* Controller
* Broker
* Server

---

## Start Zookeeper

```bash
JAVA_TOOL_OPTIONS="-XX:+IgnoreUnrecognizedVMOptions" \
./bin/pinot-admin.sh StartZookeeper -zkPort 2191
```

---

## Start Controller

Open a new terminal.

```bash
export JAVA_OPTS="-Xms128M -Xmx256M"

JAVA_TOOL_OPTIONS="-XX:+IgnoreUnrecognizedVMOptions" \
./bin/pinot-admin.sh StartController \
-zkAddress localhost:2191 \
-controllerPort 9000
```

Controller UI:

```
http://localhost:9000
```

---

## Start Broker

Open another terminal.

```bash
export JAVA_OPTS="-Xms128M -Xmx256M"

JAVA_TOOL_OPTIONS="-XX:+IgnoreUnrecognizedVMOptions" \
./bin/pinot-admin.sh StartBroker \
-zkAddress localhost:2191
```

Broker default port:

```
8099
```

---

## Start Server

Open another terminal.

```bash
export JAVA_OPTS="-Xms128M -Xmx256M"

JAVA_TOOL_OPTIONS="-XX:+IgnoreUnrecognizedVMOptions" \
./bin/pinot-admin.sh StartServer \
-zkAddress localhost:2191
```

---

# 6. Verify Pinot Is Running

Check running processes:

```bash
ps -ef | grep pinot
```

Check controller port:

```bash
lsof -i :9000
```

---

# 7. Check Logs (If Something Fails)

Controller logs

```bash
tail -50 controller-console.log
```

Broker logs

```bash
tail -50 broker-console.log
```

Server logs

```bash
tail -50 server-console.log
```

Zookeeper logs

```bash
tail -50 zookeeper-console.log
```

---

# 8. Start Pinot Using QuickStart (Alternative)

This command automatically starts:

* Zookeeper
* Controller
* Broker
* Server
* Sample dataset

```bash
JAVA_TOOL_OPTIONS="-XX:+IgnoreUnrecognizedVMOptions" \
./bin/pinot-admin.sh QuickStart -type batch
```

Access UI:

```
http://localhost:9000
```

---

# 9. Example Pinot Query

Once Pinot is running, open the query console and run:

```sql
SELECT COUNT(*) FROM airlineStats
```

---

# 10. Stop Pinot Cluster

```bash
pkill -f pinot
```

---

# Recommended Minimal Workflow

Start Pinot:

```bash
cd ~/Downloads/apache-pinot-1.4.0-bin

export JAVA_OPTS="-Xms128M -Xmx256M"

JAVA_TOOL_OPTIONS="-XX:+IgnoreUnrecognizedVMOptions" \
./bin/pinot-admin.sh QuickStart -type batch
```

Open UI:

```
http://localhost:9000
```

Stop Pinot:

```bash
pkill -f pinot
```

---

# Notes

• Use **Java 11** for best compatibility.
• Pinot requires more than **100MB heap** to run.
• Minimum recommended heap for local setup is **256MB**.

---
