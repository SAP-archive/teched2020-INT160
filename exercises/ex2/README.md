# Exercise 2 - Exercise 2 Description

In this exercise, we will build a bot that fetches a list of customers from an OData service and shows it to the user as a list.

## Exercise 2.1 Create the new bot "northwind-bot".

1. Navigate to https://cai.tools.sap and login

2. Click on "+ New bot"

3. Select "Perform Actions"

4. Select "Greetings" as predefined skill

2.	Enter "northwind-bot" as bot name

1. Select "Personal" as Data Policy

1. Select "Store"

1. Select "Non-vulnerable"

1. Select "Private" as the bots visibility

1. Click on "Create a Bot"

## Exercise 2.2 Create intent and skill for showing customers

After completing these steps the bot will recognize that the user wants to show a list of customers and trigger the corresponding skill.

1. Click on "+ Create" to create a new intent
1. Enter "show-customers" as the name and click on "Create Intent"
1. Click on the newly created intent and add some expressions: e.g.
   - show customers
   - I want to see a list of customers
   - view all customers
1. Navigate to the "Build" tab
1. Click on "+ Add skill"
1. Enter "show customers" as the name and click on "Add"
1. CLick on the newly created skill
1. Select the "Triggers" tab
1. Click into the field after "If" and select "@show-customers"
1. Click on "Save"

## Exercise 2.2 Create intent and skill for showing customers

1. Navigate to the "Actions" tab of your "show-customers" skill

1. Click on "Add new message group"

1. Click on "Connect External Service" and choose "Consume API Service"

1. Select "GET" as the HTTP type instead of the pre-selected "POST"

1. Enter in the URL field: `https://services.odata.org/V2/Northwind/Northwind.svc/Customers`

1. Click on "Save"

1. Click on "Send Message" and select "Custom"

1.	In the "Response Script" editor, enter the following snippet:
```handlebars
{
  "type": "list",
  "content": {
    "title": "Customers",
    "elements": [
    {{#eachJoin api_service_response.default.body.d.results}}
      {
        "title": "{{CompanyName}}",
        "subtitle": "{{City}}, {{Country}}",
        "buttons": [
          {
            "title": "Navigate",
            "value": "https://www.google.de/maps/search/{{Address}}, {{City}}, {{Country}}",
            "type": "web_url"
          }
        ]
      }
      {{/eachJoin}}
    ]
  }
}
```

## Exercise 2.3 Test your bot

1. Click on "Chat Preview" in the bottom right corner

2. Enter "show customers" and press Enter

3. A list of customers should be displayed as well as a "Navigate" button to open the address in Google Maps

## Summary

You've now created a simple chatbot that fetches customer details from an OData service and displays it in the SAP Conversational AI Web Client without the need to deploy an own service and write custom code.
