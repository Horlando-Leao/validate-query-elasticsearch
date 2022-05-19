# validate-query-elasticsearch
validate query elasticsearch example

I had a bit of trouble finding an implementation example, so since I did, I'll post it here to help.

```python
from elasticsearch import Elasticsearch
from elasticsearch.client.indices import IndicesClient

es = Elasticsearch(
    hosts=['localhost'],
    http_auth=('user-auth', 'pass-auth'),
    scheme=False,
    port=9200,
    verify_certs=True
)

indices = IndicesClient(client=es)


query = {
    "query": {
        "bool": {
            "filter": [
                {
                    "query_string": {
                        "query": "Developer AND \"Systems Analyst\" AND (java OR python)",
                        "default_field": "github"
                    }
                }
            ]
        }
    }
}

query_string = "Developer AND \"Systems Analyst\" AND (java OR python)"

validate_dict = indices.validate_query(index='my-index-test', body=query, doc_type='doc')
print(validate_dict)

validate_string = indices.validate_query(index='my-index-test', q=query_string, doc_type='doc')
print(validate_string)
```
