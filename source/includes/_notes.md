# Implementation Notes

## Gems used

Below are the main gems I utilised for this implementation:

Gem | Purpose
---------- | -------
Rspec-rails| Testing
Factory_girl_rails | Replacement of fixtures
Faker | Random generation of attributes for factory girl
Database_cleaner | For refreshing of test database while running test suite
Kaminari | Pagination
Activerecord-import | Allows for mass import of model instances with a single query

## Highlights / Challenges

### Data Modelling

I chose to design 3 data models to reflect the data given: SalesRep, Prospect and Email (pls refer to db/schema.rb for full details).

While the spec did not call for any specific endpoints that would interface with the SalesRep model, I chose to separate it out to better accommodate future extension of the API and to more accurately model the relationship between SalesRep and Prospect.

In short, SalesReps can have many Prospects, while each Prospect is associated with only one SalesRep. Emails are each associated with 1 SalesRep and 1 Prospect.

The data model facilitates easy filtering of Emails and allows for automatic status updates for prospects upon insertion of Emails into the database.

### Seeding

Status updates of prospects are needed both in the seed task and the POST#create endpoint for Email, so I defined the function in lib/HelperFunctions.rb to keep my code DRY. If I were to modularise out more functions that can be used in multiple spots, that's also where I would place them.

 The specific function that updates the status of Prospects is as follows:

`def updateStatus(prospect)
    flags = {"replied" => false, "followed_up" => false, "contacted" => false}

    prospect.emails.each do |email|
      flags["contacted"] = true
      flags["followed_up"] = true if flags["replied"] && email.sales_rep.email_address === email.sender
      flags["replied"] = true if email.prospect.email_address === email.sender
    end

    if flags["followed_up"]
      prospect.update({:status => "followed_up"})
    elsif flags["replied"]
      prospect.update({:status => "replied"})
    elsif flags["contacted"]
      prospect.update({:status => "contacted"})
    else
      prospect.update({:status => "cold"})
    end
  end
end`   

### API Formatting

At first, I wanted to keep my API JSON API-compliant and used jsonapi-resources as a quick way of getting my resource routes up. However, the curl requests started to look unwieldy compared to using the native json formatter.

`curl -X POST --data '{"data":{"type":"prospects","attributes":{"email_address": "kelvin.tyb@gmail.com"}}}' "http://localhost:3000/api/v1/prospects" -H "Content-Type: application/vnd.api+json"`

vs

`curl -X POST "http://localhost:3000/api/v1/prospects?email_address=kelvin.tyb@gmail.com`

### Deployment

<aside class="notice">GET https://sw-rails.herokuapp.com/api/v1/prospects works with a SUCCESS response but returns an empty array since the database is not seeded</aside>

While I was able to make the API publicly accessible and use `config.force_ssl = true` to enforce SSL protocol for the production app, I faced some difficulty seeding the production database from local .csv files. This led to 2 learning points:

1) I should have configured my development database to use Postgresql instead of Sqlite3 since Heroku dynos are a bad fit for Sqlite3 instances (https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem). This would allow for import of the database through db:load (using the `taps` gem). This is probably the easiest way to get the database seeded in the current implementation.

2) However, the above may not be as scalable in the scenario where users may want to upload their data using .csv files from remote locations. In this scenario, doing entire database imports will quickly become untenable as it would require users to update the same single database to be later imported into the host or (more realistically) update several databases (which all have to be synchronised).

The alternative is to provision users with a secure cloud solution (Amazon S3) from which the .csv data can be pushed to production server and parsed with a rake task into the appropriate format. Parts of this process could also be more plausibly automated compared with the previous solution. 
