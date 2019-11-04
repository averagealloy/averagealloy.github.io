---
layout: post
title:      "What is CRUD? "
date:       2019-11-04 02:45:23 +0000
permalink:  what_is_crud
---


If by chance on your first try you understand CRUD take the day off because your brilliant.
but I did not and because I did not. im going to try to help you ( whoever you are) understand this crud.


# What does CRUD stand for?

CRUD is an acronym standing for....

```
C = CREATE 
R = READ
U = UPDATE 
D = DELETE
```
there are seven main CRUD functions

```
GET  /flashcards        index
GET /flashcards/:id     show
GET /flashcards/new     new
GET /flashcards/:id     edit
PATCH /flashcards/:id   update
POST /flashcards        create
DELETE /flashcards/:id  destroy

```

I will go through them one by one and talk about them some more.


# index
Index is pretty simple it looks somthing like this 

```
  get '/flashcards' do
      if logged_in?
        @flashcards = current_user.flashcards.all
        erb :"flashcards/index"
      else
        redirect "/login"
      end
    end
```

All this route is doing is checking if there is a user logged in. if so send them to view that you have created. if not send them back to the login 

# show
Show is just geting the id of the user and in the URL it will say flashcards/:"the number of the post" the route will make more sense 

```
  get '/flashcards/:id' do
        @flashcards = Flashcard.find_by_id(params["id"])
        erb :"flashcards/show"
      end

```
This is setting a varible and in that varible is say in params get the id then render the flashcard/show page 

# new 
This route is saying for take all users and render a page. it looks like this 

```
  get '/flashcards/new'do
      @users = User.all
      erb :"flashcards/new"
    end
		
```
Again it sets a varible @users to user.all then rendering a view 

# edit 
You ca....... well it's edit so let me just show you the route 

```
  get '/flashcards/:id/edit' do
       @users = User.all
      @flashcards = Flashcard.find_by_id(params[:id])
      if @flashcards.user.id == current_user.id
             erb :"flashcards/edit"
         else
             redirect "/flashcards"
         end
    end
```
This route is asigning a varible like #new then setting a new varible to get the id in params.
after that im asking with an if statment if the user id is matching the current user id if so render a web page 
if not render another web page

# update
Im not sure why I remember this but in the lessons they used a car analogie. don't thorw out a car if you just wanna change one thing. PUTS vs PATCH let me show you what im talking about 


```
  patch '/flashcards/:id' do
      @flashcards = Flashcard.find_by_id(params[:id])
      params.delete("_method")
      if @flashcards.update(params)
        redirect "/flashcards/#{@flashcards.id}"
      else
        redirect "/flashcards/#{@flashcards.id}/edit"
      end
    end

```
Here im setting a varible (are you seeing a patern yet)  that varible has the id were looking for in it. Then I'm removing  "_method_" from the params hash. Then im asking if my params have changed then redirect to a page else redirect to page 

Im not saying delete the whole flashcard then recreate a similar one im saying go in there like a ninja and change it 

# create
Here is the method that actually puts it where it need to go. This is the go not the show

```
  post '/flashcards' do
        flashcard = Flashcard.new(user_id: current_user.id, title: params[:title], description: params[:description])
        if flashcard.save
          redirect "/flashcards/#{flashcard.id}"
        else
          redirect "flashcard/new"
        end
      end
```
Hey a new user has user id ,name and description then save it. Redirect to the flash card you just created if not go back your flashcard/new page 

# destroy
The most metal of the routes. This route is like the index route simple lets check it out.

```
delete '/flashcards/:id' do
        @flashcards = Flashcard.find_by_id(params[:id])
        if @flashcards.user.id == current_user.id
          @flashcards.delete
          redirect "/flashcards"
        else
          redirect "/flashcards"
        end
      end
```
This route gets the user id in params. Then is saying is the user id the same as the current user id if so DROP THAT DELETE HAMMER BROOOOOOOOOOOO. if not send the user back to /flashcards.

# conclusion
I hope this helped and if didnt AAQ it.  Thoes guys are great! 
