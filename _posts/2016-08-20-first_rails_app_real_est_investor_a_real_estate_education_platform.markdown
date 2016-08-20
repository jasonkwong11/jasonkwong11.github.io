---
layout: post
title:  "First Rails App: real.est.investor: a Real Estate Education Platform"
date:   2016-08-19 21:19:54 -0400
---


Greetings! This post describes the joys of creating my first Rails application, real.est.investor, a real estate investing education platform.This site is meant for real estate investors to meet and share tips and best practices in their experiences with real estate investment. Real estate investment is a hobby of mine and I enjoy learning about it. There's so much to learn and investment strategies often vary depending on the region due to differences in local laws, procedures, etc. I thought a platform to create a common knowledge base on the subject would be a great way to educate newbies and pro's alike.

How it works: 

Users create accounts and are provided full CRUD capabilities (Create, Read, Update, Destroy) in creating posts or "tips". Authentication is through Devise and OmniAuth - Facebook. Users can only edit and delete their own posts. Users can also comment on each other's posts, utilizing nested forms. Bad data is prevented by presenting the user validation errors in the views if character minimums for posts and comments are not met.

There's a responsive navigation bar at the top that changes user links depending on if the user is logged in or logged out. On the index page, newest tips are listed first. There is also a class level ActiveRecord scope method used to render the "Top Contributor" feature which highlights the user who's posted the most tips and the number of posts they've contributed. 
Nested forms for each post allow the user to associate relevant details about the property they mention in the post or are referring to in their post. This is accomplished through the nested form writing to an associated model using a custom attribute writer.

The process:

This CRUD app was a great learning experience for creating my first rails app. The biggest lesson I learned is that Devise and OmniAuth can be incredibly frustrating. Because Devise's views and sessions controller are hidden until you generate them (but they're hooked up in the routes.rb by default), routing can easily send you in a spiral of self-doubt, questioning whether you know how to route at all. Luckily, you're not crazy, Devise is! OmniAuth? The cure is usually running rake routes often to make sure views are wired to the correct controller actions. The cure for OmniAuth? There is no cure.

Other than the above frustrations, everything else moved surprisingly smoothly! Heeding Avi's sage wisdom, I stubbed out the features for this website from the outside edges, inward:  starting by creating the database migrations and model associations then moving to the views and routes. This plan of lets you gain momentum in getting the low hanging fruit first. Then you have a very solid foundation when trying to implement nested routes, and subsequently Devise and Omniauth. This drastically sped up the entire development process.

Also refactoring is fun! Fat models and skinny controllers! After refactoring, the controllers were much more readable and the repeated code was nicely tucked away in appropriate helper files. A TON of logic and repeated code was removed from the views and stuck into partials. Looking through the views, it was quite easy to understand what was being rendered on each page.

In the future..

Although I consider this a decent start, there are a couple of features I'd like to add to this project. 

1. Add software for creating data models and analysis of investment properties, that provides recommendations for investor on where/what to invest in their specific region.

2. Visually make it more palatable. Right now it's very minimalist.

