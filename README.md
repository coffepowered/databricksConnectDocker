## VSCode container for databricks

This container is designed for developing PySpark application in VS Code using Databricks-Connect.

Quick guide
1. Copy `.devcontainer` folder inside your project main folder
2. rename file `devcontainer.example.json` to `devcontainer.json` and configure DATABRICKS_* env vars
3. Open vs code and select `Open folder in container`
4. Once in, open a new shell and run `databricks-connect test`

## Requirements

### Windows 10
* VS Code with the [Remote Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
* Docker or a [CodeSpace](https://visualstudio.microsoft.com/services/visual-studio-codespaces/) Plan
* If using Docker:
  * `WSL 1` on Windows 10 is the only *tested* way of running this

## Test it out

First from command prompt check that you databricks connect install can connect:

```
databricks-connect test
```

If create a test.py file and paste this code:
```
from pyspark.sql import SparkSession
spark = SparkSession\
.builder\
.getOrCreate()

print("Testing simple count")

# The Spark code will execute on the Azure Databricks cluster.
print(spark.range(100).count())
```

Press F5 and select the Python debugger.

## Why?
Because setting up Databricks-Connect (particulary on Windows is a PIA). This allow:
* A common setup between team members
* Multiple side by side versions
* Ability to reset your environment
* Even run the whole thing from a [browser](https://docs.microsoft.com/en-gb/visualstudio/online/how-to/browser)!
