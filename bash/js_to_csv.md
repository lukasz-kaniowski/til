# Json to csv

Install `jq` and `recs`

    cat out.json | jq  ".[]" --compact-output | recs tocsv > out.csv

Links:

* [https://basetable.wordpress.com/2014/11/13/convert-json-to-csv-with-jq-and-recordstream/](https://basetable.wordpress.com/2014/11/13/convert-json-to-csv-with-jq-and-recordstream/)
