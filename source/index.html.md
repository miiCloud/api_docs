---
title: miiCloud API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  
toc_footers:
  - <a href="mailto:dev@miicloud.io?Subject=Create%20Sandbox%20Dev%20Token">Request a Developer Token</a>
  - <a href='https://github.com/lord/slate'>Docs Powered by Slate</a>

includes:
  - sample_code
  - errors
  
  

search: true
---

# Overview

Welcome!

miiCloud is a Computer Vision company that leverages Deep Learning for image and video analysis.  We train and calibrate our models on an extensive data pipeline to bring the best technology to our customers.  

This guide provides an overview of using the miiCloud Application Programming Interface (API), related operations, request and response structures.  The API is organized around REST and returns JSON encoded responses. 

Everything on miiCloud's platform revolves around a "Person" and their identity.  Our goal is to provide the most secure, accurate, fast and user friendly platform to store and verify a person's identity so customers like yourself can build amazing applications for your end users.  The following document describes how to "Enroll" a new Person into miiCloud and the follow-on actions that are available to you. 

We will continue to add new APIs to this site on a regular basis.  If you have specific requests for your business, please contact us: dev@miicloud.io

# Authentication

> To authorize, use this code:

```python
# The 'requests' library helps to create & parse json objects easily
import requests

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
requests.post(api_endpoint, headers={'Authorization': auth_token}, json = enrollment_payload)
```

```shell
# With shell, you can just pass the correct header with each request
 curl -X POST \
  api_endpoint \
  -H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131' \  
```


miiCloud uses API tokens to allow access to our API. You can request a unique API token by emailing dev@miicloud.io

miiCloud expects for the API token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131`

<aside class="notice">
Note the API token listed here is public.  You can use it or request a unique token at no cost by emailing dev@miicloud.io
</aside>

# Persons

## Enroll Person
```python
import requests
import os

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
image_path = 'input/persons/will_smith/will_smith_001.jpg'
api_endpoint = 'https://sandbox.miicloud.io/miicloud/person'
enrollment_payload = 
{ 
    'customer_person_id': '1234567', 
    'first_name': 'John', 
    'last_name': 'Doe', 
    'email': 'test@domain.com', 
    'phone': '0123456789' 
}
filename = os.path.basename(image_path)
file = {'image_path': (filename, open(image_path, 'rb'), "multipart/form-data")}
enroll_response = requests.post(api_endpoint, headers={'Authorization': auth_token}, data = enrollment_payload, files = file)
```

```shell
curl -X POST \
https:///sandbox.miicloud.io/miicloud/person \
-H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131' \
-F customer_person_id=5712f5242cf956c7c1d248e89 \
-F image_path=@input/persons/will_smith/will_smith_001.jpg \
-F first_name=John \
-F last_name=Doe \
-F email=test@domain.com \
-F phone=+141524204243
```

Identifies the largest face in a given image and adds it to the database.  If a human face is not found, it returns a “fail” status.  In order to provide high accuracy for subsequent matches, during enrollment, a face must be presented with a frontal pose with area of the face consisting of eye brows, eyes, nose and lips. 

> The above request returns a JSON object structured as follows:

```json
{
    "status": "success",
    "miicloud_person_id": "059339e9-02d7-4777-95ce-acca440af413",
    "customer_person_id": "5712f5242cf956c7c1d248e89",
    "first_name": "John",
    "last_name": "Doe",
    "email": "test@domain.com",
    "phone": "+141524204243",
    "bounding_box": {
        "left_x": 38,
        "left_y": 53,
        "right_x": 168,
        "right_y": 183
        },
    "image": "enroll/will_smith_001_Fkkx7eK.jpg"
}

```
>Successfull HTTP status code = 200

### Sandbox API Endpoint
`POST https://sandbox.miicloud.io/miicloud/person`
### Production API Endpoint
`Please contact dev@miicloud.io`

### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
customer_person_id | Yes | a unique identifier maintained by the customer per person enrolled in the database.  This identifier allows you link multiple images of an individual to their identity and link other identifiable info such as name, etc.
image_path | Yes | local or remote destination of the image accessible by your application
first_name | No | first name associated with the person being added to database
last_name | No | last name associated with the person being added to database
email | No | email associated with the person being added to database
phone | No | phone # associated with the person being added to database

### Response 

Field | Required | Description
--------- | ------- | -----------
status | Yes | response by API whether it was successful or not
miicloud_person_id | No | unique identifier stored in miiCloud per person.  This identifier links the identity in the customer’s system to miiCloud.
customer_person_id | No | a unique identifier maintained by the customer per person enrolled in the database.  miiCloud returns this identifier in the response to allow the customer to validate correct identities were linked
first_name | No | first name associated with the person being added to database
last_name | No | last name associated with the person being added to database
email | No | email associated with the person being added to database
phone | No | phone # associated with the person being added to database
bounding_box | No | an object with bounding box coordinates around the face
left_x | No | x-coordinate of the bounding box’s top left corner 
left_ y | No | y-coordinate of the bounding box’s top left corner
right_x | No | x-coordinate of the bounding box’s bottom right corner
right_y | No | y-coordinate of the bounding box’s bottom right corner
image | No | image name that was passed in with miicloud's suffix added to the end of filename

<!-----------
===============================================
===============================================
===============================================
----------->

## Update Person
```python
import requests
import os

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
image_path = 'input/persons/will_smith/will_smith_003.jpg'
api_endpoint = 'https://sandbox.miicloud.io/miicloud/person'
payload = 
{ 
    'customer_person_id': '5712f5242cf956c7c1d248e89', 
    'first_name': 'John', 
    'last_name': 'Doe', 
    'email': 'john_doe_2@domain.com', 
    'phone': '+12222222222' 
}
filename = os.path.basename(image_path)
file = {'image_path': (filename, open(image_path, 'rb'), "multipart/form-data")}
enroll_response = requests.put(api_endpoint, headers={'Authorization': auth_token}, data = payload, files = file)
```

```shell
curl -X PUT \
https://sandbox.miicloud.io/miicloud/person/5712f5242cf956c7c1d248e89 \
-H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131' \
-F image_path=@input/persons/will_smith/will_smith_003.jpg \
-F first_name=John \
-F last_name=Doe \
-F email=john_doe_2@domain.com \
-F phone=+12222222222
```

Updates an existing person in the database with the new information sent in the request payload.

<aside class="notice">
If any field is left blank in the request, the Person's data will be removed for that field(s).  The "image_path" field can be left blank since previously enrolled images will not be removed.
</aside>

> The above request returns a JSON object structured as follows:

```json
{
    "status": "success",
    "miicloud_person_id": "19b38661-7941-484c-82f6-81ea85172e0e",
    "customer_person_id": "5712f5242cf956c7c1d248e89",
    "first_name": "John",
    "last_name": "Doe",
    "email": "john_doe_2@domain.com",
    "phone": "+12222222222",
    "images": [
        {
            "image": "https://mc-enroll-images.s3.amazonaws.com/media/enroll/will_smith_003_EusaSSY.jpg",
            "created": "2019-04-06T17:49:05.550801Z"
        },
        {
            "image": "https://mc-enroll-images.s3.amazonaws.com/media/enroll/will_smith_002_OxCrHWz.jpg",
            "created": "2019-04-06T17:45:43.458062Z"
        }   
    ],
    "bounding_box": {
        "left_x": 35,
        "left_y": 77,
        "right_x": 222,
        "right_y": 264
    }
}

```
>Successfull HTTP status code = 200

### Sandbox API Endpoint
`PUT https://sandbox.miicloud.io/miicloud/person`
### Production API Endpoint
`Please contact dev@miicloud.io`

### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
customer_person_id | Yes | a unique identifier maintained by the customer per person enrolled in the database.  This identifier allows you link multiple images of an individual to their identity and link other identifiable info such as name, etc.
image_path | No | local or remote destination of the image accessible by your application.  This image is added to the collection of image(s) already associated with the person
first_name | No | new first name to be associated with the person 
last_name | No | new last name to be associated with the person
email | No | new email to be associated with the person
phone | No | new phone # to be associated with the person

