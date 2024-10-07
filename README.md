 

## **Retrieving Access Tokens**

 
 - ***Method: getClientToken***

To obtain an access token, send a GET request to the **`{{BASE_URL}}/clients/token`** endpoint. If the request is successful, you will receive a JSON response with a status code of 200 and a content type of `application/json`.
 

## Request
  
  

    curl --location '{{BASE_URL}}/clients/token' \
        --header 'Authorization: Basic base64(login:password)'

## The structure of the response body is as follows

 

    {
      "code": 200,
      "message": "OK",
      "data": {
        "access_token": ""
      }
    }

## Handling Invalid Credentials
If the credentials provided are incorrect, the API will return a response with a status code of **401**. The response schema in this case is as follows:

    {
      "code": 401,
      "message": "Authorization required to do specific action\nДля выполнения определенных действий требуется авторизация",
      "error": "UNAUTHORIZED",
      "path": "/api",
      "timestamp": 1728224040208
    }

## **Upload Client Devices**

 - ***Method: uploadDevice***
 
 This endpoint allows the client to upload a list of devices. Use POST method.
 
#### Request Body
 -   `deviceList` (array of objects, required): An array of device objects containing device UIDs.
-   `deviceUid` (string, required): The unique identifier of the device.
-   `autoAccept` (boolean, required): A boolean value indicating whether the devices should be automatically accepted.

## Example Request

    curl --location '{{BASE_URL}}/clients/devices/uploads' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
      "deviceList": [
        {
          "deviceUid": "imei device"
        }
      ],
      "autoAccept": true
    }'
## Response
The response will be in the form of a JSON schema:

    {
        "result": "SUCCESS",
        "uploadId": "",
        "uploadStatus": "Progress",
        "totalDeviceCount": 1,
        "validDeviceCount": 0,
        "invalidDeviceCount": 1,
        "invalidReasons": [
            {
                "deviceUid": "",
                "reason": "device already exists"
            }
        ]
    }

## Approve Client Devices

**Method: *approveDevice***

This endpoint is designed to approve client devices. To use this feature, send a **POST** request to the `{{BASE_URL}}/clients/devices/approve` endpoint.
#### Request Body
The request must include the following field:

-   **deviceUid** (string, required): The unique identifier for the device that you wish to approve.
## Example Request

    curl --location 'https://api.neolock.uz/api/clients/devices/approve' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "deviceUid": "imei"
    }'

 

## Successful Response

Upon successful approval, you will receive a response with the following structure:

    {
      "result": "SUCCESS",
      "objectId": "",
      "requestedId": "",
      "transactionId": "",
      "code": 2000000,
      "message": "SUCCESS"
    }

## Error Response
If there is an error during the approval process, the response will contain an error object with the following structure:

    {
      "result": "FAIL",
      "error": {
        "code": 4000310,
        "message": "DEVICE_STATE_INVALID",
        "reason": "Current status is 'Enrolled'."
      }
    }

The error object provides important information, including:

-   **code**: The specific error code.
    
-   **message**: A brief description of the error.
    
-   **reason**: Detailed information about the error reason.

## lockDevice

This endpoint allows you to lock a client's device by sending an HTTP POST request to `{{BASE_URL}}/clients/devices/lock`. The request should include the deviceUid, message, and tel in the raw request body.

### Request Body

-   `deviceUid` (string): The unique identifier of the device to be locked.
    
-   `message` (string): The message to be sent along with the lock request.
    
-   `tel` (string): The telephone number associated with the device.
## Example Request

    curl --location '{{BASE_URL}}/clients/devices/lock' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "deviceUid": "",
        "message": "your lock message",
        "tel":"your phone number"
    }'
### Response
Upon successful execution, the endpoint returns a 200 status with a JSON response containing the following fields:

-   `result` (string): The result of the lock operation.
    
-   `objectId` (string): The ID of the locked object.
    
-   `requestedId` (string): The ID of the lock request.
#### Example Response

    {
      "result": "success",
      "objectId": "locked_object_id",
      "requestedId": "lock_request_id"
    }
If deviceUid is not found, this response will be returned

    {
        "code": 404,
        "message": "Device not found with {deviceUid}\nУстройство не найдено с {deviceUid}",
        "error": "NOT_FOUND",
        "path": "/api",
        "timestamp": 1728225028579
    }

## unlockDevice
This endpoint allows you to send an HTTP POST request to unlock a client's device. The request should be sent to **`{{BASE_URL}}/clients/devices/unlock`**.
### Request Body

-   `deviceUid` (string): The unique identifier of the device to be unlocked.
    
-   `message` (string): A message related to the unlock request.
    
-   `tel` (string): The telephone number associated with the device.
## Example Request

    curl --location '{{BASE_URL}}/clients/devices/unlock' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "deviceUid": "",
        "message": "your unlocked message",
        "tel":"your phone number"
    }'
 
### Response

Upon successful execution, the endpoint returns a status code of 200 and a JSON response with the following fields:

-   `result` (string): The result of the unlock operation.
    
-   `objectId` (string): The identifier of the unlocked object.
    
-   `requestedId` (string): The identifier of the unlock request.

## Example

    {
	    "deviceUid": "",
	    "message": "your unlocked message",
	    "tel":"your phone number"
    }

## Add Offline Device Lock Pin
*Method: **POST** getPinWithPasskey*.

This endpoint allows the client to add an offline lock pin for a specific device.

#### Request Body

-   `deviceUid` (string): The unique identifier of the device.
    
