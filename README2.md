## How to create a new output type

It can only generate stats per reference position, unless you modify the pile up they do.

Changes only needed in `pysamstats.pyx` and `scripts/pysamstats`, if you want
to be consistent with the rest of the code :

### pysamstats.pyx

`dtype_new_type` and `fields_new_type` : define the output fields
`_rec_new_type` where everything gets actually calculated

`load_new_type` (called via `pd.DataFrame.from_records(pysamstats.load_new_type(alignmentfile=bam.filename, fafile=fasta.filename, fields=fields,one_based=True))` for instance, where field are a subset of `fields_new_type`)

`load_new_type` calls `stat_new_type`, which basically iterates over the
reference positions, calling `_rec_new_type`

### scripts/pysamstats

Just add the new type created to `stats_types`, and a its description in `epilog`

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
