###########
RESTful API
###########
***************************
Overview of the RESTful API
***************************

How it works
^^^^^^^^^^^^

Each of the api's URI match a document or a subset of documents. HTTP methods define actions to do on the document(s).

Theses methods are:

	* GET : 	retrieve a document or a subset of documents
	* POST : 	create a new document
	* PUT : 	update a document
	* DELETE : 	delete a document


Data format
^^^^^^^^^^^

Only JSON format is supported. You must set the Content-Type header at application/json in all requests to the server.


API responses
^^^^^^^^^^^^^

The API's responses contain the standards HTTP codes which are significant of what happened.  

	* 200 OK : 			all went successfully gone
	* 201 Created : 	a new document was created
	* 204 Deleted : 	a document was deleted
	* 400 Bad Request : the request is malformed
	* 401 Forbidden : 	document ressource is forbidden
	* 404 Not Found : 	document is not found
	* 500 Internal :	Server Error : a server internal error occurred

	
API structure
-------------

URIs
^^^^

URI are prefixed with /api/, followed by the document’s type and optionnaly by the document identifier. 

    *URIs examples*
	
    >>> /api/<DOCUMENT_TYPE>/
	 
    >>> /api/<DOCUMENT_TYPE>/<DOCUMENT_ID>
	
	
Playing with the API
^^^^^^^^^^^^^^^^^^^^^

Curl is used in examples bellow to make request to the server. If you're not familiar with please take a look at http://curl.haxx.se/
	
1. Get a document's list

	It's done by invoking the GET HTTP method on an URI like /api/<DOCUMENT_TYPE>/
	
	*Request*
	
	>>> curl -XGET http://<HOST>/api/<DOCUMENT_TYPE>/
	
	*Response*
	
	The server returns data as below
	
	>>> {
	    "total_rows":<Database Document's count>,
	    "rows":[{
	 	   Document data...
	 	   }]
	    }
	
	* total_row is the document count of the specified type in the database, not in the response.
	* rows contains the documents, see data structure for more explanations.
	
	1.1. Pagination
	
		Pagination is implemented by passing extra parameters in query string. These parameters are skip and limit. 
		
		For example
		
		>>> curl -XGET http://<HOST>/api/<DOCUMENT_TYPE>/?skip=10&limit=5
		
		This query will return documents from 10th to 15th 
		
	1.2. Fulltext search
	
		Fulltext search is done by passing a fulltext parameter.
		
		For example
		
		>>> curl -XGET http://<HOST>/api/<DOCUMENT_TYPE>/?fulltext=<SEARCH_STRING>
		
		The search string can contain boolean operators such as AND and OR
		
		For example
		
		>>> curl -XGET http://<HOST>/api/<DOCUMENT_TYPE>/?fulltext="search_string OR another_search_string"
		
2. Get a document
	
	It's done by invoking the  GET HTTP method on an URI like /api/<DOCUMENT_TYPE>/<DOCUMENT_ID>
	
	*Request*
	
	>>> curl -XGET http://<HOST>/api/<DOCUMENT_TYPE>/<DOCUMENT_ID>/
	
	*Response*
	
	The server returns one document
	
	>>> { Document data... }

3. Create a document

	It's done by invoking the POST HTTP method on an URI like /api/<DOCUMENT_TYPE>/
	
	*Request*
	
	>>> curl -XPOST http://<HOST>/api/<DOCUMENT_TYPE>/ -H 'Content-Type: application/json' -d '{
			"an_attribute" : "a value", "another_attribute" : "another value" 
		}'
	
	.. warning:: Do not forget the Content-Type parameter

	*Response*
	
	The server returns the newly created document with some system attributes such as _id or _rev. Systems attributes are described HERE
	

4. Update a document

	It's done by invoking the PUT HTTP method on an URI like /api/<DOCUMENT_TYPE>/<DOCUMENT_ID>
	
	*Request*
	
	>>> curl -XPUT http://<HOST>/api/<DOCUMENT_TYPE>/ -H 'Content-Type: application/json' -d '{
			"an_attribute" : "an updated value", "another_attribute" : "another value" 
		}'
	
	.. warning:: Do not forget the Content-Type parameter

	*Response*
	
	The server returns the updated document.
	
	
5. Delete a document

	It's done by invoking the DEL HTTP method on an URI like /api/<DOCUMENT_TYPE>/<DOCUMENT_ID>
	
	*Request*
	
	>>> curl -XDEL http://<HOST>/api/<DOCUMENT_TYPE>/<DOCUMENT_ID>
		
	*Response*
	
	The server returns nothing
		
	
*************************************
Specifics of the API by document type
*************************************
	
Search
^^^^^^


1. For events

	Geographic and date time searchs are implemented.

	1.1. Geographic search
	
		Geographic search is a search that limits events contained in a bounding box.
	
		For example
		
		>>> curl -XGET http://<HOST>/api/event/?bbox=<TOP_LEFT_LATITUDE>,<TOP_LEFT_LONGITUDE>,<RIGHT_BOTTOM_LATITUDE>,<RIGHT_BOTTOM_LONGITUDE>
		
	1.2. Date time search
		
		Two parameters are availlable: start_time and end_time (optionnal)
				
		Lorsque seul le paramètre start_time est fourni, les évènements retournés sont ceux pour lesquels la date fournie est comprise entre les dates de début et de fin de l'évènement.
		
		Lorsque les deux paramètres start_time et end_time sont fournies, les évènements retournés sont ceux pour lesquels l'intersection des deux intervales de date n'est pas nul. 
		