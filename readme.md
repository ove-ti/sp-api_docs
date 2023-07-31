# Here is the basic Information for the amazon project


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
