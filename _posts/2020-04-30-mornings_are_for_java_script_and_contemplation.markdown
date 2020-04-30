---
layout: post
title:      "'Mornings are for java(script) and contemplation...'"
date:       2020-04-30 02:21:54 -0400
permalink:  mornings_are_for_java_script_and_contemplation
---


![](https://media.giphy.com/media/XtQFRwoz77Suc/giphy.gif)

The JavaScript project should: 
* be a SPA (single page application)
* have a frontend built with HTML, CSS, and JavaScript
* have  a backend Rails API 
* use Object Oriented JS

## Stranger Messages 

A place where you can send messages from the Upside Down!

*Stranger Things* is one of my all time favorite shows. It was love at first watch. 

**The nostalgia. The sci-fi. The music. The waffle-loving protagonist. What's not to love??**

To demostrate some fun DOM manip for my JS project, I decided to create an application to play out messages 
à la Will communicating with Joyce from the Upside Down. 

![](https://i.imgur.com/Pjmm2Xdl.png)

### Basic Features

* user can play past messages sent by others 
* to send their own message, user claims a username
* once identified, a user can submit a message
* message can be played by the lights 
* message content can be revealed
* message can be deleted by it's user  

### Getting Started 
1. create a Rails API for the backend (database)
2. generate migrations, models, routes and controller actions
3. test out the model relationships in the console 
4. move on to the frontend and it's Three Musketeers (HTML, CSS, JS)
5. set up the basic HTML structure 
6.  use OOJS to keep the code organized 
7. wire up your JavaScript files and make fetch requests to send and retrieve data from the backend
8. manipulate the DOM with data received from the fetch requests
9. set up event listeners for the user to interact with the app w/o refreshing the page
10. draw out string lights with pure CSS ( a test of patience)

To avoid a giant index.js file that's a jumble of classes and functions, I opted to take the modular approach and subdivided my JS files into adapters and components, each it's own class. 

Adapters > messagesAdapter.js & usersAdapter.js

The adpater classes handle the fetch requests to the backend. For my application, those are to Create (POST), Read (GET), and Delete(DELETE). No updating allowed here, sorry. 
		 
Components > app.js, user.js & message.js 

The user and message classes instantiate new objects for each user and message and have functions to generate HTML for each instance. 

The application kicks off in the index.js, which mounts a new App class instance. The App class is where most of the action happens. It is serviced by the adapters and the user and message classes. I am able to keep things like all the messages or the current user as a property of App and can access them throughout the code without having to make additional network calls. Modular code is organized code. 

I think JavaScript is really dang cool.

![](https://i.imgur.com/gNQKV73b.gif)

I mean, the power, the possibilities...but JS has no love for throwing an error, which is super unhelpful when your code is not working and you don't know why. 

Sidestep, let me tell you about my best friend `console.log()`...She can tell you when your reaching a function or that your event listener is working. She can confirm that the value you're looking for is what you're receiving. She does it a lot. In summary, it is an incredibly useful tool. Maybe it's not the most efficient, but it is simple. When the whole point of my application (for a message to play out letter-by-letter) wasn't working I console-logged almost every step and was able to pinpoint the moment it was failing. 

Now JavaScript's minimalistic error throwing also causes a problem when it comes to validating user input. To keep my database clean and orderly, I have model validations. There will be no blank usernames here sir! A message's content cannot have numbers or special characters. Letters only or the highway bud! But to be able to tell the user what is wrong with their submission, I need to catch these validation errors and display them to the DOM. To do so you've got to force your fetch request to hit a catch by checking the status of the response. 

```
async checkStatus(res) {
    if (res.status > 299 || res.status < 200) {
      const eMsg = await res.json()
      throw new Error(eMsg.errors)
    }
  }
```

Now I can call this checkStatus function in my fetches and catch the errors thrown from the controller action. 

in the controller action: 
```
def create 
    message = Message.new(message_params)
    if message.save 
      ...
    else 
      render json: { errors: message.errors.full_messages}, status: 400
    end 
  end 
```

in the try/catch of my fetch function:
```
catch(error) {
      let errMsg = document.createElement('p')
      errMsg.innerText = error
      errMsg.setAttribute('class', 'error')
      this.mform.prepend(errMsg)
      setTimeout(() => errMsg.remove(), 3000)
    }
```

the user sees:

![](https://i.imgur.com/bHNbkW3l.png)

#### Lastly, let's chat about Fetch!

...I will not use a mean girls gif here, however tempting it might be...(overdone. so not fetch.)

Fetch is a built-in API that provides an interface for sending and receiving data (resources). 

A fetch call is asynchronous, meaning it operates in a spearate order from the rest of the code, and it returns a promise. 

For this project, I wanted to embrace the async/await syntax, despite only being taught `fetch().then().then()` in our labs. The async/ await makes your asynchronous code read like your synchronous code. Beautiful!

##### async/await
So `async` says make this function asynchronous and `await` says pause here until the promise is fulfilled. 

```
async getMessages() {
    const res = await fetch(this.baseURL)
    return await res.json()
  }
``` 

![](https://i.imgur.com/bn6xn08.gif)
...well, not really. 

If you don't use await in front of the response, the promise will go uncaught(unresolved) and your code it is going to break. 

If you try to use await in a function that is not async, your code is going to break. 

Async/await are a duo. They are like Eleven and waffles. Don't try to separate them. Mouthbreather. 

![](https://media.giphy.com/media/3o6ZtcoBuq9M8Kt7CU/giphy.gif)

See you in the freezer section!

![](https://i.imgur.com/7KPqSgim.jpg)