-   `challenge` (string): The challenge for adding the offline lock pin.

## Example

    curl --location '{{BASE_URL}}/clients/devices/offlineDeviceLockPin' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "deviceUid":"",
        "challenge":"code" }'

#### Response

-   `result` (string): The result of the operation.
-   `objectId` (string): The identifier of the object.
-   `requestedId` (string): The requested identifier.
-   `pinNumber` (array of strings): The offline lock pin number.

>      {
>        "result": "SUCCESS",
>       "objectId": "67026fc1c5217554fe25dc42",
>        "requestedId": "",
>         "pinNumber": [
>               "09162095"
>           ]
>       }

## Send Message to Device
*Method: **POST** sendMessageToDevice*.

This endpoint allows you to send a message to a specific device.

#### Request Body

-   `deviceUid` (string, required): The unique identifier of the device.
    
-   `message` (string, required): The message to be sent to the device.

>     curl --location '{{BASE_URL}}/clients/devices/sendMessage' \
>     --header 'Content-Type: application/json' \
>     --header 'x-knox-apitoken: token' \
>     --data '{
>         "deviceUid": "",
>         "message":"your message"
>     }'
#### Response

Upon successful execution, the endpoint returns a JSON object with the following fields:

-   `result` (string): The result of the message sending operation.
    
-   `objectId` (string): The object identifier.
    
-   `requestedId` (string): The requested identifier.

## Example

    {
        "result": "SUCCESS",
        "objectId": "67026fc1c5217554fe25dc42",
        "requestedId": ""
    }

## Get Client Device Details
*Method: **GET** getDeviceInfoByDeviceUid*

This endpoint retrieves the details of a specific client device identified by its unique ID.

## **Request**

    curl --location '{{BASE_URL}}/clients/devices/{deviceUid}' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token'

## **Response** -   Status: 200


    {
        "result": "",
        "device": {
            "objectId": "",
            "deviceUid": "",
            "status": "",
            "createDate": 0,
            "modifiedDate": 0,
            "simControlEnabled": true,
            "simControlApplied": true,
            "latestRelockApplied": true,
            "autoLockTarget": true,
            "agentVersion": "",
            "imei": "",
            "imei2": "",
            "licenseExpiryDate": 0,
            "firmwareVersion": "",
            "deviceSupportHotp": true,
            "offlineLocked": true,
            "offlineLockApplied": true,
            "simControlLocked": true
        }
    }
## **Response** -   Status: 404

    {
        "code": 404,
        "message": "Device not found with {deviceUid}\nУстройство не найдено с {deviceUid}",
        "error": "NOT_FOUND",
        "path": "/api",
        "timestamp": 1728224276773
    }

## Get Client Devices Uploads
*Method: **GET** getUploadedDevicesInfo*

This endpoint retrieves a list of client device uploads with the option to paginate the results.

## Request

### Query Parameters

-   `pageNum` (integer) - The page number for pagination.
    
-   `pageSize` (integer) - The number of items per page.

## Example

    curl --location '{{BASE_URL}}/clients/devices/uploads?pageNum=0&pageSize=100' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token'

## Response
Upon a successful request, the server will respond with a status code of 200 and a JSON object with the following structure:

-   `code` (integer) - The status code of the response.
    
-   `message` (string) - Additional information or error message.
    
-   `data` (array) - An array containing the details of client device uploads.
    
    -   `id` (integer) - The unique identifier of the device upload.
        
    -   `deviceUid` (string) - The unique identifier of the device.
        
    -   `imei` (string) - The International Mobile Equipment Identity (IMEI) number of the device.
        
    -   `imei2` (string) - The second IMEI number of the device.
        
    -   `serial` (string) - The serial number of the device.
        
    -   `model` (string) - The model of the device.
        
    -   `uploadID` (string) - The identifier of the upload.
        
    -   `uploadStatus` (string) - The status of the upload.
        
    -   `status` (string) - The overall status of the device.

## Example

    {
      "code": 0,
      "message": "",
      "data": [
        {
          "id": 0,
          "deviceUid": "",
          "imei": "",
          "imei2": "",
          "serial": null,
          "model": null,
          "uploadID": "",
          "uploadStatus": "",
          "status": ""
        }
      ]
    }

## complete Device
*Method: **POST** completeDevice*

## Example request

    curl --location '{{BASE_URL}}/clients/devices/complete' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "deviceUid": "deviceUid"
    }'
## delete Device
*Method: **POST** deleteDevice*

## Example request

    curl --location '{{BASE_URL}}/clients/devices/delete' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "deviceUid": "deviceUid"
    }'
## _create Subscription_
*Method: **POST** createSubscription*
This endpoint is used to create a new subscription for clients.
#### Request

-   Body (raw, JSON):
    
    -   url (string, required): The URL for the subscription.

## Example

    curl --location '{{BASE_URL}}/subscriptions/clients' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token' \
    --data '{
        "url":"your url"
    }'
#### Response

-   Status: 201
    
-   Content-Type: application/json
    
-   { "code": 0, "message": "", "data": "" }
## delete Subscription
*Method: **POST** deleteSubscription*

This endpoint is used to delete subscriptions for clients.

#### Request Body

This request does not require a request body.

## Example


     curl --location --request DELETE '{{BASE_URL}}/subscriptions/clients' \
    --header 'Content-Type: application/json' \
    --header 'x-knox-apitoken: token'
        }'


#### Response

The response for this request is in JSON format and has the following schema:

## Example

    {
        "code": 200,
        "message": "Subscription deleted"
    }
