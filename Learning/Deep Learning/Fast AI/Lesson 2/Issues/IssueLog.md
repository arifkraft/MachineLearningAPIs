# Log 

## 1. Kernel Frozen - Non responding 
**File:** lesson2-image_models.ipynb (Planet Challenge Notebook) <br>
**Code block:**
```python
learn.set_data(get_data(sz))
learn.freeze()
learn.fit(lr, 3, cycle_len=1, cycle_mult=2)
```
**Description:** Not sure what cuased it!<br>
**Solution:** Restart Kernel didn't work. Force `RESET` VM
