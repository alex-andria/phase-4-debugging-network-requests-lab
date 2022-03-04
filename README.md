# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
    1. submitted toy with console open. 
    2. saw 500 error and opened Network tab to check toys post Preview and Header tabs.
    3. Saw exception: "#<NameError: uninitialized constant ToysController::Toys>" and found that it was indeed an Internal Server Error.
    4. Entered byebug in the create method of toys_controller.rb
    5. Found that the create method was accidentally pluralized for Toy.create.
    6. Posted once more to double check server is working.


- Update the number of likes for a toy

  - How I debugged:
    1. Clicked on like button. Checked console to see 
      "localhost/:1 Uncaught (in promise) SyntaxError: Unexpected end of JSON input
        at main.chunk.js:378:20
        (anonymous) @ ToyCard.js:21"
    2. Checked network to find that Status C ode 204 No Content
    3. SyntaxError signaled that it is a JSON parsing error and that I'll need to check in the controller actions for
    4. put in byebug in update and check params when liking:
      (byebug) params
      #<ActionController::Parameters {"likes"=>9, "controller"=>"toys", "action"=>"update", "id"=>"1"} permitted: false>
      (byebug) 
    5. Understood that toy was being updated but the JSON response wasn't being rendered.


- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged:
    1. Saw 404 error on console.
    2. Checked network preview and saw Error not Found with:
      "#<ActionController::RoutingError: No route matches [DELETE] \"/toys/1\">"
    3. checking routes.rb for destroy.
    4. added destroy to routes.
