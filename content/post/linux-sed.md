---
title: "Bash: Replace text, call script from python"
date: 2020-11-22T23:54:54+01:00
draft: false

categories:
- Linux
tags:
- linux
- sed

---
Basic linux sed commands to search, replace, and add strings in files.
<!--more-->

## Find lines with certain [string] and replace the whole [line]
sed -i "/[string]/c [new_line]" filePath

## Call script from Jupyter Python
```python
import subprocess

K = str("6666")
subprocess.check_call(['./run_simulation.sh', 'vClass=\"passenger\"', K])
```

where `run_simulation.sh` contains code as:
```bash
#!/bin/bash

# Replace the model.flow.xml file with new parameters
sed -i "/$1/c $2" model.flows.xml
```

Reference to this [stackoverflow](https://stackoverflow.com/questions/17242828/python-subprocess-and-running-a-bash-script-with-multiple-arguments)

## Examle of Jupyter and Script
```python
import subprocess

str_to_be_replaced = "<vType vClass=\"passenger\""
def run_sim(index):
    # Get new parameters
    param = simp_high.iloc[index]
    
    param_maxSpeed = param.iloc[0]
    param_minGap = param.iloc[1]
    param_accel = param.iloc[2]
    param_decel = param.iloc[3]
    
    # Replace flow.xml with new parameters
    str_to_replace = "\  \ <vType vClass=\"passenger\" id=\"passenger\"  tau=\"0.5\" speedDev=\"0.1\" maxSpeed=\"" + str(param_maxSpeed) \
        + "\" minGap=\"" + str(param_minGap) + "\" accel=\"" + str(param_accel) \
        + "\" decel=\"" + str(param_decel) + "\" latAlignment=\"center\" />"
    
    subprocess.check_call(['./run_simulation.sh', str_to_be_replaced, str_to_replace])
    
    # Replace sumo.cfg with new output file name
    str_output_prefix = "<output-prefix value=\"y" + str(index) + ".\"/>"
    subprocess.check_call(['./script_sumo_gui.sh', str_output_prefix])
```
