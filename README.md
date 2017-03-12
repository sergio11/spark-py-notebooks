## Integración de PySark with Jupyter Notebook

### Pyspark in Ipython

To be able to choose spark when launching an ipython shell.

Two steps need to be taken:

1. Add a ipython profile named **pyspark**.

`ipython profile create pyspark`

2. Add `~/.ipython/profile_pyspark/startup/00-pyspark-setup.py`

```import os
import sys
spark_home = os.environ.get('SPARK_HOME', None)
if not spark_home:
    raise ValueError('SPARK_HOME environment variable is not set')
sys.path.insert(0, os.path.join(spark_home, 'python'))
sys.path.insert(0, os.path.join(spark_home, 'python/lib/py4j-0.10.1-src.zip'))
exec(open(os.path.join(spark_home, 'python/pyspark/shell.py')).read())```

Then we can now launch the following command: `ipython --profile=pyspark`

## Pyspark in Jupyter Notebook

After created pyspark ipython profile, although Jupyter use Kernel to control its configuration
, we can further create a pyspark kernel to launch pyspark in Jupyter Notebook.
 Add pyspark Jupyter kernel at `/home/sergio/.local/share/jupyter/kernels/pyspark` (for example).

And then put the following configuration:

```{
    "display_name": "PySpark (Spark 2.0.0)",
    "language": "python",
    "argv": [
       "python2.7", "-m", "IPython.kernel", "-f", "{connection_file}", "--profile=pyspark"
    ],
    "env": {
        "CAPTURE_STANDARD_OUT": "true",
        "CAPTURE_STANDARD_ERR": "true",
        "SEND_EMPTY_OUTPUT": "false",
        "SPARK_HOME": "/usr/local/spark"
    }
}```
