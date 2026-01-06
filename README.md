# Telegraf JMX Monitoring via Jolokia Proxy

A Docker Compose example demonstrating how to collect JVM metrics from a Java application using Telegraf and Jolokia proxy.

## Architecture

```
┌─────────────┐     JMX/RMI      ┌─────────────┐    HTTP/JSON    ┌───────────┐
│ jmx-target  │◄────────────────►│  Jolokia    │◄───────────────►│ Telegraf  │
│ (Java App)  │     :9999        │  (Proxy)    │     :8080       │           │
└─────────────┘                  └─────────────┘                 └───────────┘
```

## Components

| Service | Description |
|---------|-------------|
| **jmx-target** | Sample Java application with JMX remote access enabled on port 9999 |
| **jolokia** | Jolokia proxy (Tomcat + WAR) that bridges JMX to HTTP/JSON |
| **telegraf** | Metrics collector using `jolokia2_proxy` input plugin |

## Collected Metrics

- **jvm_runtime** - Uptime, VM info
- **jvm_memory** - Heap and non-heap memory usage
- **jvm_threading** - Thread counts
- **jvm_classloading** - Loaded/unloaded classes
- **jvm_gc** - Garbage collector stats (per collector)
- **jvm_memory_pool** - Memory pool usage (per pool)
- **jvm_os** - CPU load, file descriptors, system memory

## Usage

```bash
# Start all services
docker compose up -d

# View metrics (Telegraf outputs to stdout)
docker compose logs -f telegraf

# Stop all services
docker compose down
```

## Configuration

- `telegraf/telegraf.conf` - Telegraf input/output configuration
- `jolokia/Dockerfile` - Jolokia proxy setup (v2.4.2)
- `jmx-target/Dockerfile` - Sample JMX-enabled Java app

## References

- [Telegraf jolokia2_proxy plugin documentation](https://github.com/influxdata/telegraf/blob/master/plugins/inputs/jolokia2_proxy/README.md)
- [Jolokia Proxy Mode](https://jolokia.org/features/proxy.html)

## Adapting for Your Application

1. Replace `jmx-target` with your Java application
2. Update the target URL in `telegraf.conf`:
   ```toml
   [[inputs.jolokia2_proxy.target]]
     url = "service:jmx:rmi:///jndi/rmi://your-host:your-port/jmxrmi"
   ```
3. Add custom MBean metrics as needed
