---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>
  - <a href='https://kelvintyb.github.io'>API created by Kelvin Tan</a>

includes:
 - notes
search: true
---

# Introduction

Welcome to the SalesKitten API! You can use our API to access API endpoints, which can get information on various prospects/whales & emails in our database.

We currently only have language bindings in Shell - You can view code examples in the dark area to the right :)

# Prospects

## Get All Prospects

```shell
curl -X GET "https://sw-rails.herokuapp.com/api/v1/prospects?page=1&per_page=2"
```

> The above command returns JSON structured like this:

```json
  {
    "id":1,
    "sales_rep_id":1,
    "first_name":"Ashleigh",
    "last_name":"Schiller",
    "title":"Dynamic Paradigm Director",
    "email_address":"ashleigh@hilpert_mccullough_and_dubuque.com",
    "contact":null,
    "coy_name":"Hilpert, McCullough and DuBuque",
    "coy_desc":"De-engineered needs-based monitoring",
    "coy_addr":"7248 D'Amore Row","coy_city":"Leonorborough",
    "coy_country":"Malta",
    "coy_contact":"269-846-1976 x16677",
    "status":"cold"
  },
  {
    "id":2,
    "sales_rep_id":1,
    "first_name":"Caleigh",
    "last_name":"Stiedemann",
    "title":"Investor Solutions Planner",
    "email_address":"caleigh@christiansen_quigley_and_kovacek.com",
    "contact":null,
    "coy_name":"Christiansen, Quigley and Kovacek",
    "coy_desc":"Extended reciprocal productivity",
    "coy_addr":"1810 Heller Mission",
    "coy_city":"North Paxtonmouth",
    "coy_country":"Lao People's Democratic Republic",
    "coy_contact":"110-535-4500 x611",
    "status":"cold"
  }
```

This endpoint retrieves all prospects, and accepts optional parameters for pagination.

### HTTP Request

`GET https://sw-rails.herokuapp.com/api/v1/prospects`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | false | If set to a number, the per page limit of results will be 25
per_page | false | If set to a number, the per page limit of results will be set to that number


## Filter Prospects


```shell
curl -X GET "https://sw-rails.herokuapp.com/api/v1/prospects?filter_by_status=replied"

```

> The above command returns JSON structured like this:

```json
{
  "id":2,
  "sales_rep_id":1,
  "first_name":"Caleigh",
  "last_name":"Stiedemann",
  "title":"Investor Solutions Planner",
  "email_address":"caleigh@christiansen_quigley_and_kovacek.com",
  "contact":null,
  "coy_name":"Christiansen, Quigley and Kovacek",
  "coy_desc":"Extended reciprocal productivity",
  "coy_addr":"1810 Heller Mission",
  "coy_city":"North Paxtonmouth",
  "coy_country":"Lao People's Democratic Republic",
  "coy_contact":"110-535-4500 x611",
  "status":"replied",
}
```

This endpoint retrieves all prospects that have the specified status.


### HTTP Request

`https://sw-rails.herokuapp.com/api/v1/prospects?filter_by_status=<status>`

### URL Parameters

Parameter | Description | Options
--------- | ----------- | -----------
status | The status of the prospect to be retrieved | "cold", "contacted", "replied", "followed_up"

<aside class="warning">Note that there are only 4 filter options for prospects for now!</aside>


## Add a Prospect


```shell
curl -X POST "https://sw-rails.herokuapp.com/api/v1/prospects?sales_rep=1&email_address=kelvin.tyb@gmail.com&first_name=kelvin&last_name=tan"
```

> The above command posts JSON structured like this:

```json
{
  "id":3,
  "sales_rep_id":1,
  "first_name":"Kelvin",
  "last_name":"Tan",
  "title":null,
  "email_address":"kelvin.tyb@gmail.com",
  "contact":null,
  "coy_name":null,
  "coy_desc":null,
  "coy_addr":null,
  "coy_city":null,
  "coy_country":null,
  "coy_contact":null,
  "status":"cold"
}
```

This endpoint creates a new prospect with the given parameters.

<aside class="warning">The <code>sales_rep</code> and <code>email_address</code> parameters are required! </aside>

### HTTP Request

`POST https://sw-rails.herokuapp.com/api/v1/prospects?sales_rep=<SALES_REP_ID>&email_address=<EMAIL_ADDRESS>`

### URL Parameters

Parameter | Required?
---------- | -------
SALES_REP_ID | Required
EMAIL_ADDRESS | Required
TITLE | Optional
FIRST_NAME | Optional
LAST_NAME | Optional
CONTACT | Optional
COY_NAME | Optional
COY_DESC | Optional
COY_CITY | Optional
COY_CONTACT | Optional
COY_COUNTRY | Optional

# Emails

## Add an Email


```shell
curl -X POST "https://sw-rails.herokuapp.com/api/v1/emails?sender=neko@saleskitten.com&recipient=elon@spacex.com"
```

> The above command posts JSON structured like this:

```json
{
  "id": 1,
  "sales_rep_id": 1,
  "prospect_id": 101,
  "subject": null,
  "body": null,
  "sent_date": "02/02/2017",
  "sender": "neko@saleskitten.com",
  "recipient": "elon@spacex.com",
}
```

This endpoint creates a new email with the given parameters.

<aside class="warning">The <code>sender</code> and <code>recipient</code> email addresses are required!</aside>

### HTTP Request

`POST https://sw-rails.herokuapp.com/api/v1/emails?sender=<SENDER>&recipient=<RECIPIENT>`

### URL Parameters

Parameter | Required?
---------- | -------
SENDER | Required
RECIPIENT | Required
SUBJECT | Optional
BODY | Optional
SENT_DATE | Optional
