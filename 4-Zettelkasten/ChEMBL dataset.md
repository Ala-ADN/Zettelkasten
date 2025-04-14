```bash
! pip install chembl_webresource_client
```

### Search ChEMBL for Target Molecule
```python
target = new_client.target target_query = target.search('<Target Molecule>') 
targets = pd.DataFrame.from_dict(target_query)
tagets
```

### Select the dataset with ChEMBL ID 
filter to get bioactivity mesurments in standard IC50 units
```python
id = targets.target_chembl_id[<index>]
activity = new_client.activity 
res = activity.filter(target_chembl_id=id).filter(standard_type="IC50")
df = pd.DataFrame.from_dict(res)
df
```
