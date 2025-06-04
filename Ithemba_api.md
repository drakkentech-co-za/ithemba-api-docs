# External api
## Meta
This document has been updated at 2024-07-05
## Contact
Please contact shane.vanniekerk@drakkentech.co.za
## Authentication
Add the following header to your API calls to authenticate
```
Authorization: Bearer your-token-here
```
The token is configured in the production database table name bearer_token sid 1
## Format
The API expects data to be posted in JSON format and will reply with JSON data.
The `Content-Type: application/json` header should be set on requests.
## Location
The API is available at https://ithemba.drakkentech.co.za/api
## Endpoints
Please note the trailing slashes on the URLs.
### Health Check
Health check to see whether the API is online and authentication succeeds.
Request:
```
GET /health-check
```
Response:
```
{"message": "The server is alive!"}
```
# SEND A MESSAGE

Request
```
POST /send_message
{
    "message": "str",
    "title": "str",
    "message_type_fk": "int",
    "message_subject": "str",
    "body_html": "str",
    "resend": "Optional[int]",
    "unit_list": Optional["List"],
    "mda_user_id": Optional["List"],
    "sid": "Optional[int]"
    "sent_from": "Optional[int]"
    "message_importance_sid": "int"
  }
```
```
Field Name               | Description
-------------------------|--------------------------------------------------------------------
message                  | Content of the message you're sending.
title                    | Brief description or subject of the message.
message_type_fk          | Type of message: [1] Normal message, [2] Automated message, [3] Scheduled message.
message_subject          | Subject of the message (can be same as title).
body_html                | HTML body of the content you're sending.
time_limit               | How long should I wait between sending messages to a user? 0 will be instant. 
resend                   | Flag for resending Firebase notifications (optional).
unit_list                | List of units to which the message will be sent. Example: [{"unit_number": "the_unit_number"}].
mda_user_id              | List of units to which the message will be sent. Example: [{"user_id": "user_id"}].
sid                      | SID of the message if you're sending a resend (optional).
sent_from                | default value is 1, but can be set to any integer.
message_importance_sid   | 1-Normal, 2-Important, 3-Very Important.
```
Success Response
```
{"messages": "sent successfully"}
```
Note only one option must be present in the body (unit_list or mda_user_id)

Validation Error Response (400):
```
{"error": "requires only one field user id or unit id or mda user id"}
{"error": "No users found for the specified unit"}
{"error": "Error: Could not find mda user"}
{"error": "No user_id_list, unit_list or mda_user_id provided"}
{"error": "Bearer Token does not exist"}
```

Error Response (500):
```
{"error": "An unexpected error occurred"}
```
