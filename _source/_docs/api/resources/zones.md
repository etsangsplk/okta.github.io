# Zones API

Zones are used to group IP Address ranges in a meaningful way such that authentication policy decisions can be made based on the zone a client's IP belongs to.

This document covers details about API endpoint used to create and mange zones.

## IP Zone data model

~~~json
{
  "type": "IP",
  "id": "nzouagptWUz5DlLfM0g3",
  "name": "newNetworkZone",
  "status": "ACTIVE",
  "created": "2017-01-24T19:52:34.000Z",
  "lastUpdated": "2017-01-24T19:52:34.000Z",
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "1.2.3.4/24"
    },
    {
      "type": "RANGE",
      "value": "2.3.4.5-2.3.4.15"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "2.2.3.4/24"
    },
    {
      "type": "RANGE",
      "value": "3.3.4.5-3.3.4.15"
    }
  ],
  "_links": {
    "self": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzouagptWUz5DlLfM0g3",
      "hints": {
        "allow": [
          "GET",
          "PUT",
          "DELETE"
        ]
      }
    },
    "deactivate": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzouagptWUz5DlLfM0g3/lifecycle/deactivate",
      "hints": {
        "allow": [
          "POST"
        ]
      }
    }
  }
}
~~~

|Field Name | Description | DataType | Required | MaxLength |
|----------|-------------|----------|----------|-----------|
|type | Type of the zone (currently it can only be IP) | String | |
|id   | Unique identified for this zone                | String | |
|name | Unique name for this zone                      | String | 128 |
|gateways | IP addresses (range or cidr form) of the client originating the request that would be treated as being in this zone | Array | 150 |
|proxies | IP addresses (range or cidr form) of the device that forwards the request from clients. | Array | 150 |
|gateway.type | Format of the value - either CIDR or RANGE | String | |
|gateway.value | Value in CIDR/RANGE form depending on the type specified | String | |
|proxy.type | Format of the value - CIDR or RANGE | String | |
|proxy.value | Value in CIDR/RANGE form depending on the type specified | String | |

### Creating IP Zone

#### Successful create
~~~sh
curl -X POST 
-H "Accept: application/json"\
-H "Content-Type: application/json" \
-d '{
  "type": "IP",
  "id": null,
  "name": "newNetworkZone",
  "status": "ACTIVE",
  "created": null,
  "lastUpdated": null,
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "1.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "2.3.4.5/24"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "2.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "3.3.4.5/24"
    }
  ]
}' "https://yourorg.okta.com/api/v1/org/zones/"
~~~

#### Response (Success)

~~~json
{
  "type": "IP",
  "id": "nzouagptWUz5DlLfM0g3",
  "name": "newNetworkZone",
  "status": "ACTIVE",
  "created": "2017-01-24T19:52:34.000Z",
  "lastUpdated": "2017-01-24T19:52:34.000Z",
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "1.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "2.3.4.5/24"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "2.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "3.3.4.5/24"
    }
  ],
  "_links": {
    "self": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzouagptWUz5DlLfM0g3",
      "hints": {
        "allow": [
          "GET",
          "PUT",
          "DELETE"
        ]
      }
    },
    "deactivate": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzouagptWUz5DlLfM0g3/lifecycle/deactivate",
      "hints": {
        "allow": [
          "POST"
        ]
      }
    }
  }
}
~~~

#### Invalid zone creation request

~~~sh
curl -X POST 
-H "Accept: application/json"\
-H "Content-Type: application/json" \
-d '
{
  "type": "IP",
  "id": null,
  "name": "kgysngljmtiaeibcmpavhytgfevfbwjdpnmqnujmhdlgiiergvepikjmacdxmfntpfnarrlnisfpfyfbfehdvibuwgwgwhwhcxknenlrbdsebsgaernokhvaepdbdffdzbtn",
  "status": "ACTIVE",
  "created": null,
  "lastUpdated": null,
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "1.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "2.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "3.4.5.6-3.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "4.5.6.7-4.5.6.9"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "2.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "3.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "4.4.5.6-4.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "5.5.6.7-5.5.6.9"
    }
  ]
}
' "https://yourorg.okta.com/api/v1/org/zones/"
~~~

#### Response (Error case)
~~~json
{
  "errorCode": "E0000001",
  "errorSummary": "Api validation failed: name",
  "errorLink": "E0000001",
  "errorId": "oae0kFnvOhjRdKfRFgxaGaoUg",
  "errorCauses": [
    {
      "errorSummary": "name: The field is too long"
    }
  ]
}
~~~

#### Update existing zone

