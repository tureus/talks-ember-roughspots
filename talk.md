### Ember Roughspots

Xavier Lange <xrlange@tureus.com>

---

### Do I know what I'm talking about?

## Sure.

---

### What's Xavier gonna talk about?

 * Experience
 * Thoughts
 * Regrets

---

### Backbone.js

An SPA with `marionette.js`.

 * A pain to build (`require.js` was tricky)
 * Models were weak
   * Synchronizing view state through event bus
   * Naming things is trouble
 * Managing login/logout state was a pain for a newb
 * How am I qualified to choose a template system?
   * And optimize it for deployments?
 * Share-nothing architecture was "slow" but easy to reason about
 * Tests? As if.

---

### Toying with Ember.js

## First-impressions are hard to shake.

 * Binding data is so easy!
 * Active community focused on the "omakase" experience
 * IE8 support!
 * Feels snappy.
 * Identity-map driven data layer
   * With ActiveRecord-style assocations?!
 * And a sweet mascot?!

---

### It seems like there's always been pushback...

---

My rails coworkers would say

    we can't just throw away a working app

adding on

    ... for an `unproven` framework

Or

    I'd much rather have a `sprinkling` of JS

(because JS is weirder than usual. `NaN !== NaN`, etc)

---

A client's possible gut reaction:

    How much is this going to cost me?

Or, scarier still:

    I can't risk having to search for ember experts

---

A real thing I was told

<blockquote>
But at the moment, the team as a whole isn't well equipped to work in the ember framework and given the amount of work that needs to be done, I don't think it's economical to pursue it until we have the bandwidth to allow everybody to learn a new framework.
</blockquote>

---

Person quoted really wanted to hack a single, standalone, complex, `D3`-driven view using `react.js`.

---

#### My experience

##### 5 full-scale Ember applications

More than a year of full-time Ember work. Scary-enough: all apps from scratch.

Working with, among other things:

 * REST API
 * `i18n`
 * WebSockets with custom ember-data adapter
 * IE8-compliant drag'n'drop interactions

---

### A content management system for experts

 * Home-rolled login
 * Rails backend
 * Rails build system
 * Was lucky to be in sync with rails-compatible libraries
 * Backend was well tested, frontend had none

---

### A realtime app with websockets

 * Talked to a robot (how neat!)
 * Open WS connections to 10+ robots at once
 * Slow on data writes (probably the robots, maybe the preparation of data)
 * `ember-data` was amazing for bringing together WebSocket-backed objects and REST API-backed objects
 * Promises are a fantastic way of orchestrating async behavior
 * Should not have kept 10MB JPEGs in data store

---

### 2 snappy interactive surveys

 * The most-straightforward of my Ember apps
 * Great user experience, snappy going through pages
 * Nested routes work so well
 * IE8 support. Even if IE8 itself acts funny.
 * First foray in to ember-cli

---

### Form-driven, wizard-y tool

By far the most advanced Ember app I've built. Used everything learned in other apps.

 * `simple-auth` for login management
 * multi-model forms
 * typeaheads
 * server-side validations
 * had popups, removed them
 * `easyform` ripped out by designer in deadline who could not style it (library ergonomics)
 * Many async relationships

---

### simple-auth

 * Some say SPA login is antipattern but this works well
 * Token-based authentication is a good pattern
 * Is not strict on session input data and will "break" silently
   * That's kind of a JavaScript thing

---

### Multi-model forms

We aren't building pure CRUD anymore so you'll have complex forms. Ember/ember-data rock for organizing those forms but you'll still have slow API calls when each saving each. Plus you'll have to up your Promises game to save hierarchal/associated models.

---

### API-driven Typeaheads

Use POJOs for the inputs then handle select event to copy data to model. Data binding is weird. Hard lesson to learn.

Brings up the issue how to build reusable components which talk to the API. So many different paths to take. Which is the right one?

---

### Server-side validations

Interactive validations are the best UX but you can do a lot less work by just piggy backing on API conventions

 * HTTP 422 to trigger Data.Errors objects to be filled in
  * Super tricky to setup correctly, documentation is sparse
  * Not easy to mix/match with model-side validations
 * Then you can bind to the errors structure in your view
 * Errors go away when the field is modified
 * Difficult to put together just right
  * Need to make sure every `save()` has a failure callback in the chain
   * You always do that, right? Right?

---

### Popups/modals

Puts weird code in to your application template so it can handle click events inside/around your modal

Difficult to send signals from popups to the active route. Send action up with promise handle to communicate back down (fancy!)

Signals go up not down.

For that reason I didn't want to use them. And my bias against them: sudden, obscuring changes in the UI seem like bad `<form>` UX.

---

### `easyform` library

A rails-y way of structuring forms -- has almost the right form semantics.

 * Difficult for the non-ember designer to wrap in the divs he wanted
 * Ripping it out meant we lost true `<label>` support
  * Ember manages all the `id` properties on inserted elements and `easyform` is the only library willing to do the legwork

---

### Complex UIs also have complex persistence requirements

 * You'll need to mix adapters and custom transforms
 * Promise-fu will be required
   * Handling errors will feel very complex

-- 

### Testing all paths through app seems necessary

Surprises when routing between sibling routes

    * You may not rely on `model` hooks being fired
    * You controller may need to access sibling controllers to keep values consistent

        ember-data models in the 'invalid' state will cause runtime error if you try to `push` over them

        always reset their state.

        you can leak memory, unsaved records will hang out

### `async` assocation will reduce the readability of your code

    and you'll always have to consider "will this access cause a server fetch?"

    you will find got'chas with trying to remove models from store which may be async

### Observers are not necessarily removed in a down-up fashion

    You'll get flashes of blank data on your screen as objects are torn down
    Worse, the observers you write may reset data. Certain plugins will change bound values as they tear down. Is it a bug? Yes. But these are popular plugins.

Note: You'll need to verify the view is not marked as 'dead'.

### Event bubbling is complicated by nested components

 * You'll contstruct nice reusable components but then worry about 
  how to get their data flowing up to the router/controller
 * Nest components may not render properly in tests

---

Typeaheads

    * Which part of your code knows how to build URLs?
    * Should it use ember-data?

---

### Ember gives you fantastic tools. It also gives a few answers. But more importantly, your universe just got more complicated.

And please note:

  * Angular won't solve that
  * React.js won't solve that

---

### How to work around these issues?

  * Be compulsive in learning what's going on in Ember
  * Be ready to defend your choice of framework
  * These pains are felt.
  * The learning curve is real. You're building apps in a whole new way. Upon reflections it feels like the "real" way to build apps but no programmer wants to work harder than they have to.