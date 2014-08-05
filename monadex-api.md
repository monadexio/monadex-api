FORMAT: 1A

# Monadex API
Monadex API is a **tshirt designer service** similar to [Teespring](http://www.teespring.com).

## Authentication
*Monadex API* uses both OAuth and HTTP Basic Authentication.

## Media Types
Where applicable this API uses the [JSON](http://en.wikipedia.org/wiki/Json) to represent resources states and affordances.

Requests with a message-body are using plain JSON to set or update resource states.

## Error States
The common [HTTP Response Status Codes](https://github.com/for-GET/know-your-http-well/blob/master/status-codes.md) are used.

# Group Campaign
Campaign-related resources of *Monadex API*.

## Campaign [/campaigns/{id}{?access_token}]
A single Campaign object. The Campaign resource is the central resource in the Monadex API. It represents one tshirt campaign - a single sellable object.

The Campaign resource has the following attributes:

- id
- created_at
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

The states *id* and *created_at* are assigned by the Monadex API at the moment of creation.

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash.
    + access_token (string, optional) ... Monadex API access token.

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
                "design": "JSON representation of the tshrit design"
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

## Campaigns Collection [/campaigns{?access_token,since,limit}]
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
                    "price": "Price of one tshirt",
                }
            ]

### List All Campaigns [GET]
+ Parameters
    + since (optional, string) ... Timestamp in ISO 8601 format: `YYYY-MM-DDTHH:MM:SSZ` Only campaigns updated at or after this time are returned.

+ Response 200

    [Campaigns Collection][]

### Create a Campaign [POST]
To create a new Campaign simply provide a JSON hash of all the attributes except *id* and *created_at*for the new Campaign.

This action requries an `access_token` with `campaign_write` scope.

+ Parameters
    + access_token (string, optional) ... Monadex API access token.

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

## End early [/campaigns/{id}/end_early{?access_token}]
End a Campaign early.

The resource has the following attribute:

- ended_at

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash
    + access_token (string, optional) ... Monadex API access token.

+ Model (application/hal+json)

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
This action requries an `access_token` with `campaign_write` scope.

+ Response 204

## Duplicate [/campaigns/{id}/duplicate{?access_token}]
Duplicate a Campaign.

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash
    + access_token (string, optional) ... Monadex API access token.

+ Model (application/hal+json)

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
This action requries an `access_token` with `campaign_write` scope.

+ Response 204

# Group Access Authorization and Control
Access and Control of *Monadex API* OAuth token.

## Authorization [/authorization]
Authorization Resource represents an authorization granted to the user. You can **only** access your own authorization, and only through **Basic Authentication**.

The Authorization Resource has the following attribute:

- token
- scopes

Where *token* represents an OAuth token and *scopes* is an array of scopes granted for the given authorization. At this moment the only available scope is `campaign_write`.

+ Model (application/hal+json)

    + Headers

            Link: <http:/api.monadex.io/authorizations/1>;rel="self"

    + Body

            {
                "_links": {
                    "self": { "href": "/authorizations" },
                },
                "scopes": [
                    "campaign_write"
                ],
                "token": "abc123"
            }

### Retrieve Authorization [GET]
+ Request
    + Headers

            Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

+ Response 200

    [Authorization][]

### Create Authorization [POST]
+ Request (application/json)
    + Headers

            Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

    + Body

            {
                "scopes": [
                    "campaign_write"
                ]
            }

+ Response 201

    [Authorization][]

### Remove an Authorization [DELETE]
+ Request
    + Headers

            Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

+ Response 204
