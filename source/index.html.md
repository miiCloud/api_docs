---
title: miiCloud API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  
toc_footers:
  - <a href="mailto:dev@miicloud.io?Subject=Create%20Sandbox%20Dev%20Token">Request a Developer Token</a>
  - <a href='https://github.com/lord/slate'>Docs Powered by Slate</a>

includes:
  - errors

search: true
---

# Overview

Welcome!

miiCloud is a Computer Vision company that leverages Deep Learning for image and video analysis.  We train and calibrate our models on an extensive data pipeline to bring the best technology to our customers.  

This guide provides an overview of using the miiCloud Application Programming Interface (API), related operations, request and response structures and sample code.  The API is organized around REST and returns JSON encoded responses. The current version of the API is 20190315v02

We will continue to add new APIs to this site on a weekly basis.  If you have specific requests for your business, please contact us: dev@miicloud.io

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

## Enroll a Person
```python
import requests

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
enrollment_payload = 
{ 
    'customer_person_id': '1234567', 
    'image_path': 'usr/local/app/data/image0001.jpg', 
    'first_name': 'John', 
    'last_name': 'Doe', 
    'email': 'test@domain.com', 
    'phone': '0123456789' 
    
}

requests.post(api_endpoint, headers={'Authorization': auth_token}, json = enrollment_payload)
```

```shell
curl -X POST \
api_endpoint \
-H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131' \
-F customer_person_id=1234567
-F image_path= usr/local/app/data/image0001.jpg \
-F first_name=John \
-F last_name=Doe \
-F email=test@domain.com \
-F phone=0123456789 \
```

Identifies the largest face in a given image and adds it to the database.  If a human face is not found, it returns a “fail” status.  In order to provide high accuracy for subsequent matches, during enrollment, a face must be presented with a frontal pose with area of the face consisting of eye brows, eyes, nose and lips. 

> The above request returns a JSON object structured as follows:

```json
{
    "status": "success",
    "bounding_box": {
    "left_x": 379,
    "left_y": 260,
    "right_x": 647,
    "right_y": 529
    },
    "person_id": "3a298260-6986-41d4-972e-6f0910293362",
    "customer_person_id": "1234567"
}

```

### Sandbox API Endpoint
`POST http://3.92.92.102/miicloud/enroll`
### Production API Endpoint
`Please contact dev@miicloud.io`

### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
customer_person_id | Yes | a unique identified maintained by the customer per person enrolled in the database.  This identifier allows you link multiple images of an individual to their identity and link other identifiable info such as name, etc.
image_path | Yes | local or remote destination of the image accessible by your application
first_name | No | first name associated with the person being added to database
last_name | No | last name associated with the person being added to database
email | No | email associated with the person being added to database
phone | No | phone # associated with the person being added to database

### Response 

Field | Required | Description
--------- | ------- | -----------
status | Yes | response by API whether it was successful or not
bounding_box | No | an object with bounding box coordinates around the face
left_x | No | x-coordinate of the bounding box’s top left corner 
left_ y | No | y-coordinate of the bounding box’s top left corner
right_x | No | x-coordinate of the bounding box’s bottom right corner
right_y | No | y-coordinate of the bounding box’s bottom right corner
person_id | No | unique identifier stored in miiCloud per person.  This identifier links the identity in the customer’s system to miiCloud.
customer_person_id | No | a unique identified maintained by the customer per person enrolled in the database.  miiCloud returns this identifier in the response to allow the customer to validate correct identities were linked



## Find Matching Faces


```python
import requests

auth_token = 'Token 0e653da4969a757d21de0ffa6387d5fbd6401131'
match_faces_payload = 
{ 
    'image_path': 'usr/local/app/data/test_img_0001.jpg', 
}

requests.post(api_endpoint, headers={'Authorization': auth_token}, json = enrollment_payload)
```

```shell  
curl -X POST \
  api_endpoint \
  -H 'authorization: Token 0e653da4969a757d21de0ffa6387d5fbd6401131' \
  -F image_path= usr/local/app/data/test_img_0001.jpg
  
```
> The above request returns a JSON object structured as follows:

```json
{
    "status": "success",
    "faces": [
        {
        "match_status": "known",
        "bounding_box": {
            "left_x": 464,
            "left_y": 291,
            "right_x": 688,
            "right_y": 515
            },
        "person_id": "3a298260-6986-41d4-972e-6f0910293362",
        "customer_person_id": "1234567",
        }
    ]
}

```

Matches the faces in a given image against the faces in the database for one or more matches.  

### Sandbox API Endpoint
`POST http://3.92.92.102/miicloud/match_faces`

### Production API Endpoint
`Please contact dev@miicloud.io`


### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
authorization | Yes | a unique api token per miiCloud customer
image_path | Yes | local or remote destination of the image accessible by your application

### Response

Field | Required | Description
--------- | ------- | -----------
status | Yes | response by API whether it was successful or not
faces | No | returns an array if 1 or more faces is found in the image
match_status | No | if the face(s) in the image matche with 1 or more known person(s) in the database; values are: "known" or "uknown"
bounding_box | No | an object with bounding box coordinates around the face
left_x | No | x-coordinate of the bounding box’s top left corner 
left_ y | No | y-coordinate of the bounding box’s top left corner
right_x | No | x-coordinate of the bounding box’s bottom right corner
right_y | No | y-coordinate of the bounding box’s bottom right corner
person_id | No | unique identifier stored in miiCloud per person.  This identifier links the identity in the customer’s system to miiCloud.
customer_person_id | No | a unique identified maintained by the customer per person enrolled in the database.  miiCloud returns this identifier in the response to allow the customer to validate correct identities were linked

