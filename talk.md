### Ember Roughspots

Xavier Lange

<xrlange@tureus.com>

`@tureus`

---

### What's Xavier gonna talk about?

 * Experience
 * Thoughts
 * Regrets

---

### Do I know what I'm talking about?

## Sure.

---

### Backbone.js

An SPA with `marionette.js`.

 * A pain to build (`require.js` was tricky)
 * Models were weak
   * Synchronizing parent containers through eventbus
 * Managing login/logout state was a pain
 * How am I qualified to choose a template system?
   * And optimize it for deployments?
 * Share-nothing architecture was "slow" but easy to reason about

---

### Toying with Ember.js

#### First-impressions are hard to shake.

 * Binding data is so easy!
 * Active community!
 * Technical choices focused on the "omakase" experience!
 * IE8 support!
 * Feels modern and snappy!
 * Identity-map data layer!
   * With ActiveRecord-style assocations?!!
 * And a sweet mascot?!!

---

### But there's always pushback...

---

My rails coworkers would say

<blockquote>we can't just throw away a working app</blockquote>

<blockquote>... for an `unproven` framework</blockquote>

Or

<blockquote>I'd much rather have a <strong>sprinkling<strong> of JS</blockquote>

---

A client's possible gut reaction:

<blockquote>
How much is this going to cost me?
</blockquote>

Or, scarier still:

<blockquote>
I can't risk having to search for ember experts
</blockquote>

---

A real thing I was told...

---

<blockquote>
But at the moment, the team as a whole isn't well equipped to work in the ember framework and given the amount of work that needs to be done, I don't think it's economical to pursue it until we have the bandwidth to allow everybody to learn a new framework.
</blockquote>

anon

---

Person quoted really wanted to hack a single, standalone, complex, `D3`-driven view using `react.js`.

Ember slowed him down with a learning curve and shiny object syndrome made the *easy* path more appealing

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
 * Open `WebSocket` connections to 10+ robots at once
  * Connection lib wrapped by data adapter
 * Slow on data writes (probably the robots, maybe the preparation of data from ember-data, maybe my JS)

---

### A realtime app with websockets (cont.)
 * `ember-data` was amazing for handling `WebSocket`-backed objects tied to REST API-backed objects
 * `Promise`s are a fantastic way of orchestrating async behavior
 * Should not have streamed 10MB JPEGs to the data store
  * Easiest work-around for lack of access to S3 endpoint

---

### 2 snappy interactive surveys

 * The most-straightforward of my Ember apps
 * Great user experience, snappy going through pages
  * Easy to tweak for weird requirements
 * Nested routes work so well. Hierarchal data is nice.
 * IE8 support. Even if IE8 itself acts funny.
 * First foray in to `ember-cli`

---

### Form-driven, wizard-y tool

By far the most advanced Ember app I've built. Used everything learned in other apps.

 * `simple-auth` for login management
 * multi-model forms
 * typeaheads
 * server-side validations
 * modals with forms
 * `easyform` for more-or-less semantic forms
 * async relationships

---

# On that fifth app, what did I learn?

---

### simple-auth

 * Workable solution
 * Token-based authentication feels natural
  * True API consumer
 * Fails to call out missing, important properties
   * That's kind of a JavaScript thing
   * Poor ergonomics (should be a bug report)
 * You'll need to build an expiring token for personalized server-generated items
   * Dynamic PDFs can't be linked using API token
   * Or just hack it out with cookies. Relaxing your true API consumer.

---

### Multi-model forms

  * `ember`/`ember-data` rock for organizing complex forms
  * you'll have to up your Promises game to save hierarchal/associated models
  * rendering many components will be *slow* (pre-`glimmer`, of course)
  * parallel-loading data from `model` callback will make `modelFor` more complicated

---

### Multi-model forms (cont.)

 * Upside: keeps backend simple with resource-centric logic
  * Don't have to handle nested structures
  * Composable at the expense of high latency, serialized requests
    * Fast backend more important than ever

---

### API-driven Typeaheads

Use POJOs for the inputs then handle select event to copy data to model. Data binding is weird. Hard lesson to learn.

Brings up the issue how to build reusable components which talk to the API. So many different paths to take. Which is the right one?

---

### `ember-data` server-side validations got'chas

Interactive validations are the best UX but you can do a lot less work by just piggy backing on API conventions.

Less work can be good, mm'kay?

---

### `ember-data` server-side validations got'chas (cont.)

 * HTTP 422 to trigger Data.Errors objects to be filled in
  * Tricky to setup correctly, documentation is sparse, throws runtime exceptions if you don't catch it properly
  * Not easy to mix/match with client-side validations
    * content for `errors` property
    * there is a PR to address this.
 * Difficult to put together just right
  * Need to make sure every `save()` has a failure callback in the chain
   * You always do that, right? Right?

---

### `ember-data` server-side validations got'chas (cont.)

 * Then you can bind to the errors structure in your view
 * Errors go away when the field is modified

---

### Popups/modals

Puts weird code in to your `application.hbs` so it can handle click events at the proper `DOM` level

Signals go up not down: send action up with promise to communicate back down. Way too fancy.

For that reason I didn't want to use them. And my bias against them: sudden, obscuring changes in the UI seem like bad `<form>` UX.

---

### `easyform` library

A rails-y way of structuring forms -- has almost the right form semantics.

 * Difficult for the non-ember designer to wrap in the divs he wanted
  * Anecdotal evidence, was on a deadline so couldn't do any handholding
  * Unfortunate reality of poor library ergonomics
 * Ripping it out meant we lost true `<label>` support
  * Ember manages all the `id` properties on inserted elements and `easyform` is the only library willing to do the legwork to wire things up

---

### Async associations

Feels like a great way to reduce API calls and keep object relationships clean

Now all access have additional burden of being a promise. Breaks sequential flow of code.

Now all writes have additional burden of being a promise. You have to 'unwrap' a relationship

---

### Nested components

Components lack composability for actions

Must have added level of indirection through `sendAction`

Must build actions path for nested components to keep bubbling.

Component actions will fizzle out, not throw exceptions like controller/route actions

---

### Observers

Sigh.

---

### Testing

 * Great scaffolded code shows you the ropes
 * Fast because it's hooked in to backburner
 * Built in to the standard tool
 * Hard to beat the ease of getting started

---

### Testing (cont.)

 * Your asynchronous code won't work and you won't understand what to do or why
  * Programmers do not love or appreciate that surprise
  * Idiomatic code isn't testable
   * The docs are a lie

---

### Testing (cont.)

 * I have had issues testing nested components
  * Probably an older library issue, had to move on due to time

---

### Testing (cont.)

 * Hard to tell when your app is truly tested
  * Surprises when routing between routes and sibling-routes
   * Model will be cached!
   * You'll need to access sibling controllers directly to modify their cached data
 * Can you test your `ember-data` isn't leaking records between routes?
  * Unsaved, dirty records will just hang out

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

---

Xavier Lange
<xrlange@tureus.com>
`@tureus`

---

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

