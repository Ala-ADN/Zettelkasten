#### take the root of the word + remove stopwords
```python
import re
import nltk
from nltk.corpus import stopwords
from nltk.stem.porter import PorterStemmer

def stemming(content):
  stemmed = re.sub('[^a-zA-Z]',' ',content)
  stemmed = stemmed.lower()
  stemmed = stemmed.split()
	  stemmed = [port_stem.stem(word) for word in stemmed if not word in stopwords.words('english')]
  stemmed = ' '.join(stemmed)

  return stemmed
```