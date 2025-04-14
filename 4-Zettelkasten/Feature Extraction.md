### TF-IDF: assign each word a frequency
TF = occ/wc
IDF = log(nbdoc/occdoc)
TF-IDF = TF x IDF
```python
from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer()
vectorizer.fit(array(str):X) #X text column
```
