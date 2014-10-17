Save-a-Selfie HTTP API
======================

Clients communicate with the database server through HTTP requests. 


Retrieve all entries
--------------------

Retrieving all entries from the database is done by sending a GET request to 
`/entries/`.

If there are no entries in the database, the server will send a 204 status;
otherwise, it will return a JSON array for all entries. Each entry is represented as a dictionary
with the following keys:
    id,
    latitude,
    longitude,
    uploadedby,
    thumbnail,
    comment,
    kind

Retrieve the metadata of all entries
------------------------------------

To retrieve only the metadata of all database entries (without image data), send a GET request to `/entries/metadata/`.
This will return a 204 if the database is empty, and a JSON array of dictionaries with the following keys:
    id,
    latitude,
    longitude,
    uploadedby,
    comment,
    kind


Retrieve all entries of one kind
--------------------------------

To retrieve all entries of a particular kind, e.g. all entries for AEDs, send a GET request to
`/entries/$kind`. If there are no entries of that kind in the database, the server will send a 204; 
otherwise, it will return the requested entries as a JSON array as described in "Retrieve all entries".

Create a new entry
------------------

To create a new entry, send a POST request to `/entries/` with the following parameters:
    latitude,
    longitude,
    thumbnail,
    image,
    uploadedby,
    comment,
    kind

`image` and `thumbnail` should be sent as base-64 encoded strings. `uploadedby` should contain a uniquely identifiable user ID, but can be left empty in case of anonymous upload.`comment` is a user comment that clarifies the location of the AED/other resource, e.g. "on the ground floor to the left of reception desk". `kind` is the kind of object that appears in the selfie. The following strings should be used for the `kind` property: 

Object          |  `kind`
-------------------------
Defibrillator   | AED

(to be extended)


Find an entry with a given ID
-----------------------------

To find an entry with a given ID, send a GET request to `/entries/$num`, where $num is the requested ID.
If the entry exists, the server will send a JSON array with a single dictionary corresponding to the entry.


Delete an entry with a given ID
-------------------------------

To delete an entry with ID $num, send a DELETE request to `/entries/$num`. 


Find entries near a particular location
-----------------------------------------

To find entries near the location ($latitude, $longitude), send a GET request to `/entries/near/$latitude,$longitude`. If you want to find entries within $distance km from the given location, the request URL is `/entries/near/$latitude,$longitude,$distance`. The entries will be returned as a JSON array of dictionaries. By default, this will give you the 10 closest entries, but you can get the N closest entries by appending `+N` to the URL, as in `/entries/near/$latitude,$longitude+N`.

To these URLs, `/$kind` can be appended if you want to find all the entries for a particular kind of object near a given location.



