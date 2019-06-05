[![Build Status](https://dev.azure.com/benamram/dfir/_apis/build/status/omerbenamram.pymft-rs?branchName=master)](https://dev.azure.com/benamram/evtx/_build/latest?definitionId=2&branchName=master)

# pymft-rs

Python bindings for `https://github.com/omerbenamram/mft/`.

## Installation

Available on PyPi - https://pypi.org/project/mft/.

To install from PyPi - `pip install mft` 

### Wheels
Wheels are currently automatically built for python3.6 python3.7 for all 64-bit platforms (Windows, macOS, and `manylinux`).

### Installation from sources
Installation is possible for other platforms by installing from sources, this requires a nightly rust compiler and `setuptools-rust`.

Run `python setup.py install`

## Usage

```python
from mft import PyMftParser, PyMftAttributeX10, PyMftAttributeX30

def main():
    parser = PyMftParser("/Users/omerba/Workspace/pymft-rs/samples/MFT")
    for entry_or_error in parser.entries():
        if isinstance(entry_or_error, RuntimeError):
            continue 
        
        print(f'Entry ID: {entry_or_error.entry_id}')
    
        for attribute in entry_or_error.attributes():
            resident_content = attribute.attribute_content
            if resident_content:
                if isinstance(resident_content, PyMftAttributeX10):
                    print(f'Found an X10 attribute')
                    print(f'Modified: {resident_content.modified}')
                if isinstance(resident_content, PyMftAttributeX30):
                    print(f'Found an X30 attribute')
                    print(f'Modified: {resident_content.modified}')
                    print(f'Name: {resident_content.name}')
            
        print('--------------------------------')
```

Will print:

```
Entry ID: 0
Found an X10 attribute
Modified: 2007-06-30 12:50:52.252395+00:00
Found an X30 attribute
Modified: 2007-06-30 12:50:52.252395+00:00
Name: $MFT
--------------------------------
Entry ID: 1
Found an X10 attribute
Modified: 2007-06-30 12:50:52.252395+00:00
Found an X30 attribute
Modified: 2007-06-30 12:50:52.252395+00:00
Name: $MFTMirr
--------------------------------
.....
```