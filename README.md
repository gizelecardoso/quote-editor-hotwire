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

## Chapter 4 - Turbo Frame and Turbo Stream templates


- Turbo Frames
    - are independent pieces of a web page that can be appended, prepended, replaced, or removed without a complete page refresh and writing a single line of JavaScript!

    ```ruby
    turbo_frame_tag helper
    ```
    - the turbo_frame_tag helper creates a <turbo-frame> custom element that contains the HTML generated by the content of the block.

    - Rules of Turbo Frame

        - **Rule 1:** When clicking on a link within a Turbo Frame, Turbo expects a frame of the same id on the target page. It will then replace the Frame's content on the source page with the Frame's content on the target page.

        - **Rule 2:** When clicking on a link within a Turbo Frame, if there is no Turbo Frame with the same id on the target page, the frame disappears, and the error Response has no matching <turbo-frame id="name_of_the_frame"> element is logged in the console.

        - **Rules 3:** A link can target another frame than the one it is directly nested in thanks to the data-turbo-frame data attribute.


            **Note:** There is a special frame called _top that represents the whole page. It's not really a Turbo Frame, but it behaves almost like one, so we will make this approximation for our mental model.

    - dom_id helper

    ```ruby
    # If the quote is persisted and its id is 1:
    dom_id(@quote) # => "quote_1"

    # If the quote is a new record:
    dom_id(Quote.new) # => "new_quote"

    # Note that the dom_id can also take an optional prefix argument
    # We will use this later in the tutorial
    dom_id(Quote.new, "prefix") # "prefix_new_quote"
    ```

- Turbo Stream
    - the combination of Turbo Frames and the new TURBO_STREAM format, we will be able to perform precise operations on pieces of our web pages without having to write a single line of JavaScript, therefore preserving the state of our web pages
    - the turbo_stream helper responds to the following methods, so that it can perform the following actions

    ```ruby
    # Remove a Turbo Frame
    turbo_stream.remove

    # Insert a Turbo Frame at the beginning/end of a list
    turbo_stream.append
    turbo_stream.prepend

    # Insert a Turbo Frame before/after another Turbo Frame
    turbo_stream.before
    turbo_stream.after

    # Replace or update the content of a Turbo Frame
    turbo_stream.update
    turbo_stream.replace
    ```


___

## Chapter 5 - Real-time updates with Turbo Streams

- The Turbo Stream format allows, in combination with Action Cable, to make real-time updates to our web pages with just a few lines of code. Real-world applications are, for example, group chats, notifications, or email services.


    ```ruby
    after_create_commit -> {
        broadcast_prepend_later_to "quotes"
        partial: "quotes/quote",
        locals: { quote: self },
        target: "quotes"
    }
    ```

    - **after_create_commit** callback to instruct our Ruby on Rails application that the expression in the lambda should be executed every time a new quote is inserted into the database.

    - **lambda expression** It instructs our Ruby on Rails application that the HTML of the created quote should be broadcasted to users subscribed to the "quotes" stream and prepended to the DOM node with the id of "quotes"
    *broadcast_prepend_to* method will render the quotes/_quote.html.erb partial in the Turbo Stream format with the action prepend and the target "quotes" as specified with the target: "quotes" option


    We need to specify it in the Quotes#index view:

    ```ruby
    #<%# app/views/quotes/index.html.erb %>

    <%= turbo_stream_from "quotes" %>
    ```

    The HTML generated by the turbo_stream_from helper looks like this:

    ```html
    <turbo-cable-stream-source
    channel="Turbo::StreamsChannel"
    signed-stream-name="very-long-string"
    >
    </turbo-cable-stream-source>
    ```

    - **Turbo::StreamsChannel** inside the channel attribute is the name of the Action Cable channel. Turbo Rails always uses this channel, so this attribute is always the same.

    - **signed-stream-name** attribute is the signed version of the "quotes" string we passed as an argument. It is signed to prevent malicious users from tampering with it and receiving HTML from streams they should not have access to


    - In development, your config/cable.yml

        ```ruby
        # config/cable.yml

        development:
        adapter: redis
        url: redis://localhost:6379/1
        ```


- Turbo Streams conventions and syntactic sugar
        
    ```ruby
    #after_create_commit -> {
        #broadcast_prepend_later_to "quotes"
        # The partial default = to_partial_path on an instance of the model
        # by default in Rails for our Quote model is equal to "quotes/quote"
        # partial: "quotes/quote",

        # The locals default value is equal to { model_name.element.to_sym => self } 
        # in the context of our Quote model, is equal to { quote: self }
        # locals: { quote: self }

        # By default, the target option will be equal to model_name.plural
        # Because of that we can drop this line
        # target: "quotes"
    #}

    after_create_commit -> { broadcast_replace_later_to "quotes" }
    ```

- Making broadcasting asynchronous with ActiveJob

    ```ruby - before
    after_create_commit -> { broadcast_prepend_to "quotes" }
    after_update_commit -> { broadcast_replace_to "quotes" }
    after_destroy_commit -> { broadcast_remove_to "quotes" }
    ```

     ```ruby - after
    after_create_commit -> { broadcast_prepend_later_to "quotes" }
    after_update_commit -> { broadcast_replace_later_to "quotes" }
    after_destroy_commit -> { broadcast_remove_to "quotes" }
    ```

    **Note**: The broadcast_remove_later_to method does not exist because as the quote gets deleted from the database, it would be impossible for a background job to retrieve this quote in the database later to perform the job.


- More syntactic sugar

    ```ruby - before
    after_create_commit -> { broadcast_prepend_to "quotes" }
    after_update_commit -> { broadcast_replace_to "quotes" }
    after_destroy_commit -> { broadcast_remove_to "quotes" }
    ```

    ```ruby - after
    # Those three callbacks are equivalent to the following single line
    broadcasts_to ->(quote) { "quotes" }, inserts_by: :prepend
    ```

___
