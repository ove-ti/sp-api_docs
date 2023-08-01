# Here is the basic Information for the amazon project



## First things what we need:

Information:

1. **selling region**: na, eu, fe (us-east-1, eu-east-1, us-west-2)
    - go to: https://developer-docs.amazon.com/sp-api/docs/sp-api-endpoints
2. **Marketplace-Id**: (Canada: A2EUQ1WTGCTBG2, United States of America: ATVPDKIKX0DER)
    - go to: https://developer-docs.amazon.com/sp-api/docs/marketplace-ids

3. **App type**: 
    - Public apps 
    - Private seller apps 
    - Private vendor apps

4. **Register as a developer*:*
    - Go to: https://developer-docs.amazon.com/sp-api/docs/creating-and-configuring-iam-policies-and-entities
    1. Create AWS Account
    - Register as a developer
    - AWS, here we have to make the IAM user account
    - You might want to login from Root account to make the IAM User account from there
    2. Create IAM User account
    - Create the username
    - provide user access to the AWS Management console. 
    - Select I want to create IAM user
    - give it a password 
    - give it permissions
    - download the csv containing credentials
    - Now sign in as the IAM user
    - Create an access key for this user
    - Once created download the csv file with the access key and the secret access key
    3. Create the IAM Policy:
    - Sign in to the AWS Management Console and then open the IAM Console
    - on the navigation pane choose policies, if first time choose Get Started
    - Create Policy 
    - Choose the JSON tab. 
    - and paste the following code:

    ```json
      {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "execute-api:Invoke",
        "Resource": "arn:aws:execute-api:*:*:*"
      }
    ]
    } 
      ```
    - Give you policy a name and a Description
    4. Create the IAM Role
    - on the AWS Management Console, on the IAM Console
    - go on the Roles pane, and choose Create role
    - On the Select Trusted entity page, choose AWS account and select in case the IAM user is on the same account then This account 
    - give the role a name 
    5. Add an AWS STS policy to your IAM user 
    - Again find the IAM user
    - where it says Add permission, choose Create Inline policy 
    - On service choose STS
    - Under actions Write and AssumeRole
    - Under resources chooose Add ARNs
    - give a name to the policy
    6. Now you can create the application:
    - on the seller central site create a new application
    - give it a name 
    - as API type give SP API
    - IAM ARN, go on AWS and under **Roles** select the one that contains arn
 
## Importanto to have to use in python:

SP_API_REFRESH_TOKEN
The refresh token used obtained via authorization (can be passed to the client instead)
LWA_APP_ID
Your login with amazon app id
LWA_CLIENT_SECRET
Your login with amazon client secret
SP_API_ACCESS_KEY
AWS USER ACCESS KEY
SP_API_SECRET_KEY
AWS USER SECRET KEY
SP_API_ROLE_ARN
The role’s arn (needs permission to “Assume Role” STS)

How to get them:

SP_API_REFRESH_TOKEN: This is obtained when you complete the OAuth 2.0 process with Amazon's Selling Partner API. You will need to set up an OAuth 2.0 application in the Amazon Developer Central and then use this to authorize your application. The refresh token is returned as part of the OAuth 2.0 authorization process.

LWA_APP_ID: This is the Login with Amazon (LWA) App ID. You can get this by creating a new security profile in Amazon Developer Central. Once the security profile is created, the LWA App ID is displayed in the security profile details.

LWA_CLIENT_SECRET: This is the Login with Amazon (LWA) Client Secret. It is also obtained when you create a new security profile in Amazon Developer Central. It is displayed along with the LWA App ID in the security profile details.

SP_API_ACCESS_KEY and SP_API_SECRET_KEY: These are your AWS Access Key ID and Secret Access Key. You can get these by creating a new IAM user in your AWS Management Console. When you create the user, be sure to grant them programmatic access so that an access key ID and secret access key are created.

AWS USER ACCESS KEY and AWS USER SECRET KEY: These are the same as the SP_API_ACCESS_KEY and SP_API_SECRET_KEY. They are obtained by creating a new IAM user with programmatic access in your AWS Management Console.

SP_API_ROLE_ARN: This is the Amazon Resource Name (ARN) for the IAM Role that you create in your AWS Management Console. This role should have the necessary permissions to access the Selling Partner API. Once the role is created, you can find the ARN in the role details.

## Fullfillment 
<b>
<span style="color:red">
Required fields json format:
</span>
</b>

```json 

{
  "ShipmentRequestDetails": {
    "AmazonOrderId": "string",
    "ItemList": [
      {
        "OrderItemId": "string",
        "Quantity": "integer"
      }
    ],
    "ShipFromAddress": {
      "Name": "string",
      "AddressLine1": "string",
      "City": "string",
      "StateOrProvinceCode": "string",
      "PostalCode": "string",
      "CountryCode": "string"
    },
    "PackageDimensions": {
      "Length": "decimal",
      "Width": "decimal",
      "Height": "decimal",
      "Unit": "string"
    },
    "Weight": {
      "Value": "decimal",
      "Unit": "string"
    },
    "ShippingServiceOptions": {
      "DeliveryExperience": "string",
      "CarrierWillPickUp": "boolean"
    }
  },
  "ShippingServiceId": "string"
}

```
<b>
<span style="color:red">
Make a request with python:
</span>
</b>


```python 
import requests
import json

url = 'https://api.amazon.com/...'  # Replace with the actual API endpoint
headers = {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN'  # Replace with your actual access token
}
data = {
    # Your JSON data here
}

response = requests.post(url, headers=headers, data=json.dumps(data))

print(response.status_code)
print(response.json())


```

## Inventory feeds:

could be: Direct fullfillment Inventory || fba_inventory 

most likely Vendor Direct fullfillment Inventory 



```json 
{
  "inventory": {
    "sellingParty": {
      "partyId": "your_selling_party_id"
    },
    "isFullUpdate": true,
    "items": [
      {
        "buyerProductIdentifier": "product_id_1",
        "availableQuantity": {
          "amount": 100,
          "unitOfMeasure": "EA"
        },
        "isObsolete": false
      },
      {
        "buyerProductIdentifier": "product_id_2",
        "availableQuantity": {
          "amount": 50,
          "unitOfMeasure": "EA"
        },
        "isObsolete": false
      }
    ]
  }
}

```
Now you have to make the request to the endpoint with the specified warehouse

```py
# Define the endpoint
warehouseId = "CLA1"
url = f"https://sellingpartnerapi-na.amazon.com/vendor/directFulfillment/inventory/v1/warehouses/{warehouseId}/items"

```




- <b>In the case the above fields are not filled, then the amazon api witll return an error </b>

## Extra Information:
<table>
  <tr>
    <td>
      <a href="https://github.com/amzn/selling-partner-api-docs" alt="amazon docs">
        <img src="images\image.png" alt="Image 1" width="300"/>
      </a>
    </td>
    <td>
        <a href="https://github.com/saleweaver/python-amazon-sp-api" alt= "python docs">
      <img src="images\image-1.png" alt="Image 2" width="300"/>
    </td>
  </tr>
</table>


