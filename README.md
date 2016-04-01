# README for d1_pyore

d1_pyore is a python library and utilities for working with OAI-ORE documents.

## Dependencies:

  * rdflib >= 4.0
  * rdflib-jsonld
  * requests
  
## How to use

### A. Create an OAI-ORE document from a list of PIDs.

Given the text file pids.txt:

<pre>
# Comment line, separate the # from text with a space.
# These are example values for pids2ore
# First row = identifier for resource map object
# Second row = identifier for metadata document
# Subsquent rows = identifiers for data
# Blank rows are ignored
# White space is stripped from start and end of rows.

ore_pid_value
sci_meta_pid_value
data_pid_1
data_pid_2
data_pid_3
</pre>

Generate an OAI-ORE document by:

<pre>
cat pids.txt | pids2ore
</pre>

The rdf-xml ORE document will be sent to stdout. Different formats (e.g. n3, turtle, json-ld) may be specified with the --format parameter.

### B. Text dump of an OAI-ORE document

Given the rdf-xml OAI-ORE document "eg1.xml", parse and dump out the contents in slightly more intelligable plain text:

<pre>
ore2txt ../examples/eg1.xml
</pre>


### C. Create an ORE programmatically in Python

```python
import d1_pyore

pkg = d1_pyore.ResourceMap()
pkg.oreInitialize("pid_for_ore")
pkg.addMetadataDocument("pid_for_metadata")
pkg.addDataDocuments(["data_pid_1", "data_pid_2"], "pid_for_metadata")
print pkg.serialize(format="json-ld", indent=2)

```

```json
[
  {
    "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_1",
    "http://purl.org/dc/terms/identifier": [
      {
        "@value": "data_pid_1"
      }
    ],
    "http://purl.org/spar/cito/isDocumentedBy": [
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_1"
      }
    ]
  },
  {
    "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_2",
    "http://purl.org/dc/terms/identifier": [
      {
        "@value": "data_pid_2"
      }
    ],
    "http://purl.org/spar/cito/isDocumentedBy": [
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_2"
      }
    ]
  },
  {
    "@id": "https://cn.dataone.org/cn/v2/resolve/pid_for_metadata",
    "http://purl.org/dc/terms/identifier": [
      {
        "@value": "pid_for_metadata"
      }
    ],
    "http://purl.org/spar/cito/documents": [
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_2"
      },
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_1"
      }
    ]
  },
  {
    "@id": "https://cn.dataone.org/cn/v2/resolve/pid_for_ore",
    "@type": [
      "http://www.openarchives.org/ore/terms/ResourceMap"
    ],
    "http://purl.org/dc/terms/creator": [
      {
        "@value": "d1_pyore DataONE Python library"
      }
    ],
    "http://purl.org/dc/terms/identifier": [
      {
        "@value": "pid_for_ore"
      }
    ],
    "http://www.openarchives.org/ore/terms/describes": [
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/pid_for_ore#aggregation"
      }
    ]
  },
  {
    "@id": "http://www.openarchives.org/ore/terms/Aggregation",
    "http://www.w3.org/2000/01/rdf-schema#isDefinedBy": [
      {
        "@id": "http://www.openarchives.org/ore/terms/"
      }
    ],
    "http://www.w3.org/2000/01/rdf-schema#label": [
      {
        "@value": "Aggregation"
      }
    ]
  },
  {
    "@id": "https://cn.dataone.org/cn/v2/resolve/pid_for_ore#aggregation",
    "@type": [
      "http://www.openarchives.org/ore/terms/Aggregation"
    ],
    "http://www.openarchives.org/ore/terms/aggregates": [
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_2"
      },
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/pid_for_metadata"
      },
      {
        "@id": "https://cn.dataone.org/cn/v2/resolve/data_pid_1"
      }
    ]
  }
]
```
