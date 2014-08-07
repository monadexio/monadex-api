FORMAT: 1A

# Monadex API
Monadex API is a **tshirt designer service** similar to [Teespring](http://www.teespring.com).

## Authentication
*Monadex API* uses HTTP Basic Authentication and persistent login sessions to save user credential.

## Media Types
Where applicable this API uses the [JSON](http://en.wikipedia.org/wiki/Json) to represent resources states and affordances.

Requests with a message-body are using plain JSON to set or update resource states.

## Error States
The common [HTTP Response Status Codes](https://github.com/for-GET/know-your-http-well/blob/master/status-codes.md) are used.

# Group Campaign
Campaign-related resources of *Monadex API*.

## Campaign [/campaigns/{id}]
A single Campaign object. The Campaign resource is the central resource in the Monadex API. It represents one tshirt campaign - a single sellable object.

The Campaign resource has the following attributes:

- id
- created_at
- ended_at
- title
- description
- length
- url
- goal
- sold
- reservations
- cost
- price
- design
- orders

The states *id* and *created_at* are assigned by the Monadex API at the moment of creation.

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash.

+ Model (application/json)

    JSON representation of Monadex Resource.

    + Body

            {
                "id": "42",
                "created_at": "2014-04-14T02:15:15Z",
                "ended_at": "2014-08-02T02:15:15Z",
                "title": "Title of Campaign",
                "description": "Description of Campaign",
                "length": "Length of Campaign",
                "url": "URL of Campaign",
                "goal": "Number of tshirts for sale",
                "sold": "Number of tshirts sold",
                "reservations": "Number of tshirts reserved",
                "cost": "Cost of one tshirt",
                "price": "Price of one tshirt",
                "design": "JSON representation of the tshrit design",
                "orders": "Orders of the campaign"
            }

### Retrieve a Single Campaign [GET]
+ Response 200

    [Campaign][]

### Edit a Campaign [PATCH]
To update a Campaign send a JSON with updated value for one or more of the Campaign resource attributes. All attributes values (states) from the previous version of this Campaign are carried over by default if not included in the hash.

+ Request (application/json)

        {
            "design": "Updated tshirt design"
        }

+ Response 200

    [Campaign][]

### Delete a Campaign [DELETE]
+ Response 204

## Campaigns Collection [/campaigns{?since,limit}]
Collection of all Campaigns.

In addition it **embeds** *Campaign Resources* in the Monadex API.

+ Model (application/json)

    JSON representation of Campaign Collection Resource. The Campaign resources in collections are embedded. Note the embedded Campaigns resource are incomplete representations of the Campaign in question. Use the respective Campaign id to retrieve its full representation.

    + Body

            [
                {
                    "id": "42",
                    "created_at": "2014-04-14T02:15:15Z",
                    "ended_at": "2014-08-02T02:15:15Z",
                    "title": "Title of Campaign",
                    "description": "Description of Campaign",
                    "length": "Length of Campaign",
                    "url": "URL of Campaign",
                    "goal": "Number of tshirts for sale",
                    "sold": "Number of tshirts sold",
                    "reservations": "Number of tshirts reserved",
                    "cost": "Cost of one tshirt",
                    "price": "Price of one tshirt"
                }
            ]

### List All Campaigns [GET]
+ Parameters
    + since (optional, string) ... Timestamp in ISO 8601 format: `YYYY-MM-DDTHH:MM:SSZ` Only campaigns updated at or after this time are returned.

+ Response 200

    [Campaigns Collection][]

### Create a Campaign [POST]
To create a new Campaign simply provide a JSON hash of all the attributes except *id* and *created_at*for the new Campaign.

+ Request (application/json)

        {
            "title": "Title of Campaign",
            "description": "Description of Campaign",
            "length": "Length of Campaign",
            "url": "URL of Campaign",
            "goal": "Number of tshirts for sale",
            "cost": "Cost of one tshirt",
            "price": "Price of one tshirt",
            "design": "JSON representation of the tshrit design"
        }

+ Response 201

    [Campaign][]

## End early [/campaigns/{id}/end_early]
End a Campaign early.

The resource has the following attribute:

- ended_at

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash

+ Model (application/json)

    JSON representation of Campaign Resource.

    + Body

            {
                "id": "42",
                "created_at": "2014-08-01T02:15:15Z",
                "ended_at": "2014-08-02T02:15:15Z",
                "title": "Title of Campaign",
                "description": "Description of Campaign",
                "length": "Length of Campaign",
                "url": "URL of Campaign",
                "goal": "Number of tshirts for sale",
                "sold": "Number of tshirts sold",
                "reservations": "Number of tshirts reserved",
                "cost": "Cost of one tshirt",
                "price": "Price of one tshirt",
                "design": "JSON representation of the tshrit design"
            }

### End a Campaign early [PUT]

+ Response 204

## Duplicate [/campaigns/{id}/duplicate]
Duplicate a Campaign.

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash

+ Model (application/json)

    JSON representation of Campaign Resource.

    + Body

            {
                "id": "42",
                "created_at": "2014-08-01T02:15:15Z",
                "ended_at": "2014-08-02T02:15:15Z",
                "title": "Title of Campaign",
                "description": "Description of Campaign",
                "length": "Length of Campaign",
                "url": "URL of Campaign",
                "goal": "Number of tshirts for sale",
                "sold": "Number of tshirts sold",
                "reservations": "Number of tshirts reserved",
                "cost": "Cost of one tshirt",
                "price": "Price of one tshirt",
                "design": "JSON representation of the tshrit design"
            }

### Duplicate a Campaign[PUT]

+ Response 204

## Order [/campaigns/{id}/order]
Place an order in a Campaign.

+ Model (application/json)

    JSON representation of Campaign Resource.

    + Body

            {
                "id": "42",
                "created_at": "2014-08-01T02:15:15Z",
                "quantity": "No of tshirt to buy",
                "size": "Size of tshirt",
                "style": "Style of tshirt",
                "buyer": "Id of the buyer",
                "campaign": "Id of the campaign",
                "status": "reserved"
            }

### Order a Campaign[POST]

+ Request (application/json)

    {
        "quantity": "Number of tshirt to buy",
        "size": "Size of tshirt",
        "style": "Style of tshirt",
        "firstname": "Firstname of buyer,",
        "lastname": "Lastname of buyer,",
        "phone": "Phone of buyer,",
        "address": "Address of buyer",
        "city": "City of buyer",
        "zipcode": "Zipcode of buyer",
        "state": "State of buyer",
        "country": "Country of buyer",
        "email": "Email of buyer",
        "number": "Number of buyer's credit card",
        "exp_month": "Expiration month of the card",
        "exp_year": "Expiration year of the card",
        "cvc": "CVC of the card"
    }

+ Response 200

    [Order][]

# Group Users
Page and filter the users.

## User [/users/{id}]
User Resource represents an user.

The User Resource has the following attribute:

- id
- name
- email
- password

+ Model (application/json)

    + Body

            {
                "id": "37,
                "name": "Name of User",
                "email": "Email of User",
                "password": "Password of User"
            }

### Retrieve User [GET]
+ Response 200

    [User][]

### Remove a User [DELETE]
+ Response 204

## Users Collection [/users{?limit}]
Collection of all Users.

+ Model (application/json)

    JSON representation of Users Collection Resource.

    + Body

            [
                {
                    "id": "37,
                    "name": "Name of User",
                    "email": "Email of User",
                    "password": "Password of User"
                }
            ]

### List All Users [GET]
+ Parameters
        + limit (optional, string) ... Limit number of users listed

+ Response 200

    [Users Collection][]

### Create User [POST]
+ Request (application/json)
    + Body

            {
                "email": "Email of User",
                "password": "Password of User"
            }

+ Response 201

    [User][]
