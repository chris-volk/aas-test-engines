[![linting: pylint](https://img.shields.io/badge/linting-pylint-yellowgreen)](https://github.com/pylint-dev/pylint)

# Test Engines for the Asset Administration Shell

The Asset Administration Shell (AAS) is a standard for Digital Twins.
More information can be found [here](https://industrialdigitaltwin.org/content-hub/downloads).

The tools in this repository offer measures to validate compliance of AAS implementations against the AAS standard.

## Check AAS Type 1 (File)

### Check AASX:
```python
from aas_test_tools import file
from xml.etree import ElementTree

with open('aas.aasx') as f:
    file.check_aasx_file(f)
```

### Check JSON:

```python
from aas_test_tools import file

# Check file
with open('aas.json') as f:
    file.check_json_file(f)

# Or check data directly
aas = {
    'assetAdministrationShells': [],
    'submodels': [],
    'conceptDescriptions': []
}
file.check_json_data(aas)
```

### Check XML:
```python
from aas_test_tools import file
from xml.etree import ElementTree

# Check file
with open('aas.xml') as f:
    file.check_xml_file(f)

# Or check data directly
data = ElementTree.fromstring(
    '<environment xmlns="https://admin-shell.io/aas/3/0" />')
file.check_xml_data(aas)
```

### Checking older versions

By default, the `file.check...` methods check compliance to version 3.0 of the standard.
You may want to check against older versions by passing a string containing the version to these methods.

You can query the list of supported versions as follows:

```python
from aas_test_tools import file

print(file.supported_versions())
print(file.latest_version())
```

## Check AAS Type 2 (HTTP API)

### Check a running server instance

```python
from aas_test_tools import api

tests = api.generate_tests()

# Check an instance
api.execute_tests(tests, "http://localhost")

# Check another instance
api.execute_tests(tests, "http://localhost:3000")
```

### Checking older versions and specific profiles

By default, the `api.generate_tests` method generate test cases for version 1.0RC03 of the standard and all associated profiles.
You may want to check against older versions by passing a string containing the version to these methods.
You can also provide a list of profiles to check against:

```python
from aas_test_tools import api

tests = api.generate_tests('1.0RC03', ['repository'])
api.execute_tests(tests, "http://localhost")
```

You can query the list of supported versions and their associated profiles as follows:

```python
from aas_test_tools import api

print(api.supported_versions())
print(api.latest_version())
```

## Command line interface

You may want to invoke the test tools using the simplified command line interface:

```sh
# Check file
python -m aas_test_tools file test.aasx

# Check server
python -m aas_test_tools api https://localhost --profile registry
```
