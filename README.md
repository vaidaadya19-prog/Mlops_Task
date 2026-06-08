# Mlops_Task
# MLOps Batch Job – Rolling Mean Signal Generator

## Overview

This project implements a production-style MLOps batch processing pipeline that generates trading signals from OHLCV market data using a rolling mean strategy. The application validates configuration files and datasets, processes time-series data, computes performance metrics, and produces structured outputs suitable for monitoring and deployment workflows.

The pipeline is fully containerized with Docker, supports configurable parameters through YAML files, maintains detailed execution logs, and provides robust error handling for reliable execution in production environments.

---

## Key Features

* Configuration-driven execution using YAML
* Automated dataset validation
* Rolling mean calculation on closing prices
* Binary signal generation based on market trends
* Structured JSON metrics reporting
* Comprehensive logging system
* Dockerized deployment
* Reproducible execution using random seeds
* Graceful error handling and reporting

---

## Project Structure

```text
mlops-task/
│
├── run.py              # Main pipeline script
├── config.yaml         # Runtime configuration
├── data.csv            # Input OHLCV dataset
├── requirements.txt    # Python dependencies
├── Dockerfile          # Container definition
├── metrics.json        # Output metrics
├── run.log             # Execution logs
└── README.md           # Project documentation
```

---

## Technology Stack

* Python 3.9+
* Pandas
* NumPy
* PyYAML
* Docker

---

## Workflow

### Step 1: Configuration Loading

The pipeline loads parameters from `config.yaml` and validates:

* Seed value
* Rolling window size
* Pipeline version

### Step 2: Dataset Validation

The CSV dataset is checked for:

* File existence
* Non-empty content
* Presence of the `close` column
* Valid data format

### Step 3: Rolling Mean Computation

A moving average is calculated using the configured window size.

```python
rolling_mean = close.rolling(window=window).mean()
```

### Step 4: Signal Generation

Trading signals are generated according to:

```text
Signal = 1  → Close Price > Rolling Mean
Signal = 0  → Close Price ≤ Rolling Mean
```

### Step 5: Metrics Generation

The pipeline computes:

* Rows processed
* Signal rate
* Execution latency
* Pipeline version
* Execution status

---

## Configuration

### `config.yaml`

```yaml
seed: 42
window: 5
version: "v1"
```

| Parameter | Type    | Description                     |
| --------- | ------- | ------------------------------- |
| seed      | Integer | Random seed for reproducibility |
| window    | Integer | Rolling mean window size        |
| version   | String  | Pipeline version identifier     |

---

## Installation

### Clone the Repository

```bash
git clone <repository-url>
cd mlops-task
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Running the Pipeline

### Local Execution

```bash
python run.py \
  --input data.csv \
  --config config.yaml \
  --output metrics.json \
  --log-file run.log
```

---

## Docker Deployment

### Build Image

```bash
docker build -t mlops-task .
```

### Run Container

```bash
docker run --rm mlops-task
```

### Save Outputs to Host Machine

```bash
mkdir output

docker run --rm \
  -v "$(pwd)/output:/app" \
  mlops-task
```

Generated files:

```text
output/
├── metrics.json
└── run.log
```

---

## Sample Output

### Metrics File

```json
{
  "version": "v1",
  "rows_processed": 9996,
  "metric": "signal_rate",
  "value": 0.4991,
  "latency_ms": 18,
  "seed": 42,
  "status": "success"
}
```

### Error Output

```json
{
  "version": "v1",
  "status": "error",
  "error_message": "Description of the failure",
  "latency_ms": 5
}
```

---

## Logging

Execution details are written to `run.log`.

Example:

```text
2026-06-01T10:00:00 [INFO] Loading configuration
2026-06-01T10:00:01 [INFO] Dataset loaded
2026-06-01T10:00:01 [INFO] Rolling mean computed
2026-06-01T10:00:01 [INFO] Metrics written successfully
```

---

## Validation & Error Handling

The pipeline automatically detects:

* Missing configuration files
* Invalid configuration parameters
* Missing datasets
* Malformed CSV files
* Empty datasets
* Missing `close` column
* Runtime processing errors

Structured error reports are generated even when execution fails.

---

## Reproducibility

To ensure consistent results across executions, the pipeline initializes NumPy using a configurable random seed:

```python
np.random.seed(seed)
```

This guarantees deterministic behavior for the same dataset and configuration.

---

## Learning Outcomes

This project demonstrates core MLOps concepts including:

* Data validation
* Configuration management
* Logging and monitoring
* Batch processing pipelines
* Containerization with Docker
* Reproducible machine learning workflows
* Production-ready error handling

---

## Author

**Aadya Vaid**
B.Tech Computer Science Engineering (AI/ML)

---
