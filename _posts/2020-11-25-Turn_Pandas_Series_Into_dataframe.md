---
layout: post
title: How to Turn a Distinct Pandas Series into a Dataframe
---

Here's something that came up yesterday, and I didn't see a good blog post or stack overflow answer for, so I hope this is a helpful topic.

When trying to make column values unique on a single column in a dataframe, that will return a series, so any merges you perform on the series will return a key error value. In order to fix this, you need to make the series a dataframe and name the column.

```
insertRows = pd.read_excel("excelSheet.xlsx", header=0)
distinctSeries = insertRows["column_name"].drop_duplicates()
distinctDF = uniquePrivClassSeries.to_frame()
distinctDF.columns = ["KeyName"]
```

After that is done, you can use the pandas merge method with this dataframe, like this (I perfer to have the two columns with separate names to make it easier to find key errors.):

```
finalRowsToInsert = pd.merge(distinctDF, checkListDF, how='inner', left_on='KeyName', right_on=u'KeyName2')
```

Hope that helps!    
