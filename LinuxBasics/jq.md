learn to filter and transform json data. jq is like sed for JSON data. AN eloquent command line processor tool to filter and transform your JSON with ease

jq uses filters

can apply jq to json string, API response or json file

.[] iterates over each element of the array form the response, one at a time

iterate over each element:

```
jq '.[] | .details' learnjq.json
```

chain property values

```
jq '.[] | {name: .details.name, url:.details.url}' learnjq.json
```

Access single array item

```
jq '.[1] | {name: .details.name, url: .details.url}' learnjq.json
```

Put results in an array

```
jq '[.[] | {name: .details.name, url: .details.url}]' learnjq.js
```

jq functions:

length returns length of input

```
jq '.[] | {name: .details.name, servicecount: .services | length}' learnjq.json
```

select based on a condition  

```
jq '.[] | select(.details.name=="AWS")' learnjq.json
```

Access something within a file  

```
jq ".[0].summary.failed" /terraform/scan-result.json
```

Access a key with a . in it

```
cat example.json | jq .example.'"thing.with.dot"'
```
