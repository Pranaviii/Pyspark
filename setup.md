# PySpark Setup on macOS

## Step 1: Navigate to Spark Folder

```bash
cd ~/Desktop/Data\ engineernig/spark
```

---

# Step 2: Create Virtual Environment

Check Python version:

```bash
python3 --version
```

Create virtual environment:

```bash
python3 -m venv .pyspark-env
```

---

# Step 3: Activate Virtual Environment

```bash
source .pyspark-env/bin/activate
```

After activation, terminal should look like:

```bash
(.pyspark-env) Pranavis-MacBook-Pro:spark pnarukullas25$
```

---

# Step 4: Install PySpark

```bash
pip install pyspark
```

---

# Step 5: Install FindSpark

```bash
pip install findspark
```

---

# Step 6: Install JupyterLab

```bash
pip install jupyterlab
```

---

# Step 7: Install Java 17 (Important)

Spark does not work properly with Java 26.

Install Java 17 using Homebrew:

```bash
brew install openjdk@17
```

---

# Step 8: Set JAVA_HOME

Open `.zshrc`:

```bash
nano ~/.zshrc
```

Add these lines at the bottom:

```bash
export JAVA_HOME=/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
```

Save and exit:
- CTRL + O
- Enter
- CTRL + X

Reload terminal configuration:

```bash
source ~/.zshrc
```

Verify Java version:

```bash
java -version
```

Expected output:

```bash
openjdk version "17..."
```

---

# Step 9: Start JupyterLab

Activate virtual environment first:

```bash
source .pyspark-env/bin/activate
```

Start JupyterLab:

```bash
jupyter lab
```

This will open JupyterLab in the browser.

---

# Step 10: Create Spark Session

In Jupyter Notebook:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("PySpark-Get-Started") \
    .master("local[*]") \
    .getOrCreate()

print("Spark Session Created Successfully!")
```

Expected output:

```text
Spark Session Created Successfully!
```

---

# Common Warnings (Safe to Ignore)

## Loopback Address Warning

```text
WARN Utils: Your hostname resolves to a loopback address
```

Safe on macOS local setup.

---

## Native Hadoop Warning

```text
WARN NativeCodeLoader: Unable to load native-hadoop library
```

Also normal on macOS.

---

# Useful Commands

## Check Java Version

```bash
java -version
```

---

## Check Installed Java Versions

```bash
/usr/libexec/java_home -V
```

---

## Deactivate Virtual Environment

```bash
deactivate
```

---

# Folder Structure

```bash
spark/
│
├── .pyspark-env/
├── notebooks/
├── data/
├── jars/
├── python/
└── spark-4.1.2-bin-hadoop3/
```

---

# Recommended Stable Versions

| Tool | Recommended Version |
|------|---------------------|
| Java | 17 |
| Python | 3.11 or 3.12 |
| PySpark | 4.1.2 |
| JupyterLab | Latest |
