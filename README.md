# 📜 LogStream - Real-Time Log Aggregation & Alerting (Async)
Python · AsyncIO · Pandas · Matplotlib  

LogStream is a Python-based asynchronous log aggregation and alerting simulator.  
It generates synthetic logs from multiple services, applies regex-based alert rules, stores events, and visualizes log rates in real time.  

It’s designed for **DevOps monitoring, alerting demos, and log stream processing experiments** without needing external brokers like Kafka.

-----------------

## 🛠 Tech & Languages

| Layer        | Tech       | Notes                                                |
|--------------|------------|------------------------------------------------------|
| Language     | Python 3.10+ | Async programming with `asyncio`                   |
| Data         | Pandas     | Event aggregation, CSV export                       |
| Charts       | Matplotlib | Log volume visualization (by severity over time)    |
| Regex Rules  | re (stdlib)| Simple but powerful alerting engine                 |
| Runtime      | Colab/Local| Works in notebooks or local Python environments     |

-------

## 🌐 Architecture

**Flow**

1. **Producers**: Asynchronous coroutines simulate logs from different sources (API, Payments, DB).  
2. **Queue**: Events are pushed into an in-memory `asyncio.Queue`.  
3. **Consumer**: Reads logs from the queue, applies regex-based alert rules, and stores events.  
4. **Storage**: Events are appended to a DataFrame and exported to CSV.  
5. **Visualization**: A time-series chart (PNG) shows log rate per severity level.  

**Diagram**

┌─────────────┐ log events ┌─────────────┐
│ Producers │ ───────────▶ │ Queue │
│ (api, db) │ │ asyncio │
└─────────────┘ └─────┬───────┘
│
consume│apply rules
▼
┌─────────────┐
│ Consumer │
│ Alerts + DF │
└─────┬───────┘
│
┌────────────▼──────────┐
│ Storage & Visualization│
│ CSV + PNG (rates) │
└───────────────────────┘

yaml
Copy code

---

## 📦 Repository Structure

logstream/
├─ app/
│ ├─ logstream.py # Producers, consumer, alert rules
│ └─ rules.py # Regex alert definitions
├─ outputs/
│ ├─ logs.csv # Event dump
│ └─ log_rates.png # Rate chart
├─ tests/
│ └─ test_rules.py # Basic rule checks
├─ requirements.txt
└─ README.md

yaml
Copy code

---

## ▶️ Run in Google Colab

Paste the **single code cell** into Colab and run:

```python
# Run the demo
await run_demo()
Outputs:

logs.csv → All captured events

log_rates.png → Chart of events per log level

Console → Sample of events + triggered alerts

🔗 Event Sources & Rules
Sources: api, payments, db

Log levels: INFO, DEBUG, WARN, ERROR

Messages: login events, cache hits/misses, DB timeouts, payment failures, etc.

Alert Rules (regex-based):

Rule Name	Pattern	Alert Level
Errors	\bERROR\b	CRITICAL
ServiceUnavailable	service unavailable	WARN
HighLatency	latency high	WARN
DBTimeout	db timeout	WARN
PaymentFailures	payment failed	WARN

📊 Outputs
Sample CSV (logs.csv):

ts	source	level	message
14:32:15	api	INFO	user login ok
14:32:15	db	ERROR	db timeout
14:32:16	payments	WARN	payment failed

Sample Chart (log_rates.png):

Time-series stacked line chart

X-axis: seconds (timestamps)

Y-axis: count of events

Lines: INFO, DEBUG, WARN, ERROR

🧪 Tests
Run with:

bash
Copy code
pytest -q
Included tests:

Rule pattern matching

Alert generation

Log event formatting

🐳 Docker
Build and run:

bash
Copy code
docker build -t logstream:latest .
docker run -it logstream:latest
In Docker, events and alerts will print to stdout, and CSV/PNG will be written inside the container.

☸️ Kubernetes (Optional Extension)
To deploy as a service in Kubernetes:

Wrap the consumer in a REST API (FastAPI/Flask)

Expose /alerts and /events endpoints

Deploy with a Deployment + Service

Export CSV/PNG to persistent volume or object storage

🔐 Production Notes
Replace synthetic logs with real log sources (Kafka, Loki, Fluentd)

Send alerts to Slack, PagerDuty, or email

Use Elasticsearch for full-text indexing

Add retention policies for log volume

👤 Author
Siddharth Raut — DevOps & Cloud Engineer
