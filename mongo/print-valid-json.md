# Printing valid json

Mongo shell has build in `printjson` command which is not producing valid json. Instead it contains i.e. `IsoDate`

In order to produce valid json use this instead

    print(JSON.stringify(db.collection.find().toArray()));
    
Executing mongo from command line requires passing `--quiet` flag so result can be piped into `jq`

    mongo script.js --quiet | jq
