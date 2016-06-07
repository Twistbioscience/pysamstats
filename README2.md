##additional output column

The results returned by pysam contains a new column called 'single_deletions'
It contains the number of single deletions (really ?!),
but the value is shifted, so please use the following function

##needed function to shift the single_deletions column

```
def shift_column(df, column='single_deletions'):
    tmp = list(df[column])
    for i in reversed(range(len(tmp))):
        tmp[i] = tmp[i-1]
    df[column] = tmp
```

##if you want to isolate the block_deletions count :

```
def isolate_block_deletions(df):
    df['deletions'] -= bam_stats.single_deletions
```
