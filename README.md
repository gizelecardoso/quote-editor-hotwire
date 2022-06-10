# TURBO RAILS TUTORIAL

We are building a simple application based on this tutorial.

<a href="https://www.hotrails.dev/turbo-rails">Link Tutorial</a>

___
## Chapter 0 - Introduction

- Rails 7.0.0
- Ruby 3.2.1

- Docker
    - docker-compose up -d

    - Run postgres on port 5450
    - Run redis on port 6379

- Install the dependencies and create the database

    ```ruby
    bin/setup
    ```

- Run the rails server

    ```ruby
    bin/dev
    ```
    based on the file **Procfile.dev**
___

## Chapter 1 - CRUD

Using the TDD - development based on tests.

- Creating Tests of our application

    ```ruby
    bin/rails g system_test quotes
    ```

    - First CRUD Test - CREATE
        - Visit the page index
        - See the name Quotes
        - Click on New Quote
        - Fill name and click on Create quote
        - Return of the Quote page with the new name created.

    - use Rails fixtures to create fake data for our tests
        
        ```ruby
        # test/fixtures/quotes.yml
        ```

    - Second CRUD Test - READ

    - Third CRUD Test - UPDATE

    - Fourth CRUD Test - DELETE

- Creating Model Quote

    ```ruby
    rails generate model Quote name:string
    ```

    ```ruby
    bin/rails db:migrate
    ```

- Creating Controller Quotes

    ```ruby
    bin/rails generate controller Quotes
    ```

    - Create Action:
        - index
        - show
        - new
        - create
        - edit
        - update
        - destroy

- Creating Views 

    - Based on Actions:
        - index.html.erb
        - new.html.erb
        - edit.html.erb
        - show.html.erb

    - Partial:
        ### partial to delete or edit each quote of our list
        - _quote.html.erb

        ### partial for use form in many places of our application
        - _form.html.erb

            this partial especially, will use the benefit of the gem **simple_form**

- Run Test
    
    ```ruby
    bin/rails test:system
    ```

- Fixture and seed:

    Fixture was be used to create fake data for test.
    To development we use seed file.
    To reuse the fixture file on the seed file, we will add the following code on the seed file:

    ```ruby
    puts "\n== Seeding the database with fixtures =="
    system("bin/rails db:fixtures:load")
    ```

- Run Seed

    ```ruby
    bin/rails db:seed
    ```
___

