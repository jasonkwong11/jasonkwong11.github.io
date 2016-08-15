---
layout: post
title:  "The Magical RESTful Conventions of Nested Routes"
date:   2016-08-15 00:11:09 +0000
---


Rails was created with the spirit of prizing convention over configuration. Accordingly, one can write their routes in a RESTful manner fairly intuitively.

Take this path for example: 

`/houses/3/rooms/2`  

RESTful convention allows us to understand that the path above will take us a a page related to room 2 of house 3.

In our routes.rb file, we can represent it like below:

`	resources: houses do
resources :rooms
end`

```
resources: houses do
resources :rooms
end
```


The resources method auto-magically gives us access to the 7 RESTful routes for not only the top level houses routes, but also the accompanying rooms routes associated with each house route. 

Top level: houses_url, edit_house_url, house_url,  etc...

Bottom level: house_rooms_url, new_house_room_url, edit_house_room_url

The cool things about these is that looking at each url path, they make sense on an intuitive level through use of the singular and plural forms of house.

Note! We should never be nesting resources more than one level deep! Otherwise we lose clarity. A great blog post by Jamis Buck on this can be found here: http://weblog.jamisbuck.org/2007/2/5/nesting-resources

For example: house_rooms_url has a singular 'house' and a plural 'rooms'. One can infer that this path will take us to the index page containing all rooms in a given house. (The plural forms are conventionally associated to the index page of that resource). Thus new_house_room_url, and edit_house_room_url will take us to the create and update actions for a single room in a given house respectively.

It's important to note that when adding these helpers to your views, each nesting layer must be accounted for when designating the arguments for the nested routes. 

Example: 

One argument, (house) for a single resource: 

`<%= link_to "Edit house", edit_house_path(house) %>  `

Two arguments, (house, room) for a nested resource:

```
<%= link_to "Edit room", edit_house_room_path(house, room) %>  
```

Happy RESTful routing!


