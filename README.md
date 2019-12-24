
This is a quick script that allows you to save common file to any location with date time appended behind like "filename_1912181212.csv" (`save_dt`). Everytime you want to load it back, just use `load_newest`, it will automatically find the newest file.

Feel free to download/edit this script, and maybe share your version with me!


```python
import sldt
```


```python
# launch logger to see info when saving and loading files
import logging
logging.getLogger().setLevel(logging.INFO)
```


```python
sldt.__SUPPORTED_EXT__
```




    ['.pkl', '.csv', '.png', '.txt']



## anything → pkl


```python
a_list = ['a', 0.1, False]

# save two files for demo
sldt.s(a_list, 'output/a_list.pkl')
import time
time.sleep(60)
sldt.s(a_list, 'output/a_list.pkl')

# it will load the newest (which is the later one) back
a_list = sldt.l('output/a_list.pkl')
a_list
```

    INFO:root:output/a_list_1912240137.pkl saved
    INFO:root:output/a_list_1912240138.pkl saved
    INFO:root:output/a_list_1912240138.pkl loaded





    ['a', 0.1, False]



## pandas dataframe → csv


```python
import pandas as pd
df = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
sldt.s(df, 'output/df.csv')

df = sldt.l('output/df.csv')
df
```

    INFO:root:output/df_1912240108.csv saved
    INFO:root:output/df_1912240108.csv loaded



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>4</td>
    </tr>
  </tbody>
</table>


you can pass any argument as to `pd.DataFrame.to_csv()`, if there's no arguments in `save_dt`, the default will be `index=False`.

For example, `save_dt(df, 'output/df.csv', sep=';')`

## figure → png


```python
import seaborn as sns
exercise = sns.load_dataset("exercise")
g = sns.catplot(x="time", y="pulse", hue="kind", data=exercise)

sldt.s(g, 'output/g.png')
```

    INFO:root:output/g_1912240108.png saved


Display it using `![](output/g_1912240037.png)` in markdown.

the default arguments will be `dpi=600, bbox_inches: 'tight'`. And it will try to close the fig after saving the file.

## string → txt


```python
text = '''some random sentences
here
and there'''
sldt.s(text, 'output/text.txt')

text = sldt.l('output/text.txt')
text
```

    INFO:root:output/text_1912240108.txt saved
    INFO:root:output/text_1912240108.txt loaded





    'some random sentences\nhere\nand there'



## helper functions

you can save your own file type and load it back using `append_dt` and `find_newest`


```python
import pickle

output_filename = sldt.append_dt('output/sth.pkl', datetime_format="%y%m%d%H%M")[0] # it returns a tuple as (filename, extension)
with open(output_filename, 'wb') as f:
    pickle.dump('some strings', f)
```


```python
newest_file = sldt.find_newest('output/sth.pkl')[0]
with open(newest_file, 'rb') as f:
    sth = pickle.load(f)
sth
```




    'some strings'




```python

```
