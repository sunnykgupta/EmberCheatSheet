#Ember Cheat Sheet

(Guide)[http://emberjs.com/guides/getting-started/]
(Another Cheat Sheet)[http://embersherpa.com/]

##Summary

Ember is a javascript framework designed to build very ambitious web apps.
In my opinion it's best used as the predominant frontend technology as it
needs the entire page to operate. Although this is the case it can operate
on routes besides the root route. The router is very advanced and easy to use.
It works off of nested routes and ties in the model. It reminds me of Rails in
the way it handles template and route nesting and it's other various
opinionated ways. Think matryoshka dolls! I want Ember to be my predominant
MVC choice for web app development. It works well with the Yeoman generator.
Run grunt to build and compilte, grunt serve to look at on localhost.

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

### (Adapters and Serializers)[http://emberjs.com/guides/models/the-rest-adapter/]

There are adapters that are used to connect to backends are whatever.
There is a localstorage adapter, one for certain backends like rest apis.
If the data comes back in a way you dont expect the serializer needs to 
normalize it. Set the base url for the api in the adapter. The primary
record being returned needs to be in the root. For using Rails and AMS,
use the ActiveModelAdapter, it expects ids to be embedded and records
to be included/side loaded.

### (Models)[http://emberjs.com/guides/models/]

```javascript

App.Person = DS.Model.extend({
  firstName: DS.attr('string'),
  birthday:  DS.attr('date')
});

App.Order = DS.Model.extend({
  lineItems: DS.hasMany('lineItem')
});

App.LineItem = DS.Model.extend({
  order: DS.belongsTo('order')
});

```

When data comes back from the api, the serializer will normalize
it in a way ember expects it. When asking for an object, it will
first ask the cache, and then the api. Later on when asking, it
will hit the cache. To force a reload, 

```javascript

App.PostRoute = Ember.Route.extend({
  model: function(id){
    var post = this.get('store').find('post', id); // Find the post from the store
    post.reload(); // Force a reload
    return post;  // Return the fetched post NOW with incomplete/outdated info. When the api answers, the information will be updated.
  }
});

```

### (Routing and Routes)[http://emberjs.com/guides/routing/]

Models become resources and actions for the models are nested under in routes.
Here is an example of routing with dynamic segments.

```javascript

//This one uses ember data, ember just knows how to handle ids

App.Router.map(function() {
	/* posts */
  this.resource('posts');
  this.resource('post', { path: '/post/:post_id' });
});

//This is the route for post
App.PostRoute = Ember.Route.extend({
  model: function(params) {
    return this.store.find('post', params.post_id);
  }
});

//Or something like this

App.Router.map(function() {
  this.resource('post', {path: '/posts/:post_slug'});
});

App.PostRoute = Ember.Route.extend({
  model: function(params) {
    // the server returns `{ slug: 'foo-post' }`
    return jQuery.getJSON("/posts/" + params.post_slug);
  },

  serialize: function(model) {
    // this will make the URL `/posts/foo-post`
    return { post_slug: model.get('slug') };
  }
});
```

You can't nest under a route. Also remember the natural nesting of routes.
First comes the application template and then it nests the index route 
template. Resources and routes at this level take the place of the index route
and template. If you have futher nesting, say with a post and then actions on
the post. there is a post template with an outlet that will render the 
post.index and other routes or resources take the place of the index. The
routes index is the base route for any level of in the hierarchy.

Also remember the route sets up the controller. Put setup information here.

### (Controllers)[http://emberjs.com/guides/controllers/]

Any actions that happen in the template need to be defined here.
The controller is the templates gateway to the model. Object controller
for 1 object models and Array controller for list models. Attributes
in the template need to be defined on the Controller/Model

```javascript

App.SongController = Ember.ObjectController.extend({
  soundVolume: 1
});

App.SongsController = Ember.ArrayController.extend({
  longSongCount: function() {
    var longSongs = this.filter(function(song) {
      return song.get('duration') > 30;
    });
    return longSongs.get('length');
  }.property('@each.duration')
});
```

### (Templates)[http://emberjs.com/guides/templates/the-application-template/]

Templates are in HTML and handlebars.

```handlebars

<ul>
  {{#each people}}
    <li>Hello, {{name}}!</li>
  {{/each}}
</ul>

//Doesnt change the inner scope unless using friend

{{name}}'s Friends

<ul>
  {{#each friend in friends}}
    <li>{{name}}'s friend {{friend.name}}</li>
  {{/each}}
</ul>

//For binding to a property on the controller, in this instance it is src
<div id="logo">
  <img {{bind-attr src=logoUrl}} alt="Logo">
</div>

//For linking to another route, there are also transitions that can happen
//in the routes
{{#link-to "photos" data-toggle="dropdown"}}Photos{{/link-to}}

```

Remember to write helpers for complicated handlebars.

### (Views)[http://emberjs.com/guides/views/]

When you need sophisticated event handling, write and use a view.

### (Components)[http://emberjs.com/guides/components/]

Create custom tags with javascript backed behavior. Feels a lot like
directives in angular, kinda. Makes certain things more modular and
reusable between apps.

### (Testing)[http://emberjs.com/guides/testing/]

Write automated integration and unit tests.