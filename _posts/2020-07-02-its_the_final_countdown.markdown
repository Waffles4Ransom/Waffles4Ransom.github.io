---
layout: post
title:      "It's the final countdown..."
date:       2020-07-02 06:56:32 +0000
permalink:  its_the_final_countdown
---


![](https://i.imgur.com/EiubdSz.gif)


If I said I haven't been waiting since the beginning of this journey to use this glorious Gob gif, I'd be lying....Final Project is here!!

The React/Redux portfolio project should: 
* be built with React 
* use Redux to manage state, with thunk middleware to allow for async actions 
* communicate via fetch with a Rails API backend

This time around I've decided to build a web application dedicated to my love for snacks. And not just snacks from home, but all over the world. One of my all time favorite parts of traveling is visting the local grocery store to marvel and load up on all the different snacks and treats. 

![](https://i.imgur.com/y2xCl2H.gif)

> but in this world nothing can be said to be certain, except death and taxes...and at any given moment, jenna needs a snack.

Pretty sure that's how that quote goes...

## Snack That

A place for serious snack lovers to log and review any local or international snacks they've tried, and hopefully find new delicious snacks waiting to be enjoyed. 

### Basic Features
* users can view snacks and snackers 
* users can sign up to become a snacker, after which they are able to add snacks and reviews
* snacks have an image, country of origin, description, flavor & texture categories
* snacks are rated on a 0 to 5 scale - represented by cat butts 


### Getting Started
1. use `create-react-app` to spin up the frontend 
2. create a Rails API for the backend 
3. plan out and structure the models & their relationships
4. test those associations in console, create some seed data, and build on some basic controller actions
5. move over to the frontend, wiring up a redux store to manage most of the application's state
6. set up thunk to be able to make asynchoronous requests to the backend from inside an action creator
7. use react-router to create RESTful routing
8. get busy building out all the components of the application 
9. use as much ES6 code as possible 
10. style it up with CSS  

Now did this portfolio project require sessions? No. Did I choose to create a web application that wouldn't make much sense without them? YUP. And boy it was a bit of a challenge, but it felt good when it finally worked. One funny little roadblock to getting there was as simple as forgetting to properly desturcture my login action creator...so let's talk about that. 

#### Destructuring

The official Mozilla defintion is as follows...

> The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

So basically, a simple way to extract values from an array or object. Destructuring assignment was brought to Javascript in ES6. This included the upgraded way of handling parameters via destructuring, which helps keep your code DRY and easy to read.  

Let's cover the basics of destructuring assignment...

Destructuring an array:

![](https://i.imgur.com/HuYoZysl.png)

```
const cats = ["Rufio", "Pfeiffer"]
[bigBoi, lilBoi] = cats
bigBoi => "Rufio"
lilBoi => "Pfeiffer"
```

Destructuring an object: 

![](https://i.imgur.com/upfarCSl.png)

```
const cat = {name: "Rufio", age: 11, weight: "18lbs"}
let {name, age, weight} = cat 
name => "Rufio"
```


So we can also destructure a functional component's incoming props, allowing you to simplify the code you write inside the function. 

Without destructuring:

```
const SnackCardFront = props => {

  return(
    <div className='snackcard' onClick={() => props.handleClick()}>
      <h4>{props.snack.name}</h4>
      <img src={props.snack.image} alt={props.snack.name} className='snackImg' />
      <Link to={`/snacks/${props.snack.id}`}><button>Tell me more</button></Link>
    </div>
  )
}

export default SnackCardFront
```

With destructuring: 

```
const SnackCardFront = ({snack, handleClick }) => {

  return(
    <div className='snackcard' onClick={() => handleClick()}>
      <h4>{snack.name}</h4>
      <img src={snack.image} alt={snack.name} className='snackImg' />
      <Link to={`/snacks/${snack.id}`}><button>Tell me more</button></Link>
    </div>
  )
}

export default SnackCardFront
```

Now, the difference is minimal in the above examples, but the code is definitely less cluttered. I don't have to repetitively call `props.objectName.propertyName` everywhere. This can be a really powerful and useful with larger objects or as the parameters needed scale up. 

I am looking forward to adding more features to *Snack That* and sharing it with the world (aka probably just my other snack-obsessed friend, Judy).

But I probably won't share my snacks, sorry. 

![](https://i.imgur.com/AIAwMxd.gif)



