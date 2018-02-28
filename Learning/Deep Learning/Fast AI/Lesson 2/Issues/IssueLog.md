# Log 

## 1. Kernel Frozen - Non responding 
**File:** lesson2-image_models.ipynb (Planet Challenge Notebook) <br>
**Code block:**
```python
learn.set_data(get_data(sz))
learn.freeze()
learn.fit(lr, 3, cycle_len=1, cycle_mult=2)
```
**Description:** : Out of CPU RAM <br>
**Solution:** Upgrade to 8 vCPU 64 GB RAM. Also FastAI doesn't support multi-GPU. Get P100 instead of K80
