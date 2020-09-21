# SeekAPI Workshop

## Links
- [Workshop Resources](https://github.com/whomingbird/SEEK-API-Workshop)
- [The Seek-Test-Instance for this workshop](https://sandbox2.fairdomhub.org)
- Documentation
  - [Seek homepage](https://seek4science.org)
  - [The Seek documentation](https://docs.seek4science.org)
  - [The SeekAPI documentation](https://docs.seek4science.org/tech/api/index.html)
  - [Installation&Configuration troubleshooting](https://github.com/carpentries/workshop-template/wiki/Configuration-Problems-and-Solutions)


## General hints
### Easily access information via your browser
- Every resource is identified and accessible by `https://<host>/<resource-type>/<id>`
- Find the resource via your browser i.e. https://fairdomhub.org/models/308
- Just add `.json` to the URL to get the json response, i.e. https://fairdomhub.org/models/308.json
- Your browser should print the response in a nice filterable format. If not, try Firefox :)

### Get download link
- If an object provides `data.attributes.content_blobs` like [here](https://fairdomhub.org/models/308.json), you have a link like https://fairdomhub.org/models/308/content_blobs/5323
- You can download the file by adding `/download` to the URL, i.e. https://fairdomhub.org/models/308/content_blobs/5323/download

### Basic operations (python)
- headers
  - please see `01_Person.ipynb` for further details
  ```py
    headers = {"Content-type": "application/vnd.api+json",
               "Accept": "application/vnd.api+json",
               "Accept-Charset": "ISO-8859-1"}
  ```
- read
  - use a function
    ```py
    def json_for_resource(type, id):    
        # REM directly using 'requests' will only work for public resources,
        # for protected resources use your 'session' here
        r = requests.get(base_url + "/" + type + "/" + str(id), headers=headers)
        if (r.status_code != 200):
            print(r.json())
        r.raise_for_status()
        return r.json()
    ```
  - usage: `json_for_resource('[insert_type_here]',my_type_id)`
- insert
  - `session.post(base_url + '/[insert_type_here]', json=my_type)`
- update
  - `session.put(blob_url, data=my_data, headers={'Content-Type': 'application/octet-stream'})`
- delete
  - `session.delete(base_url + my_type['data']['links']['self'])`
- what types are defined?
  - [see the documentation](https://docs.seek4science.org/tech/api/index.html#tag/search)
  - `assays`, `data_files`, `events`, `institutions`, `investigations`, `models`, `people`, `presentations`, `programmes`, `projects`, `publications`, `sample_types`, `sops`, `studies`
- authentication
  - for using write access and requesting protected resources, you need to create a session
  ```py
    session = requests.Session()
    session.headers.update(headers)
    session.auth = (input('Username:'), getpass.getpass('Password'))
  ```

## Information and further reading
[All Jupyter-Notebooks in this repository are originally from this project.](https://github.com/FAIRdom/api-workshop)

Find more Jupyter-Notebooks, examples and Exercises here:
- https://github.com/FAIRdom/api-workshop
- https://github.com/seek4science/seekAPIexamples

## Usage examples
- https://seek.lisym.org/investigations/20
  - script (https://github.com/matthiaskoenig/ipython_for_SEEK-API)
- https://pubchem.ncbi.nlm.nih.gov/pathway/COVID-19+Disease+Map:704
