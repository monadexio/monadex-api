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

## Campaign [/campaign/{id}{?access_token}]
A single Campaign object. The Campaign resource is the central resource in the Monadex API. It represents one tshirt campaign - a single sellable object.

The Campaign resource has the following attributes:

- id
- created_at
- title
- description
- length
- url
- content

The states *id* and *created_at* are assigned by the Monadex API at the moment of creation.

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash.
    + access_token (string, optional) ... Monadex API access token.

+ Model (application/json)

    JSON representation of Monadex Resource. In addition to representing its state in the JSON form it offers affordances in the form of the HTTP Link header.

    + Headers

            Link: <http:/api.monadex.io/campaigns/42>;rel="self", <http:/api.monadex.io/campaigns/42/star>;rel="star"

    + Body

            {
                "_links": {
                    "self": { "href": "/campaigns/42" },
                    "star": { "href": "/campaigns/42/star" },
                },
                "id": "42",
                "created_at": "2014-04-14T02:15:15Z",
                "title": "Title of Campaign",
                "description": "Description of Campaign",
                "length": "Length of Campaign",
                "url": "URL of Campaign",
                "content": "String contents"
            }

### Retrieve a Single Campaign [GET]
+ Response 200

    [Campaign][]

### Edit a Campaign [PATCH]
To update a Campaign send a JSON with updated value for one or more of the Campaign resource attributes. All attributes values (states) from the previous version of this Campaign are carried over by default if not included in the hash.

+ Request (application/json)

        {
            "content": "Updated file contents"
        }

+ Response 200

    [Campaign][]

### Delete a Campaign [DELETE]
+ Response 204

## Campaigns Collection [/campaigns{?access_token,since}]
Collection of all Campaigns.

The Campaign Collection resource has the following attribute:

- total

In addition it **embeds** *Campaign Resources* in the Monadex API.

+ Model (application/hal+json)

    HAL+JSON representation of Campaign Collection Resource. The Campaign resources in collections are embedded. Note the embedded Campaigns resource are incomplete representations of the Campaign in question. Use the respective Campaign link to retrieve its full representation.

    + Headers

            Link: <http:/api.monadex.io/campaigns>;rel="self"

    + Body

            {
                "_links": {
                    "self": { "href": "/campaigns" }
                },
                "_embedded": {
                    "campaigns": [
                        {
                            "_links" : {
                                "self": { "href": "/campaigns/42" }
                            },
                            "id": "42",
                            "created_at": "2014-04-14T02:15:15Z",
                            "title": "Title of Campaign",
                            "description": "Description of Campaign",
                            "length": "Length of Campaign",
                            "url": "URL of Campaign",
                            "content": "String contents"
                        }
                    ]
                },
                "total": 1
            }

### List All Campaigns [GET]
+ Parameters
    + since (optional, string) ... Timestamp in ISO 8601 format: `YYYY-MM-DDTHH:MM:SSZ` Only campaigns updated at or after this time are returned.

+ Response 200

    [Campaigns Collection][]

### Create a Campaign [POST]
To create a new Campaign simply provide a JSON hash of the *description* and *content* attributes for the new Campaign.

This action requries an `access_token` with `campaign_write` scope.

+ Parameters
    + access_token (string, optional) ... Monadex API access token.

+ Request (application/json)

        {
            "description": "Description of Campaign",
            "content": "String content"
        }

+ Response 201

    [Campaign][]

## Star [/campaigns/{id}/star{?access_token}]
Star resource represents a Campaign starred status.

The Star resource has the following attribute:

- starred

+ Parameters
    + id (string) ... ID of the campaign in the form of a hash
    + access_token (string, optional) ... Monadex API access token.

+ Model (application/hal+json)

    HAL+JSON representation of Star Resource.

    + Headers

            Link: <http:/api.monadex.io/campaigns/42/star>;rel="self"

    + Body

            {
                "_links": {
                    "self": { "href": "/campaigns/42/star" },
                },
                "starred": true
            }

### Star a Campaign [PUT]
This action requries an `access_token` with `campaign_write` scope.

+ Response 204

### Unstar a Campaign [DELETE]
This action requries an `access_token` with `campaign_write` scope.

+ Response 204

### Check if a Campaign is Starred [GET]
+ Response 200

    [Star][]

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
