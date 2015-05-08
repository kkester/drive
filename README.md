# drive
The drive hypermedia specification

# Basic Design
All resources contain four optional high level objects
* links
* data
* entities
* schema

# Links
Links provide a list of the API's to related resources. Links must be an array of values which may be empty, but cannot be null.  The following is a list of attributes that defines a Link.
* rel - required - The static name of the Link
* href - required - The URI for the API
* text - optional - The Localized name of the Link to display.
* title - optional - The descriptive Localized name of the Link.
* method - optional - defaults to GET - the HTTP verb that should be used to invoke the Link.
* type - optional - Specifies the content type that should be used for the payload sent to the Link URI.

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
The entities section contains one or more embedded resources.  The entities section must be an object which is optional and may be empty or null. The entity structure from here is nested as seen in the example below.

Example:
```
"entities": {
	"child" : {
		"links": { ... },
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
The JSON schema that defines how the content should be structured for requests.  The schema is optional and can be null. If provided, the schema must be an object.  The schema only needs to be included if a Link is provided that has a method that requires request content.  In addition, the data section should list properties that match the schema.
