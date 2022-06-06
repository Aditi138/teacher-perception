### Teacher Perception ###

## Code for Extracting the Learning Materials
All the required scripts are `code/`, to view the rules in the required website, first create a folder (`website`) to hold the rules and copy the source html files --
```
   mkdir -p website
   cp html_pages/* website
```

To extract word order patterns, run --
For word order:
```
    cd code/
    python word_order.py \
    --input ../data/ \
    --file input_files.txt \
    --lexical \
    --use_xgboost \
    --best_depth 10 10 10 10 10 \
    --folder_name website/
    --transliterate mar
```
`--best_depth` sets the depth of the decision tree, generally 10 is a good number to extract rules that result in a reasonable model performance with keeping them concise.
But if you want to find the best performing depth, we recommend the `--no_print` option, which would only print the model accuracy with different depths, but this will not
print the rules in the website, after finding the best depth, you could run the above command by removing the `--no_print` option.
The above command extract rules for five orders: subject-verb, object-verb, adjective-noun, numeral-noun and adposition-noun. The depth parameters should also be specified
in that order for the required model. If you don't want to run models for all orders, specify which orders to skip in the option like this `--skip_models subject-verb object-verb`
and in the best-depth field simply specify -1 (e.g. `--best_depth -1 -1 10 10 10`)/
`--transliterate` option will allow you to transliterate the examples in the Roman script, note this option is only available for Marathi (mar) and Kannada (kan) languages,
for other languages simply do not add it.

For agreement:
```
    cd code/
    python agreement_per_pos.py \
    --input ../data/ \
    --file input_files.txt \
    --lexical \
    --use_spine \
    --use_xgboost \
    --best_depth 10 10 10 \
    --features Gender+Person+Number \
    --folder_name website/
    --transliterate mar
```
### Data for training Kannada and Marathi Syntactic Parser
The treebanks converted in the SUD format can be found [here](https://github.com/Aditi138/auto-lex-learn/tree/master/data).
It contains POS tags, lemmatization, morphological analyses and dependency analyses, note these are automatically converted data and not manually verified.