~~~sh
curl -X PUT 
-H "Accept: application/json"\
-H "Content-Type: application/json" \
-d '
{
  "type": "IP",
  "id": "nzovw2rFz2YoqmvwZ0g3",
  "name": "UpdatedNetZone",
  "status": "ACTIVE",
  "created": "2017-01-24T19:53:28.000Z",
  "lastUpdated": "2017-01-24T19:53:28.000Z",
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "10.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "12.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "13.4.5.6-13.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "14.5.6.7-14.5.6.9"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "12.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "13.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "14.4.5.6-14.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "15.5.6.7-15.5.6.9"
    }
  ],
  "_links": {
    "self": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovw2rFz2YoqmvwZ0g3",
      "hints": {
        "allow": [
          "GET",
          "PUT",
          "DELETE"
        ]
      }
    },
    "deactivate": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovw2rFz2YoqmvwZ0g3/lifecycle/deactivate",
      "hints": {
        "allow": [
          "POST"
        ]
      }
    }
  }
}
' "https://yourorg.okta.com/api/v1/org/zones/nzovw2rFz2YoqmvwZ0g3"
~~~

#### Update response

~~~json
{
  "type": "IP",
  "id": "nzovw2rFz2YoqmvwZ0g3",
  "name": "UpdatedNetZone",
  "status": "ACTIVE",
  "created": "2017-01-24T19:53:28.000Z",
  "lastUpdated": "2017-01-24T19:53:28.000Z",
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "10.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "12.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "13.4.5.6-13.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "14.5.6.7-14.5.6.9"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "12.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "13.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "14.4.5.6-14.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "15.5.6.7-15.5.6.9"
    }
  ],
  "_links": {
    "self": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovw2rFz2YoqmvwZ0g3",
      "hints": {
        "allow": [
          "GET",
          "PUT",
          "DELETE"
        ]
      }
    },
    "deactivate": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovw2rFz2YoqmvwZ0g3/lifecycle/deactivate",
      "hints": {
        "allow": [
          "POST"
        ]
      }
    }
  }
}
~~~

#### Get zones

~~~sh
curl "https://yourorg.okta.com/api/v1/org/zones/"
~~~

#### Get Response

~~~json
[
  {
    "id": "nzou98V1LpPAXPBos0g3",
    "name": "LegacyIpZone",
    "type": "IP",
    "status": "ACTIVE",
    "created": "2017-01-24T19:52:32.000Z",
    "lastUpdated": "2017-01-24T19:52:32.000Z",
    "system": true,
    "gateways": null,
    "proxies": null,
    "_links": {
      "self": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzou98V1LpPAXPBos0g3",
        "hints": {
          "allow": [
            "GET",
            "PUT",
            "DELETE"
          ]
        }
      },
      "deactivate": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzou98V1LpPAXPBos0g3/lifecycle/deactivate",
        "hints": {
          "allow": [
            "POST"
          ]
        }
      }
    }
  }
]
~~~

#### Get Zones with filter expression

~~~sh
curl "https://yourorg.okta.com/api/v1/org/zones/?limit=100&filter=%28id+eq+%22nzoul0wf9jyb8xwZm0g3%22+or+id+eq+%22nzoul1MxmGN18NDQT0g3%22%29"
~~~

~~~json
[
  {
    "id": "nzoul0wf9jyb8xwZm0g3",
    "name": "0",
    "type": "IP",
    "status": "ACTIVE",
    "created": "2017-01-24T19:52:48.000Z",
    "lastUpdated": "2017-01-24T19:52:48.000Z",
    "system": false,
    "gateways": [
      {
        "type": "CIDR",
        "value": "1.2.3.4/24"
      },
      {
        "type": "CIDR",
        "value": "2.3.4.5/24"
      },
      {
        "type": "RANGE",
        "value": "3.4.5.6-3.4.5.8"
      },
      {
        "type": "RANGE",
        "value": "4.5.6.7-4.5.6.9"
      }
    ],
    "proxies": [
      {
        "type": "CIDR",
        "value": "2.2.3.4/24"
      },
      {
        "type": "CIDR",
        "value": "3.3.4.5/24"
      },
      {
        "type": "RANGE",
        "value": "4.4.5.6-4.4.5.8"
      },
      {
        "type": "RANGE",
        "value": "5.5.6.7-5.5.6.9"
      }
    ],
    "_links": {
      "self": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzoul0wf9jyb8xwZm0g3",
        "hints": {
          "allow": [
            "GET",
            "PUT",
            "DELETE"
          ]
        }
      },
      "deactivate": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzoul0wf9jyb8xwZm0g3/lifecycle/deactivate",
        "hints": {
          "allow": [
            "POST"
          ]
        }
      }
    }
  },
  {
    "id": "nzoul1MxmGN18NDQT0g3",
    "name": "1",
    "type": "IP",
    "status": "ACTIVE",
    "created": "2017-01-24T19:52:48.000Z",
    "lastUpdated": "2017-01-24T19:52:48.000Z",
    "system": false,
    "gateways": [
      {
        "type": "CIDR",
        "value": "1.2.3.4/24"
      },
      {
        "type": "CIDR",
        "value": "2.3.4.5/24"
      },
      {
        "type": "RANGE",
        "value": "3.4.5.6-3.4.5.8"
      },
      {
        "type": "RANGE",
        "value": "4.5.6.7-4.5.6.9"
      }
    ],
    "proxies": [
      {
        "type": "CIDR",
        "value": "2.2.3.4/24"
      },
      {
        "type": "CIDR",
        "value": "3.3.4.5/24"
      },
      {
        "type": "RANGE",
        "value": "4.4.5.6-4.4.5.8"
      },
      {
        "type": "RANGE",
        "value": "5.5.6.7-5.5.6.9"
      }
    ],
    "_links": {
      "self": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzoul1MxmGN18NDQT0g3",
        "hints": {
          "allow": [
            "GET",
            "PUT",
            "DELETE"
          ]
        }
      },
      "deactivate": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzoul1MxmGN18NDQT0g3/lifecycle/deactivate",
        "hints": {
          "allow": [
            "POST"
          ]
        }
      }
    }
  }
 ]
