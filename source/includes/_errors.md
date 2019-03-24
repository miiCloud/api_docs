# Errors

miiCloud API uses the following error codes.  Please contact dev@miicloud.io if you experience any issues.


Error Code | Description
---------------- | -------
400 | Bad Request -- The request has 1 or more incorrect parameters.  Double check the correct parameters are being passed in the request.
401 | Unauthorized -- The API key is invalid.  
403 | Forbidden -- The requested object is only available with administrators access.
404 | Not Found -- The specified API / object could not be found.
405 | Method Not Allowed -- An invalid methos was used to access the API.
406 | Not Acceptable -- The request was not formatted in JSON.
415 | Unsupported Media Type -- The file format of the image / video is not supported by the API.

