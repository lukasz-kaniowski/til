### find text in file => replace to create a filename => create that file

    grep -o  "partials/_.*'" views/layout.ejs  | sed -E "s/(.*)'/\1\.ejs/" | xargs touch
