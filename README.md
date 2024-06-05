### This script will focus on detecting anomalies in volume, freshness, schema, and distribution of data.

```python
import logging
import pandas as pd
from datetime import datetime, timedelta

# Configure logging
logging.basicConfig(filename='anomaly_detection.log', level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s')


def load_data(file_path):
    """
    Load data from file_path.
    """
    try:
        data = pd.read_csv(file_path)
        logging.info(f"Data loaded successfully from {file_path}")
        return data
    except Exception as e:
        logging.error(f"Error loading data from {file_path}: {e}")
        return None


def detect_volume_anomaly(data):
    """
    Detect volume anomaly in data.
    """
    try:
        volume = len(data)
        if volume < 1000 or volume > 100000:
            logging.warning(f"Volume anomaly detected: {volume} records")
        else:
            logging.info(f"Volume within expected range: {volume} records")
    except Exception as e:
        logging.error(f"Error detecting volume anomaly: {e}")


def detect_freshness_anomaly(data):
    """
    Detect freshness anomaly in data.
    """
    try:
        current_date = datetime.now()
        max_date = data['timestamp'].max()
        days_difference = (current_date - max_date).days
        if days_difference > 1:
            logging.warning(f"Freshness anomaly detected: Data is {days_difference} days old")
        else:
            logging.info("Data freshness within expected range")
    except Exception as e:
        logging.error(f"Error detecting freshness anomaly: {e}")


def detect_schema_anomaly(data):
    """
    Detect schema anomaly in data.
    """
    try:
        expected_columns = ['id', 'name', 'age', 'gender']
        missing_columns = set(expected_columns) - set(data.columns)
        if missing_columns:
            logging.warning(f"Schema anomaly detected: Missing columns - {missing_columns}")
        else:
            logging.info("Schema matches expected columns")
    except Exception as e:
        logging.error(f"Error detecting schema anomaly: {e}")


def detect_distribution_anomaly(data):
    """
    Detect distribution anomaly in data.
    """
    try:
        gender_distribution = data['gender'].value_counts(normalize=True)
        if 'male' in gender_distribution and gender_distribution['male'] < 0.4:
            logging.warning("Distribution anomaly detected: Male gender ratio is low")
        else:
            logging.info("Gender distribution within expected range")
    except Exception as e:
        logging.error(f"Error detecting distribution anomaly: {e}")


def main():
    # Load data
    data = load_data("data.csv")
    if data is None:
        return

    # Perform anomaly detection
    detect_volume_anomaly(data)
    detect_freshness_anomaly(data)
    detect_schema_anomaly(data)
    detect_distribution_anomaly(data)


if __name__ == "__main__":
    main()
```

### Explanation:

1. **Logging Configuration:**
   - Configured logging to write to a file (`anomaly_detection.log`) with INFO level logging.
   - Errors and warnings are logged with appropriate messages.

2. **Functional Approach:**
   - Each anomaly detection function (`detect_volume_anomaly`, `detect_freshness_anomaly`, etc.) performs a specific task.

3. **Error Handling:**
   - Try-except blocks are used to catch and log any exceptions that occur during anomaly detection.

4. **Anomaly Detection Functions:**
   - `load_data`: Loads data from a CSV file. If an error occurs, it logs the error.
   - `detect_volume_anomaly`: Detects anomalies in data volume (number of records).
   - `detect_freshness_anomaly`: Detects anomalies in data freshness (timestamp of the latest record).
   - `detect_schema_anomaly`: Detects anomalies in data schema (missing columns).
   - `detect_distribution_anomaly`: Detects anomalies in data distribution (e.g., gender distribution).

5. **Main Function:**
   - Calls the `load_data` function to load the data.
   - Calls each anomaly detection function in sequence.