~~~

#### Get zone which has a given string in it's name

~~~sh
curl "https://yourorg.okta.com/api/v1/org/zones/?limit=-1&q=First"
~~~

~~~json
[
  {
    "id": "nzovhg5R4Gfat7hHE0g3",
    "name": "First0",
    "type": "IP",
    "status": "ACTIVE",
    "created": "2017-01-24T19:53:15.000Z",
    "lastUpdated": "2017-01-24T19:53:15.000Z",
    "system": false,
    "gateways": [
      {
        "type": "CIDR",
        "value": "1.2.3.4/24"
      },
      {
        "type": "CIDR",
        "value": "2.3.4.5/24"
      },
      {
        "type": "RANGE",
        "value": "3.4.5.6-3.4.5.8"
      },
      {
        "type": "RANGE",
        "value": "4.5.6.7-4.5.6.9"
      }
    ],
    "proxies": [
      {
        "type": "CIDR",
        "value": "2.2.3.4/24"
      },
      {
        "type": "CIDR",
        "value": "3.3.4.5/24"
      },
      {
        "type": "RANGE",
        "value": "4.4.5.6-4.4.5.8"
      },
      {
        "type": "RANGE",
        "value": "5.5.6.7-5.5.6.9"
      }
    ],
    "_links": {
      "self": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzovhg5R4Gfat7hHE0g3",
        "hints": {
          "allow": [
            "GET",
            "PUT",
            "DELETE"
          ]
        }
      },
      "deactivate": {
        "href": "https://yourorg.okta.com/api/v1/org/zones/nzovhg5R4Gfat7hHE0g3/lifecycle/deactivate",
        "hints": {
          "allow": [
            "POST"
          ]
        }
      }
    }
  }
]
~~~

#### Deactivate a zone

~~~sh
curl -X POST "https://yourorg.okta.com/api/v1/org/zones/nzovf314P6HMG5Wuw0g3/lifecycle/deactivate"
~~~

~~~json
{
  "type": "IP",
  "id": "nzovf314P6HMG5Wuw0g3",
  "name": "newNetworkZone",
  "status": "INACTIVE",
  "created": "2017-01-24T19:53:13.000Z",
  "lastUpdated": "2017-01-24T19:53:13.000Z",
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "1.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "2.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "3.4.5.6-3.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "4.5.6.7-4.5.6.9"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "2.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "3.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "4.4.5.6-4.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "5.5.6.7-5.5.6.9"
    }
  ],
  "_links": {
    "activate": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovf314P6HMG5Wuw0g3/lifecycle/activate",
      "hints": {
        "allow": [
          "POST"
        ]
      }
    },
    "self": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovf314P6HMG5Wuw0g3",
      "hints": {
        "allow": [
          "GET",
          "PUT",
          "DELETE"
        ]
      }
    }
  }
}
~~~

#### Activate a zone

~~~sh
curl -X POST "https://yourorg.okta.com/api/v1/org/zones/nzovf314P6HMG5Wuw0g3/lifecycle/activate"
~~~

~~~json
{
  "type": "IP",
  "id": "nzovf314P6HMG5Wuw0g3",
  "name": "newNetworkZone",
  "status": "ACTIVE",
  "created": "2017-01-24T19:53:13.000Z",
  "lastUpdated": "2017-01-24T19:53:13.000Z",
  "system": false,
  "gateways": [
    {
      "type": "CIDR",
      "value": "1.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "2.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "3.4.5.6-3.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "4.5.6.7-4.5.6.9"
    }
  ],
  "proxies": [
    {
      "type": "CIDR",
      "value": "2.2.3.4/24"
    },
    {
      "type": "CIDR",
      "value": "3.3.4.5/24"
    },
    {
      "type": "RANGE",
      "value": "4.4.5.6-4.4.5.8"
    },
    {
      "type": "RANGE",
      "value": "5.5.6.7-5.5.6.9"
    }
  ],
  "_links": {
    "self": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovf314P6HMG5Wuw0g3",
      "hints": {
        "allow": [
          "GET",
          "PUT",
          "DELETE"
        ]
      }
    },
    "deactivate": {
      "href": "https://yourorg.okta.com/api/v1/org/zones/nzovf314P6HMG5Wuw0g3/lifecycle/deactivate",
      "hints": {
        "allow": [
          "POST"
        ]
      }
    }
  }
}
~~~