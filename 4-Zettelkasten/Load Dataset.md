1. get [kaggle.json token](https://www.kaggle.com/settings)
2. RUN (! to run commands in ipynb)
```
!pip install kaggle
!mkdir -p ~/.kaggle
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json
```
3. get `!kaggle competitions download -c DATA`
![[Pasted image 20240219145540.png|500]]
4. unpack
```python
from zipfile import ZipFile
with ZipFile("DATA.zip",'r') as zip:
  zip.extractall()
  print("succ")
```
