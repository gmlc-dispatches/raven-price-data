## Quickstart

### Step 1: Install the data package in your DISPATCHES environment using pip

```sh
conda activate dispatches-dev  # or the name of your DISPATCHES dev environment
pip install git+https://github.com/gmlc-dispatches/synthetic-price-data
```

### Step 2: Access the contents of the data package in your code using `dispatches_data.api`

Using `dispatches_data.api.path()`:

```py
from dispatches_data.api import path

# path_to_data_package is a standard pathlib.Path object
path_to_data_package = path("synthetic_price")

# subdirectories and files can be accessed using the pathlib.Path API
path_to_xml = path_to_data_package / "ARMA_train.xml"
assert path_to_xml.is_file()

# alternatively, if the file is located directly under the data package directory (i.e. not in a subdirectory):
# this will raise an exception if the file does not exist so we don't need to check explicitly
path_to_xml = path("synthetic_price", "ARMA_train.xml")

# if the path must be passed to a function that only accepts `str` objects, it can be converted using `str()`
path_to_xml_as_str = str(path_to_xml)
```

Using `dispatches_data.api.files()`:

```py
from dispatches_data.api import files

# `all_price_files` will be a list of pathlib.Path objects for each file matching the specified `pattern`
all_price_files = files("synthetic_price", pattern="Price_20*.csv")
# check that the list of found files is not empty
assert all_price_files

# `dispatches_data.api.files()` always returns a list, even if only one file matches
price_for_21 = files("synthetic_price", pattern="Price_*21.csv")[0]
```

Using the CLI interface:

**NOTE** this example uses UNIX shell syntax. It will not work on Windows unless adapted to the specific shell (e.g. Powershell) being used.

```sh
# calling `dispatches-data-packages` on the command line with the name of the data package
# will output the absolute path to the data package, which can then be saved as a shell variable
# and used to generate paths to files in the data package
path_to_data_package="$(dispatches-data-packages synthetic_price)"
path_to_xml_file="$path_to_data_package/ARMA_train.xml"
ls -lh "$path_to_data_package"
cp "$path_to_xml_file" .

# the entire contents of the data package can be copied to the current working directory
# if this is required by the client application
cp -r "$path_to_data_package"/* .
ls -lh .
```

## Examples

### Use the data package with the RAVEN CLI

**NOTE** this example uses UNIX shell syntax. It will not work on Windows unless adapted to the specific shell (e.g. Powershell) being used.
**TODO**: must be tested

```sh
path_to_data_package="$(dispatches-data-packages synthetic_price)"
# copy contents to local directory
cp -r "$path_to_data_package"/* .
raven_framework ARMA_train.xml
```
