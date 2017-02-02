# Implementation notes

Below are the main gems I utilised for this implementation:

Gem | Purpose
---------- | -------
Rspec-rails| Testing
Factory_girl_rails | Replacement of fixtures
Faker | Random generation of attributes for factory girl
Database_cleaner | For refreshing of test database while running test suite
Kaminari | Pagination
Activerecord-import | Allows for mass import of model instances with a single query
