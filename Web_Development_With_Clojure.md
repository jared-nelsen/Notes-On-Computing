# Web Development with Clojure

Clojure enables cleaner and simpler web development by using libraries instead of a framework per se.

Can generate a Luminus project via a a Leiningen template. There are flags that can be used to determine what to include.

To open a REPL: `lein repl` or `cider-jack-in`.

The  `user` namespace is for development purposes only. It is not included in the release. It contains functions that help us in development like `start`, `stop`, `restart` and so on.

So to start our app we can just run `(start)` in the repl. 

Then the site is available at `localhost:3000`.

Project Structure:

```
- env: Seems to be environment stuff
- resources: docs, html, migrations, css, img, sql
- src: code
- test: test stuff
```

### Managing Database Migrations

New pairs of migrations can be generated with the `(create-migration "MIGRATION")` function (after the application has been started). This will create a `up (roll forward)` and a `down (roll backward)` migration script.

#### Create the database from the migrations

 Run `(migrate)` . Since h2 is in memory we will need to `(restart)` the application. This will create the sql queries.

### Specifics of SQL for Luminus

``````-- :name save-message! :! :n
-- :name save-message! :! :n
-- :doc creates a new message using the name and message keys
INSERT INTO guestbook
(name, message)
VALUES (:name, :message)
-- :name get-messages :? :*
-- :doc selects all available messages
SELECT * from guestbook
``````

The comments actually do things so `:!` `:n` are relavant. Figure this out on the fly!

### Configuration

There is a file called `dev-config.edn` in the root of the project. This is where  `dev` config like db stuff and ports are configured. There is also one for `test`.

### DB Misc Info

The state of the db is sotred in the `*db*` variable and is managed by the `Mount` library. The queries we write in SQL are bound to functions using the bind-connection macro when the namespace is loaded. 

### Working with the DB

- Reload the query functions by running (conman/bind-connection *db* "sql/queries.sql") or by restarting the app.

The queries you wrote in the sql are now bound to available functions that you dont have to write! An example is that the `get-messages` query is now bound to `(get-messages)` and same with `(save-message! {})`

## Special Note

There are ***few*** cases where you will actually need to restart the app! Use an editor with good support for REPLs. ***As you write code keep evaluating it and testing it!!!***

## Tests

Write tests in the `test` directory like you already know how.

```clojure
(deftest test-messages
(jdbc/with-transaction [t-conn *db* {:rollback-only true}]
(is (= 1 (db/save-message!
t-conn
{:name "Bob"
:message "Hello, World"}
{:connection t-conn})))
(is (= {:name "Bob"
:message "Hello, World"}
(-> (db/get-messages t-conn {})
(first)
(select-keys [:name :message]))))))
```

Don't forget about the `test-config.edn` file if you need to configure anything.

### Automatically run tests

Leiningen can be configured to automatically rerun tests! This can be done by opening a ***separate terminal*** that is `cd`d to the project directory and then run `lein test-refresh`. Now we can keep an eye on it to make sure all of our tests pass while we make changes to the code.

Developing in the REPL and then writing tests is a common workflow in Clojure. It is much faster than TDD.

## Defining HTTP Routes

Routes are found in the `app.routes.homne` namespace.

Easy.

## Aside

There is a lot of stuff here specific to the example to do with HTML and templates but this is outside of my use case so I will learn about it later in the book.

# Chapter 2: Luminus Web Stack

TODO