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

<img src="https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json.gif" style="width: 60%; height: auto;"> </img>

- 

</strong>
</p>
