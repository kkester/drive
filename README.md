# drive - DRAFT
The following describes the drive hypermedia specification.  The intent of the drive media type is to provide a standard resource structure that can drive all behavior of a calling application.

The media type for drive JSON is application/vnd.drive+json.  Support for XML may be supported as well.

# Basic Design
All resources contain four optional high level objects
* links
* data
* entities
* schema

# Links
Links provide a list of the API's to related resources. Links must be an array of values which may be empty, but cannot be null.  The following is a list of attributes that defines a Link.
* rel - required - The relation type of the Link
* href - required - The URI for the API
* text - optional - The Localized name of the Link to display.
* title - optional - The descriptive Localized name of the Link.
* method - optional - defaults to GET - the HTTP verb that should be used to invoke the Link.
* type - optional - Specifies the content type that should be used for the payload sent to the Link URI.

Example:
```
"links" : [
    {
        "rel": "self",
        "href": "https://api.domain.com/resource",
        "title": "Resource"
    },
    {
        "rel": "edit",
        "href": "https://api.domain.com/resource",
        "title": "Update",
        "method":"PUT",
        "type":"appliation/json"
   } 
]
```

# Data
The data section contains all of the attributes and their values for the resource itself. The data section must be an object which may be empty, but cannot be null.

Example:
```
"data": {
	"company": "acmeinc",
	"name": "acme",
	"createdDate": "2015-12-25T13:10:37"
}
```

# Entities
The entities section contains one or more embedded resources.  The entities section must be an object which is optional and may be empty, but cannot be null. The entity structure from here is nested as seen in the example below.

Example:
```
"entities": {
	"child" : {
		"links": [ ... ],
		"data": { ... },
		"entities": { ... },
		"schema": { ... }
	},
	"children" : [
		{
			"links": { ... },
			"data": { ... },
			"entities": { ... },
			"schema": { ... }
		}
	]
}
```

# Schema
The JSON schema that defines how the content should be structured for requests.  The schema is optional, but cannot  be null. If provided, the schema must be an object.  Typically the data section would contain properties that then would be described by the schema.  The schema would then be used to build the request content to invoke a provided Link that has a type attribute defined.

Example:
```
"schema": {
	"title": "Company"
	"type": "object",
	"properties": {
		"company": {
			"type": "string"
		},
		"name": {
			"type": "string",
			"pattern":"[a-z,A-Z]"
		},
		"createdDate": {
			"type": "string"
		}
	},
	"required": ["name"]
}
```

# Examples
```
{
	"links": [
	    {
	        "rel": "self",
	        "href": "https://api.domain.com/products/123",
	        "title": "Product"
	    },
	    {
	        "rel": "up",
	        "href": "https://api.domain.com/products",
	        "title": "Product"
	    },
	    {
	        "rel": "edit",
	        "href": "https://api.domain.com/products/123",
	        "title": "Product",
	        "method":"PUT",
	        "type":"application/json"
	    }
	],
	"data": {
		"id":"123",
		"name":"",
		"sku":"",
		"createdDate":""
	},
	"entities": {
		"prices": [
			{
				"links": [
				],
				"data": {
				}
				"schema": {
				}
			}
		]
	},
	"schema": {
		"title": "Product"
		"type": "object",
		"properties": {
			"id": {
				"type": "integer",
				"readonly":true
			},
			"name": {
				"type": "string",
				"pattern":"[a-z,A-Z]"
			},
			"sku": {
				"type": "string"
			},
			"createdDate": {
				"type": "string"
			}
		},
		"required": ["name"]
	}
}
```
