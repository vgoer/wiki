<center>googleTrans库</center>









[toc]









## googleTrans

> Googletrans [trans](https://py-googletrans.readthedocs.io/en/latest/)  是一个**免费**且**无限制的**Python 库，它实现了 Google Translate API。它使用[Google Translate Ajax API](https://translate.google.com/)来调用检测和翻译等方法。







### 1. 安装

> 安装

```shell
pip install googletrans
```





### 2. 使用

```python
from googletrans import Translator
translator = Translator()
translator.translate('안녕하세요.')
# <Translated src=ko dest=en text=Good evening. pronunciation=Good evening.>

translator.translate('안녕하세요.', dest='ja')
# <Translated src=ko dest=ja text=こんにちは。 pronunciation=Kon'nichiwa.>

translator.translate('veritas lux mea', src='la')
# <Translated src=la dest=en text=The truth is my light pronunciation=The truth is my light>
```

> 翻译

```python
from googletrans import Translator
translator = Translator()
translator.translate('안녕하세요.')
# <Translated src=ko dest=en text=Good evening. pronunciation=Good evening.>
translator.translate('안녕하세요.', dest='ja')
# <Translated src=ko dest=ja text=こんにちは。 pronunciation=Kon'nichiwa.>
translator.translate('veritas lux mea', src='la')
# <Translated src=la dest=en text=The truth is my light pronunciation=The truth is my light>
```

