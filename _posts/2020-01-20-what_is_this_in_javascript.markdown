---
layout: post
title:      "What is 'this' in JavaScript"
date:       2020-01-20 15:43:13 +0000
permalink:  what_is_this_in_javascript
---


'This' is trickly for alot of people in Javascript including me. I remember reading the curriculum and thinking well it's just self from ruby. At the end of that specfic lession they said very importantly that 'this' and 'self' were not the same. They also said that learning 'this' now (when I was reading the lesson) before my project would save me many headaches. Being  naive I had said *eh*. Now in my project week I had wished I just took the time beacuse 'this' is pretty cool.  

# disclaimer
This is just a brief intro into 'this' I have plans to write a second blog to go into it further. 



# Quick example 

If I got to a new tab in my browser I can show you what I am talking about. if you create a new object called car and say 

```
this.car = 'ford escape'
```
hit enter and then type in 
```
console.log(window.car)
```
you will see that we get ford escape. but why. we get ford escape because we are in global scope.


now that you are aware that we can call this on objects in the global scope we are now going to move into this inside an object


# 'This' in a object  

If you go back into your browser console and create an object. mine is called carGarage it looks like this 
```
let carGarage = {
car: 'bentley continental'
};
```
and then loge it using the this key word like this 
```
console.log(this.carGarage.car)
```
You will get something weird. An error the this key word should yeild the car in the car garage. Here is the Error 

```
VM1201:1 Uncaught TypeError: Cannot read property 'car' of undefined
    at <anonymous>:1:28
```

Why is that? It is because 'this' does not have access to the scope out side of the object so you would call it like this

```
console.log(carGarage.car)
```
giving you back the result of that car 

```
bentley continental
```

# 'This' inside of a method 
we can use 'this' inside of a method because inside that method it is referencing something I have already created here is a new method called washCar inside of a varible 

```
let carHome = {
car: 'honda civic',
washCar(){
console.log(`washing ${this.car}`)
}
};
```
then if you call the name of the function with the prefix of the object you will get the desired result of that method like this 
```
carHome.washCar()
```
and the retun value is 

```
washing honda civic
```

'This' was a brief introduction to this in JavaScript. There are many more interesting and cool things about 'this' but I hope this served you to get the ball rolling. I am going to have a second part to this blog like I had said eariler talking about more cool stuff with 'this' so stay tuned!
