# For Collapsible Headings 
## 1. Install the python package
```
conda install -c conda-forge jupyter_contrib_nbextensions
#or
pip install -e jupyter_contrib_nbextensions
```
## 2. Javascript and .css 
```
jupyter contrib nbextension install --user
```
## 3. Enabling/Disabling extensions
```
jupyter nbextension enable codefolding/main

# to disable
jupyter nbextension disable codefolding/main
```

