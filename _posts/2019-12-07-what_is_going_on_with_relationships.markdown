---
layout: post
title:      "What is going on with relationships "
date:       2019-12-07 21:39:57 +0000
permalink:  what_is_going_on_with_relationships
---


DISCLAIMER: We are not doing talking about the relationships like has `has_many` or `belongs_to` mearly because I don't want to! I have struggled with `:whatever has_many :other_whatever through: :even_more_other_whatever` for what ever reason.


# Let's break this down !
Lets look at the first part of this statment `:whatever has_many :other_whatever`. A bit easy on the eyes 
like a package of twizzlers has many twizzlers in it. Nice, simple , and care-free. This checks out .


But here we go. A metaphorical screw in the tuna. THROUGH ????
through what , why ,  where. This really threw me off. In a vacum the relationship works for me but its when I add it in my application I just dont see it for what ever reason. So lets try and see it together. these relationship live in your modles. Each one of your models have some of the regular cast of characters (such as has_many and belongs_to).  



# Sure, but where are the Gummy bears 

let me explain. We have three models.   

1. gummybears 
2. snackers  
3. stores

Im going to write this out in english first and then move into the codizzle 

again im going to take this slow and steady 

gummybears have many snackers that would look like this :
```

       has_many  :snackers  

```

but like the title of this section suggest where the hell are the gummy bears 

well gummybears have stores and if your lucky and eat your veggies (but mostly have money) you can have gummy too. so that means that we will add this: 

```


       has_many  :snackers  
       belongs_to :store
			 belongs_to :user 
 
```

but this would be a perferct opportunity to use a has many through realationship this may be a bit unclear but just gummybear with me for a minute.




```
has_many :gummybears, through: :stores
has_many :users, through: :stores 
```
NOW THAT WHAT WERE TALKING ABOUT !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 


# Closing thoughts 

This was a breif explanation of how a has many through relationship works and if your still struggling with this stuff you can draw out your relationships  . I can not stress enough how important it is to draw them out you know it better and will give you more confidance when it comes to your technical interview, plus talking with your cohort mates, teachers, dogs and other inanimate objects.