### Response 

Field | Required | Description
--------- | ------- | -----------
status | Yes | response by API whether it was successful or not
miicloud_person_id | No | miiCloud's unique identifier associated with the person whose information was updated. This field can be used to validate correct person's information was updated.
customer_person_id | No | the unique identifier of the Person whose information needs to be updated
first_name | No | new or existing first name associated with the person
last_name | No | new or existing last name associated with the person
email | No | new or existing email associated with the person
phone | No | new or existing phone # associated with the person
images | No | array of images associated with the person.  For security reasons, these images cannot be downloaded from the Person's profile
image | No | file name 
created | No | timestamp of when the image was enrolled by the customer
bounding_box | No | an object with bounding box coordinates around the face (if a new image pass passed in with the Person's face)
left_x | No | x-coordinate of the bounding box’s top left corner 
left_ y | No | y-coordinate of the bounding box’s top left corner
right_x | No | x-coordinate of the bounding box’s bottom right corner
right_y | No | y-coordinate of the bounding box’s bottom right corner

<!-----------
===============================================
===============================================
===============================================
----------->

## Get Person
```python
import requests

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
api_endpoint = 'https://sandbox.miicloud.io/miicloud/person

customer_person_id = 5712f5242cf956c7c1d248e89

response = requests.get(api_endpoint + '/' + customer_person_id, headers={'Authorization': auth_token})

```

```shell  
curl -X GET \
https://sandbox.miicloud.io/miicloud/person/5712f5242cf956c7c1d248e89 \
-H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131'

```
> The above request returns a JSON object structured as follows:

```json
{
    "customer_person_id":"5712f5242cf956c7c1d248e89",
    "miicloud_person_id":"059339e9-02d7-4777-95ce-acca440af413",
    "images":[
        {
            "image":"https://mc-enroll-images.s3.amazonaws.com/media/enroll/will_smith_002_zktkkgN.jpg",
            "created":"2019-04-04T22:11:08.110600Z"
        },        
        {
            "image":"https://mc-enroll-images.s3.amazonaws.com/media/enroll/will_smith_001_Yrayh4h.jpg",
            "created":"2019-04-04T21:44:19.968710Z"
        }
    ],
    "first_name":"John",
    "last_name":"Doe",
    "phone":"+12222222222",
    "email":"john_doe_2@domain.com"
}

```
>Successfull HTTP status code = 200

Returns all the profile information associated with the person, such as their name, email, and the images used to enroll their identinty.  

### Sandbox API Endpoint
`GET https://sandbox.miicloud.io/miicloud/person`

### Production API Endpoint
`Please contact dev@miicloud.io`


### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
customer_person_id | Yes | a unique identified maintained by the customer per person enrolled in the database

### Response

Field | Required | Description
--------- | ------- | -----------
status | No | only returns "fail" if a matching customer_person_id is not found
customer_person_id | No | the unique identifier maintained by the customer for the given person
miicloud_person_id | No | miiCloud's unique identifier associated with the person whose information is requested
images | No | array of images associated with the given person
image | No | file name 
created | No | timestamp of when the image was enrolled by the customer
first_name | No | first name associated with the person 
last_name | No | last name associated with the person
phone | No | phone # associated with the person
email | No | email associated with the person

<!-----------
===============================================
===============================================
===============================================
----------->

## Delete Person
```python
import requests

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
api_endpoint = 'https://sandbox.miicloud.io/miicloud/person

customer_person_id = 5712f5242cf956c7c1d248e89

response = requests.delete(api_endpoint + '/' + customer_person_id, headers={'Authorization': auth_token})

```

```shell  
curl -X DELETE \
https://sandbox.miicloud.io/miicloud/person/5712f5242cf956c7c1d248e89 \
-H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131'

```
> The above request returns a JSON object structured as follows:

```json
{
    "status":"success",
    "customer_person_id":"5712f5242cf956c7c1d248e89",
    "miicloud_person_id":"059339e9-02d7-4777-95ce-acca440af413"
}

```
>Successfull HTTP status code = 200

Deletes the person record all information associated with it. 

<aside class="warning">
Warning: This action cannot be reversed.
</aside>

### Sandbox API Endpoint
`DELETE https://sandbox.miicloud.io/miicloud/person`

### Production API Endpoint
`Please contact dev@miicloud.io`


### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
customer_person_id | Yes | the unique identifier (maintained by the customer) of the person to be deleted

### Response

Field | Required | Description
--------- | ------- | -----------
status | Yes | "success" if a person with the given "customer_person_id" is found and deleted, otherwise "fail"
customer_person_id | No | the unique identifier of the person who was deleted
miicloud_person_id | No | miicloud's unique identifier associated with the person who was deleted

<!-----------
===============================================
===============================================
===============================================
----------->

## Match Faces
```python
import requests
import os

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
image_path = 'input/test_images/test_001.jpg'
api_endpoint = 'https://sandbox.miicloud.io/miicloud/match_faces'

payload = {'image_path': image_path}
filename = os.path.basename(image_path)
file = {'image_path': (filename, open(image_path, 'rb'), "multipart/form-data")}

response = requests.post(api_endpoint, headers={'Authorization': auth_token}, data = payload, files = file)

```

```shell  
curl -X POST \
https://sandbox.miicloud.io/miicloud/match_faces \
-H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131' \
-F image_path=@input/test_images/test_001.jpg \
-F customer_person_id=8e64b48cf88349e390498c413ad2bbad
  
```
> The above request returns a JSON object structured as follows:

```json
{
    "faces": [
        {
            "match_status": "unknown",
            "bounding_box": {
                "left_x": 414,
                "left_y": -31,
                "right_x": 638,
                "right_y": 218
                },
            "match_confidence": 0
        },
        {
            "match_status": "known",
            "bounding_box": {
                "left_x": 98,
                "left_y": -44,
                "right_x": 420,
                "right_y": 314
                },
            "match_confidence": 0.6426461754214432,
            "miicloud_person_id": "e5db4da3-9d34-4a9d-b517-b203b749962c",
            "customer_person_id": "8e64b48cf88349e390498c413ad2bbad"
        }
    ]
}

```
>Successfull HTTP status code = 200

Matches the faces in a given image against the faces in the database for one or more matches.  

### Sandbox API Endpoint
`POST https://sandbox.miicloud.io/miicloud/match_faces`

### Production API Endpoint
`Please contact dev@miicloud.io`


### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
image_path | Yes | local or remote destination of the image accessible by your application
customer_person_id | No | unique identifer of a person.  This field should be used to conduct a 1:1 verify, meaning if you would like to compared this specific person's enrolled image(s) against the input image.  If this parameter is not passed, miiCloud conducts a 1:many search and compares the faces in the input image against its database for potential matches.

### Response

Field | Required | Description
--------- | ------- | -----------
faces | Yes | returns an array if 1 or more faces is found in the image
match_status | No | if the face(s) in the image matche with 1 or more known person(s) in the database; values are: "known" or "unknown"
bounding_box | No | an object with bounding box coordinates around the face
left_x | No | x-coordinate of the bounding box’s top left corner 
left_ y | No | y-coordinate of the bounding box’s top left corner
right_x | No | x-coordinate of the bounding box’s bottom right corner
right_y | No | y-coordinate of the bounding box’s bottom right corner
match_confidence | No | value between 0 - 1 to reflect how closely the test image matches the enrolled image.  This value should be multipled by 100 to get a %.
person_id | No | unique identifier stored in miiCloud per person.  This identifier links the identity in the customer’s system to miiCloud.
customer_person_id | No | a unique identified maintained by the customer per person enrolled in the database.  miiCloud returns this identifier in the response to allow the customer to validate correct identities were linked

<!-----------
===============================================
===============================================
===============================================
----------->







<!-----------
===============================================
===============================================
===============================================
----------->

