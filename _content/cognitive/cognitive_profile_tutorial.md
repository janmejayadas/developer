---
title: Tutorial: cognitive profiling
weight: 10
---

To explore the cognitive profiling capability in Watson Assistant Solutions, complete this tutorial.

**Note**: All REST API endpoints that are referred in this tutorial are prefixed with `/v1/profile`.

### Step 1: Access the Swagger documentation

1. Log in to the [Watson Assistant Solutions console](https://watson-personal-assistant.github.io/developer/) using your IBMid.
2. From the home page, click the link for the cognitive profile swagger documentation.

### Step 2: Create a profile for a user.

Pass the following JSON object to the `/user` endpoint to create a cognitive profile for John Smith. 

  ```
  {
    "first_name": "John",
    "last_name": "Smith",
    "user_id": "222222"
  }

```

HTTP Status code 200 is displayed.

### Step 3: Shape the profile

1. Submit a `like` event for John Smith. Pass the following JSON object to the `/events` endpoint to register that John Smith liked a restaurant that is named Spitteleck in Berlin, Germany.

  ```
  {
    "timestamp" : 100212,
    "user_id": 222222,
    "event_type": "like",
    "relates_to": {
      "domain": "restaurant",
      "class": "restaurant",
      "object": {
        "yelp_id":"spitteleck-berlin",
        "name":"Spitteleck",
        "is_closed":false,
        "category":["german"],
        "rating":3,
        "latitude":52.5111994,
        "longitude":13.4028,
        "price":"$$",
        "city":"Berlin",
        "zip_code":"10117",
        "country":"DE",
        "state":"BE",
        "distance":652.459473818992
      }
    }
  }

  ```
The following response is displayed.  The likeability of German restaurants is represented by a magnitude score of 0.2.  The profile service applies a confidence score of 0.83 to this value.  The likeability of medium priced restaurants is also 0.2 and a confidence score of 0.83 is applied.

  ```

  {
    "userId": 222222,
    "response": [
      {
        "confidence": 0.8333333333333334,
        "key": [
          {
            "name": "german",
            "class": "category"
          }
        ],
        "magnitude": 0.2
      },
      {
        "confidence": 0.8333333333333334,
        "key": [
          {
            "name": "$$",
            "class": "price"
          }
        ],
        "magnitude": 0.2
      }
    ]
  }

  ```
2. Submit a `hate` event for John Smith for the same restaurant.  John Smith visited the same restaurant one day later and received terrible service.  Pass the same event to the `/events` endpoint, but change the event type to `hate` and update the timestamp.
    
  ```

  {
    "timestamp" : 100213,
    "user_id": 222222,
    "event_type": "hate",
    "relates_to": {
      "domain": "restaurant",
      "class": "restaurant",
      "object": {
        "yelp_id":"spitteleck-berlin",
        "name":"Spitteleck",
        "is_closed":false,
        "category":["german"],
        "rating":3,
        "latitude":52.5111994,
        "longitude":13.4028,
        "price":"$$",
        "city":"Berlin",
        "zip_code":"10117",
        "country":"DE",
        "state":"BE",
        "distance":652.459473818992
      }
    }
  }

  ```

The following response is displayed.  In this case, John has a strong negative reaction to the restaurant.  His dislike for the restaurant is shown in the magnitude score of 0.36 for German restaurants and medum-priced restaurants.  His strong feel towards the restaurant is reflected in a confidence score of 1.

  ```
  {
    "userId": 222222,
    "response": [
      {
        "confidence": 1,
        "key": [
          {
            "name": "german",
            "class": "category"
          }
        ],
        "magnitude": 0.3666666666666667
      },
      {
        "confidence": 1,
        "key": [
          {
            "name": "$$",
            "class": "price"
          }
        ],
        "magnitude": 0.3666666666666667
      }
    ]
  }

  ```
### Step 4: Send a request to the profile service to rate and sort a list of restaurants.

Pass the following JSON object to the `/ratings` endpoint to rank and sort the restaurants according to John Smiths preferences.

```
{
  "class": "restaurant",
  "domain": "restaurant",
  "objects": [
    {
      "yelp_id": "fresh-salt-new-york",
      "name": "Fresh Salt",
      "is_closed": false,
      "category": [
        "bars",
        "german",
        "italian"
      ],
      "rating": 3,
      "latitude": 40.707077,
      "longitude": -74.002464,
      "price": "$$",
      "city": "New York",
      "zip_code": "10038",
      "country": "US",
      "state": "NY",
      "distance": 711.704497503942
    },
    {
      "yelp_id": "aaa-salt-haifa",
      "name": "Aaa Haifa",
      "is_closed": false,
      "category": [
        "bars",
        "newamerican",
        "italian"
      ],
      "rating": 3,
      "latitude": 40.707077,
      "longitude": -74.002464,
      "price": "$$",
      "city": "New York",
      "zip_code": "10038",
      "country": "US",
      "state": "NY",
      "distance": 100
    },
    {
      "yelp_id": "carmiel-best",
      "name": "Wau Carmiel",
      "is_closed": true,
      "category": [
        "german",
        "newamerican",
        "italian"
      ],
      "rating": 3,
      "latitude": 40.707077,
      "longitude": -74.002464,
      "price": "$$",
      "city": "New York",
      "zip_code": "10038",
      "country": "US",
      "state": "NY",
      "distance": 100
    }
  ],
  "user_id": "222222"
}

```
The following response is displayed:

```
{
  "userId": "222222",
  "signals": [
    {
      "rank": 1,
      "id": "carmiel-best",
      "rating": 1.0
    },
    {
      "rank": 2,
      "id": "fresh-salt-new-york",
      "rating": 0.6123979928024736
    },
    {
      "rank": 3,
      "id": "aaa-salt-haifa",
      "rating": 0.0
    }
  ]
}

```
A German restaurant, carmiel-best, is top of the list.

> **What to do next?**
- Read about [cognitive profiling]({{site.baseurl}}/cognitive/cognitive_profile_intro).