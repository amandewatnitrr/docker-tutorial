# Introduction to YAML

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/yaml.png)

<p align="justify">
<strong>

- YAML is a data serialization language that is often used for writing configuration files.
- Depending on whom you ask, YAML stands for yet another markup language or YAML ain’t markup language (a recursive acronym), which emphasizes that YAML is for data, not documents.
- YAML has features that come from Perl, C, XML, HTML, and other programming languages. YAML is also a superset of JSON, so JSON files are valid in YAML.
- All Ansible playbooks are written in YAMl. Ansible Playbooks are configuration files or text files rather that are written in a particular format called YAML.
- A YAML file is used to represent configuration data.
- YAML files are indentation sensitive.
- Data representation in YAML:
  
  ```YAML
    key1: value1 # This is how a key value pair is represented in YAML
    
    array_name: # This is how array/lists is represented in YAML. `-` represents that it's an element of the array.
        - array-ele-1
        - array-ele-2
        - array-ele-3
    
    dict_map:  # This is how dictionary/map is represented in YAML.
        map-ele-1: map-value-1
        map-ele-2: map-value-2
        map-ele-3: map-value-3

  ```

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/yaml_dict_vs_yaml_list.PNG)

- Dictionary is unordered while lists are ordered collection.
- Anyline starting with `#` in YAML is a comment.

# Introdcution to JSON Path

<img src="https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json.gif" style="width: 30%; height: auto;"> </img>

- JSON PATH is a query language that can help you parse data represented in JSON or YAML format.
- Just like query language in popular database softwares like SQL. For any given data we apply a query and we can get a result, which is a subset of that data.
- Similarly in the JSON world, `JSON PATH` is a query language that when applied to a given JSON dataset gives you result that are subset of that data.

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query1.PNG)

- The design goal of JSON is to be as simple as possible and be universally usable. This has reduced the readability of the data, to some extent. In contrast, the design goal of YAML is to provide a good human-readable format and provide support for serializing arbitrary native data structures.
- While YAML uses Indentation to organise and structure data, JSON uses braces `{}` or curly brackets.
- In YAML we use `-` to define an array or list, while we use `[]` to denote array in JSON, each element in JSON array is seprated by comma`,`.

- ```JSON
    {
    "key1": "value1",
    "array_name": [
        "array-ele-1",
        "array-ele-2",
        "array-ele-3"
    ],
    "dict_map": {
        "map-ele-1": "map-value-1",
        "map-ele-2": "map-value-2",
        "map-ele-3": "map-value-3"
    }
  ```

- The top level dictionary which has no name is `root element` of a JSON document. The root element is denoted by a `$`. So, all the above, queries will work only after adding `$.` in front of each query. That's the right way to form a json path query.
- All results of a JSON PATH Query are encapsulated within an array.

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query2.PNG)
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query3.PNG)

## JSON PATH - Criteria

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query4.PNG)

    ?() => Check if
    @ => each element of the array
    $[?(criteria)] // Syntax for JSON PATH Criteria with JSON Array/List.

## JSON PATH - WILD CARD

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query5.PNG)
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query6.PNG)
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query7.PNG)

</strong>
</p>
