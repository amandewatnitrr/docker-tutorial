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

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query5.PNG)WW
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query6.PNG)
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query7.PNG)

## JSON PATH - LISTS

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query8.PNG)
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query9.PNG)
![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query10.PNG)

## JSON PATH - KUBERNETES

### Why JSON PATH ?

- When you are working with production environments for kubernetes, we will need to see infor about 100s of nodes, 1000s of PODS, deployments and ReplicaSets, services and secrets etc.. and we will be using Kubctrl utility to view information about these objects. We will often have equirements where we will need to print summary of different states, about different resources, view different fields of all resources, query data about the resources based on different criteria etc.
- Viewing such information by going through 1000s of these resources is an overwhelming task which is why Kubectrl supports a JSON PATH option  that makes filtering data across large datasets using complex criteria an easy task.

### KubeCtl

- KubeCtl utility is a Kubernetes CLI used for reading and writing the Kubernetes Objects. Every time we run a KubCtl command, it interacts with the Kubernetes API and so the `kube-apiserver`. The `kube-apiserver` speaks the JSON language, so it returns the request to the information in a JSON format.
- The KubeCtl utility on receiving the information in a JSON format converts it into a human readable format and print it out to our screen. During that process, a lot of information that came in, in the JSON format is hidden in an effort to make the output readable by showing only the nessecary details.
- And if you like to see additional details, we can use the `-o wide` option with the  `kubectl get nodes` command. This prints additional details,but again this is not complete. There are still a lot of detils that are not yet the part of this output. For example the resource capacity vailable on these nodes, conditions of the nodes, the hardware architecture, the images available in these nodes etc.
- With JSON PATH Queries we can filter and format the output of the command as we like.

#### How to JSON PATH in KubeCtl ?

1. We need to know the command that will give me the required information in the raw format, i.e. identify KubeCtl commands. Example:

   ```KubeCtl
    kubectl get nodes
    kubectl get pods
   ```

2. Inspect the command output in JSON format. For our example add the `-o json` option at the end to the command. It will rpint the output in a JSON Format.

3. Look throught the structure of the JSON document and form the JSON PATH query that will retrieve the required information for you.

4. Finally, use the JSON PATH Query we developed with same kubectl command. For that use the `-o=jsonpath=` option passing the same JSON PATH query, that we just developed. Remember we must encapsulate the JSON PATH query within `'{json_path_query}'` as specified.

5. We now have Kubectl command with JSON PATH Query ready.  

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/json-path-query11.PNG)


</strong>
</p>
