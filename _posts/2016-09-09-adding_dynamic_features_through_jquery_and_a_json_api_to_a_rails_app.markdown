---
layout: post
title:  "Adding Dynamic Features through jQuery and a JSON API to a Rails App "
date:   2016-09-09 23:00:07 +0000
---


This post details the next iteration of my rails app "realestinvestor" (see the details in a previous post found here: http://jasonkwong11.github.io/2016/08/20/first_rails_app_real_est_investor_a_real_estate_education_platform/). This time, I added a jQuery frontend to view pages in order to render data without a page refresh and ultimately improve the user experience. First, I thought it would be nice to have an index page of users, an "Investor Directory" which I routed to "/users". This page displays the current investors that use the app and their contact details. After getting the conventional rails controller and view set up, I worked to port it into a page that hides the information initially and can be rendered by clicking a link. Also on each post's show page, users can click the "Next Post" link to render the next chronological post to see the next real estate tip without refreshing the page. Both of these features use Active Model Serialization to serve the JSON objects to the view after an initial ajax request submitted by the event handler. 

Active Model Serializers are really helpful when dealing with large amount of JSON output. It lets you select only the specific keys you're need and can also handle nested resources!. Although my JSON objects didn't have the complexity that would warrant the use of AMS, it was great to implement for practice. I can see it being very helpful in the future when working on external commercial APIs that return complex JSON structures. 

Another jQuery feature I added was rendering the JSON to the page in the post has_many comments relationship. Previously users had to go to each post's comments index per usual REST convention. Now there are ajax requests that send GET and POST requests in the background so new comments can be created and rendered instantly without page refresh. When building this feature, I ran into a problem. My javascripts were firing two requests for every click/submit event. After a while I realized that the error was that my sprockets manifest file had the '//= require_tree .' directive as well as '//= require post' directive. This would essentially load the javascript from post.js twice (once from //= require_tree ., and again from 'require post'). The solution was simply to delete require post directive because post.js would be included in require_tree .. After spending a couple hours pulling my hair out, I can safely say I will never make this mistake again!

Another lesson learned was of the value of using debugger. Debugger is really helpful in figuring out what your javascript is returning at every step of the process. With the number of 'moving parts' involved in rendering an ajax request (click/submit event, to preventing the default controller action to rendering the json, parsing the JSON and injecting the formatted data back into the view), it's important to understand what is being returned at every step of the process. Playing with the data in Chrome's developer tools also helps you figure out how to return the correct data once you realize the undesired data is being returned. 


Future Features to Add:
	Right now I like the single page look of the main index page of posts. However if the the list gets too long and unwieldy I plan to use the Kaminari gem to paginate the posts. Also, it recently occurred to me that for a website that describes real estate properties, it would be great to have a feature that allow users to upload pictures. In this case, the paperclip gem would be great to add in the future. Finally, more specific user details and a bio page would be great to add to the sparse investor directory. 

