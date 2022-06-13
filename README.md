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

## Chapter 2 - CSS files

- Based on the <a href="https://en.bem.info/methodology/">BEM Methodology</a>

- Our structure:

    * **application.sass.scss** manifest file to import all our styles.
    * **mixins/** where we'll add Sass mixins.
    * **config/** where we'll add our variables and global styles.
    * **components/** folder where we'll add our components.
    * **layouts/** where we'll add our layouts.
___

## Chapter 3 - Turbo Drive

Objective: speeds up application, converting all links and form into <a href="https://developer.mozilla.org/pt-BR/docs/Web/Guide/AJAX/Getting_Started">AJAX requests</a>.

The <head> of the HTML page wont be changed, only the <body> that will be replace with the response of the AJAX requests, caled when the Turbo Drive **intercepts a click on a link or a submission of a form**.

Because of that form of Turbo works, the rails 7 applications are single-page by default, that means only the <body> of the page are replaced.

- Disabling Turbo Drive
    Some gems not supported Turbo Drive, so on that scenarios it is disabled.

    ```js
    data: { turbo: false }
    ```

    Option to disable Turbo Drive on whole project
    ```js
    // app/javascript/application.js
    import { Turbo } from "@hotwired/turbo-rails"
    Turbo.session.drive = false
    ```

- Reloading the page

A problem with this Turbo's solution is when the <head> is change by some update in the application, by default Turbo will change only the <body>. So it's important to put a element on the application page **data-turbo-track="reload"**

    ```js
    // app/views/layouts/application.html.erb

    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    <%= javascript_include_tag "application", "data-turbo-track": "reload", defer: true %>
    ```

- Changing the style of the Turbo Drive progress bar

the browser's default progress bar/loaders won't work as expected anymore, because the Turbo Drives overrides the default behavior of the page.

    - app/assets/stylesheets/components/_turbo_progress_bar.scss
___
