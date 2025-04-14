### Side-by-side comparison
```python
import sweetviz as sv
comparison = sv.compare([train_df,"Train"], [test_df,"Test"], target_feat="Price")
comparison.show_notebook(layout='vertical', w=880, h=700, scale=0.8)
```