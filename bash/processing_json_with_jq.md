# jq processing #

## Converting `xml` file to `json` ##

1. convert xml => json using [xml2json](https://github.com/Inist-CNRS/node-xml2json-command) 
2. process created json to new format with [jq](https://stedolan.github.io/jq/)

Sample xml:

```xml
<configuration>
    <header_image source="Content/main.jpg"/>
    <info_html source="Content/info/appInfo.html"/>
    <articles>
        <article>
            <title value="Shooting flowers"/>
            <intro_image source="Content/flowers_intro.jpg"/>
            <intro_html source="Content/flowers_intro.html"/>
            <main_image source="Content/flowers.jpg"/>
            <main_html source="Content/flowers.html"/>
            <read_more value="true"/>
            <payed value="false"/>
        </article>
        <article>
            <title value="Shooting landscapes"/>
            <intro_image source="Content/landscapes_intro.jpg"/>
            <intro_html source="Content/landscapes_intro.html"/>
            <main_image source="Content/landscapes.jpg"/>
            <main_html source="Content/landscapes.html"/>
            <read_more value="true"/>
            <payed value="false"/>
        </article>
    </articles>
</configuration>
```
Converted json:

```json
{
	"configuration": {
		"articles": {
			"article": [
				{
					"title": {
						"value": "Shooting flowers"
					},
					"intro_image": {
						"source": "Content/flowers_intro.jpg"
					},
					"intro_html": {
						"source": "Content/flowers_intro.html"
					},
					"main_image": {
						"source": "Content/flowers.jpg"
					},
					"main_html": {
						"source": "Content/flowers.html"
					},
					"read_more": {
						"value": "true"
					},
					"payed": {
						"value": "false"
					}
				},
				{
					"title": {
						"value": "Shooting landscapes"
					},
					"intro_image": {
						"source": "Content/landscapes_intro.jpg"
					},
					"intro_html": {
						"source": "Content/landscapes_intro.html"
					},
					"main_image": {
						"source": "Content/landscapes.jpg"
					},
					"main_html": {
						"source": "Content/landscapes.html"
					},
					"read_more": {
						"value": "true"
					},
					"payed": {
						"value": "false"
					}
				}
			]	
		}
	}
}	
```
Jq transformation: 

```bash
$ cat config.json |jq '[.configuration.articles.article | .[] |
{title: .title.value,
introImage: .intro_image.source,
image: .main_image.source,
details: .main_html.source,
payedVersion: .payed.value,
readMore: .read_more.value
}]'
```

Result:
```json
[
  {
    "title": "Shooting flowers",
    "introImage": "Content/flowers_intro.jpg",
    "image": "Content/flowers.jpg",
    "details": "Content/flowers.html",
    "payedVersion": "false",
    "readMore": "true"
  },
  {
    "title": "Shooting landscapes",
    "introImage": "Content/landscapes_intro.jpg",
    "image": "Content/landscapes.jpg",
    "details": "Content/landscapes.html",
    "payedVersion": "false",
    "readMore": "true"
  }
]
```
