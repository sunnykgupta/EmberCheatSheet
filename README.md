#Ember Cheat Sheet

##Summary

Ember is a javascript framework designed to build very ambitious web apps.
In my opinion it's best used as the predominant frontend technology as it
needs the entire page to operate. Although this is the case it can operate
on routes besides the root route. The router is very advanced and easy to use.
It works off of nested routes and ties in the model. It reminds me of Rails in
the way it handles template and route nesting and it's other various
opinionated ways. Think matryoshka dolls! I want Ember to be my predominant
MVC choice for web app development.

##Overview

The router is where it all begins. When the user enters a route the router
initiates the route object for that route. The model can then be loaded at
this step. Then the controller for that route is instantiated and given the
model. The controller will provide the model to the template but in a sense
the controller acts as the model because the models attributes can be
called on the controller (It will be an object controller or array controller).
When actions happen in a template, they go to the controller to be handled.
In the same way it is possible to access models up the route tree, these
actions and events can bubble up the route tree.

##Development Overview

Think about what models you want to show and how many at the same time. Then
go to the router and build out your routes and resources nesting where you
want to show relationships. From there go to prototyping your templates and put
your outlets where they need to be. Make your controllers and then the routes
for each template to bring in the model. From there link together the pages
using the link to helper. Now more complicated behavior can be added. Remember
to test at each stage of development. There is a strict naming convention that
should be followed unless you want to diverge and explicitly tell ember
what template goes with what controller.

##Conventions

###Models

###Routing

###Routes

###Controller

###Templates

###View

##Tips

