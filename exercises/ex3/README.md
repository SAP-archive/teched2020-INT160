# Exercise 1 - Exercise 1 Description

In this exercise, we will add the actual functionality for receiving the tracking information of a parcel and display it in the response of the chatbot. We will do this in two different ways, first using a webhook and a custom built node.js service and afterwards purely with the capabilities of SAP Conversational AI without any additional code.

## Exercise 1.1 Use a webhook to connect to an existing node.js service

Follow the tutorial [Add Webhook to Chatbot to Retrieve Tracking Info](https://developers.sap.com/tutorials/cai-bot-shipping-2-api.html) in order to add a webhook to your chatbot built in previous excercises.



## Exercise 1.2 Replace webhook with API Service Configuration


1.	Delete the webhook configuration and click on Connect External Service - Consume API Service
2. Select POST and enter the URL https://wwwcie.ups.com/rest/Track
3. Instrad of connecting to the node.js application that handles the mapping for us, we are now connecting directly to the UPS API.

In the Body tab, paste the following JSON body:
```json
{
    "UPSSecurity": {
        "UsernameToken": {
            "Username": "ysu_deliverybot",
            "Password": "190819&Jiqiren4TEST"
        },
        "ServiceAccessToken": {
            "AccessLicenseNumber": "0D6985C32BF33012"
        }
    },
    "TrackRequest": {
        "Request": {
            "RequestOption": "1", 
            "TransactionReference": {
                "CustomerContext": "Your Test Case Summary Description"
            } 
        },
        "InquiryNumber": "{{memory.parcel-number.raw}}"
    }
}
```

4.	Click on "Save"

## Exercise 1.3 Show a list with tracking info
5. Click on "Add new message group" at the very bottom
6. Click on "Add Condition" and in input field after the "If", enter
`_api_service_response.data.body.TrackResponse`
7. Click on Save and select "is-present" in the second field appearing
8. Directly below the condition you just added, click on "Send Message" and select "Custom"
9. In the "Response Script" editor, paste the following
```
{
  "type": "list",
  "content": {
    "title": "Tracking History",
    "elements": [
    {{#eachJoin api_service_response.data.body.TrackResponse.Shipment.Package.Activity}}
      {
        "title": "{{Status.Description}}",
        "subtitle": "on {{lookup Date 6}}{{lookup Date 7}}.{{lookup Date 4}}{{lookup Date 5}}.{{lookup Date 0}}{{lookup Date 1}}{{lookup Date 2}}{{lookup Date 3}}",
        "description": "{{ActivityLocation.Address.City}}, {{ActivityLocation.Address.CountryCode}}"
      }
      {{/eachJoin}}
    ]
  }
}
```
10. Click on Save


## Exercise 1.4 Show the error message when receiving tracking info failed
1. Click on "Add new message group" at the very bottom
2. Click on "Add Condition" and in input field after the "If", enter
`_api_service_response.data.body.Fault`
3. Click on Save and select "is-present" in the second field appearing
4. Directly below the condition you just added, click on "Send Message" and select "Text"
5. Enter the following text:
```
Sorry I could not find any information with the offered number. I met the error: {{api_service_response.data.body.Fault.detail.Errors.ErrorDetail.PrimaryErrorCode.Description}}
```
6. Click on Save

## Exercise 1.5 Test your bot
1. Now you can test your bot again by clicking on "Chat Preview" in the bottom right corner.
2. Enter "track parcel 1Z74A08E6895571431"
3. A list with the tracking history should be displayed


## Summary

You've now first used a webhook to connect to a custom built middleware but also added the API calls for tracking the parcel directly in your SAP Conversational AI chatbot with the low-code approach.

