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

### Why is Xavier giving this talk?

 * Air grievances
 * Start discussions
 * Ground expectations on Ember

Note: surprisingly enough, I have found two other talks in the past few months on this subject. Ember is hitting a maturity level where the "rubber has hit the road". Productivity of the framework is being analyzed and it's hard to tell the difference between ember and just javascript/client-side apps.

---

### Bias much?

<img src="/slide_assets/images/snarky_and_anti_ember.png"></img>

---

<img src="/slide_assets/images/hype_cycle.png"></img>

---

#### Do I know what I'm talking about?

#### Sure. Why not?  <!-- .element: class="fragment" data-fragment-index="1" -->

Note: I've built many apps, most from scratch

---

#### Project-oriented talk

Note: I'll prove it to you all

---

### Backbone.js

An SPA with `marionette.js`.

 * A pain to build
 * Models are weak
 * Managing login/logout state was a pain
 * Stack choices can induce paralysis
 * Slow but easy to reason about

Note: `require.js` was tricky, synchronizing parent containers through eventbus, optimize templates for deployments

---

### Toying with Ember.js

#### First-impressions are hard to shake.

 * Easy data binding! <!-- .element: class="fragment" -->
 * Active community! <!-- .element: class="fragment" -->
 * Omakase experience! <!-- .element: class="fragment" -->
 * IE8! <!-- .element: class="fragment" -->
 * Feels modern and snappy! <!-- .element: class="fragment" -->
 * Identity-map data layer! <!-- .element: class="fragment" -->
 * And a sweet mascot?!! <!-- .element: class="fragment" -->

Note: `ActiveRecord`-style assocations are rad.

---

<img src="/slide_assets/images/tomsterflying.jpg"></img>

<h6>http://devswag.com/products/ember-mascot-stickers-4</h6>

Note: sweet mascot bro

---

#### Despite all that...


### ... there's always pushback <!-- .element: class="fragment" data-fragment-index="2" -->

---

My rails coworkers would say:

<blockquote class="fragment" data-fragment-index="1">we can't just throw away a working app</blockquote> 

<blockquote class="fragment" data-fragment-index="2">... for an `unproven` framework</blockquote>

<span class="fragment" data-fragment-index="3">
Or
</span>

<blockquote class="fragment" data-fragment-index="3">I'd much rather have a <strong>sprinkling<strong> of JS</blockquote>

---

A client's possible gut reaction:

<blockquote class="fragment" data-fragment-index="1">
How much is this going to cost me?
</blockquote>

<span class="fragment" data-fragment-index="2">
Or, scarier still:
</span>

<blockquote class="fragment" data-fragment-index="2">
I can't risk having to search for ember experts
</blockquote>

---

A real thing I was told...

---

<blockquote style="text-align: left">
But at the moment, the team as a whole isn't well equipped to work in the ember framework and given the amount of work that needs to be done, I don't think it's economical to pursue it until we have the bandwidth to allow everybody to learn a new framework.
</blockquote>

anon

---

### So not everyone will be a fan

Note: Duh. Person quoted really wanted to hack a single, standalone, complex, `D3`-driven view using `react.js`. Ember slowed him down with a learning curve and shiny object syndrome made the *easy* path more appealing

---

### I have given Ember a more-than-fair shake

Note: so this will be a project-centric talk

---

#### My experience

##### 5 full-scale Ember applications

More than a year of full-time Ember work.

 * REST API
 * `i18n`
 * WebSockets with custom ember-data adapter
 * IE8-compliant drag'n'drop interactions

Note: scary-enough: all apps from scratch. Worked with the following technologies.

---

### A content management system for experts

 * Home-rolled login
 * Rails backend
 * Rails build system
 * Was lucky to be in sync with rails-compatible libraries
 * Backend was well tested, frontend had none

Note: libraries have always felt like a pain point. I have ejected libraries over time because they fell out of sync. breaking tests, platforms.

---

### A realtime app with websockets

 * Talked to a robot (how neat!)
 * Open `WebSocket` connections to 10+ robots at once
 * Slow on data writes
 * Had tests

Note: adapter wrapped a vanilla JS WS lib I wrote before committing to Ember. Slowness was probably the robot. Test runner was weird.

---

### A realtime app with websockets (cont.)
 * `ember-data` was amazing for handling `WebSocket`-backed objects tied to REST API-backed objects
 * `Promise`s are a fantastic way of orchestrating async behavior
 * Should not have streamed 10MB JPEGs to the data store

Note: Storing base64 10MB images was a hack to work around easy access to S3

---

### 2 snappy interactive surveys

 * The most-straightforward of my Ember apps
 * Great user experience, snappy going through pages
 * Nested routes work so well. Hierarchal data is nice.
 * IE8 support. Even if IE8 itself acts funny.
 * First foray in to `ember-cli`

Note: not necessarily easily tweaked, but once I could do cool custom stuff with the router

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

Note: just a list of the things I used. no judgement. yet.

---

# On that fifth app, what did I learn?

---

### Start the parade...

---

### simple-auth

 * Workable solution
 * Token-based authentication feels natural
 * Fails to call out missing, important properties
 * You'll need to build an expiring token for personalized server-generated items

Note: feels like true API consumer (models how a native app would work). poor ergonics (should be a bug report). trouble downloading server-rendered assets like PDFs. could just hack it out with cookies but then no longer a TRUE API consumer.

---

### Multi-model forms

  * `ember`/`ember-data` rock for organizing complex forms
  * up your `Promise`s game
  * rendering many components will be *slow*
  * parallel-loading data from `model` complicates `modelFor`

Note: assocations turn in to serial requests with independent roundtrip time. pre-glimmer is slow, of course. nesting routes with `modelFor` means everyone has to know the hash, tight coupling on something that's tedious to test.

---

### Multi-model forms (cont.)

### Upside: keeps backend simple with resource-centric logic

Note: backend doesn't handle nested data. Expense of high latency, serialized requests. Fast backend more important than ever

---

### API-driven Typeaheads

 * Plain-ole JavaScript objects (POJOs)
 * Binding should be one-way
 * How reusable are `Ember.Component`s which talk to the API?

Note: my pattern is to copy AJAX data to model. Hard lesson to learn. So many different paths to take -- which is the right one?

---

### `ember-data` server-side validations got'chas

Interactive validations are the best UX but you can do a lot less work by just piggy backing on API conventions.

Less work can be good, `mm'kay`?

---

### `ember-data` server-side validations got'chas (cont.)

 * `HTTP 422` to fill in `Data.Errors`
 * Not easy to mix/match with client-side validations

Note: Tricky to setup correctly, documentation is sparse, throws runtime exceptions if you don't catch it properly. Can't use libraries like `ember-validations` with this technique (there is a PR to address this)

---

### `ember-data` server-side validations got'chas (cont.)

 * Difficult to put together just right
  * Every `save()` has a failure callback in the chain

Note: you always do that, right? Right?

---

### `ember-data` server-side validations got'chas (cont.)

 * Then you can bind to the errors structure in your view
 * Errors go away when the field is modified

---

### Building robust forms<br/><strong>(with error handling)</strong><br/>as challenging as ever

Note: maybe the toolset fits you brain better? but still difficult to maintain.

---

### Popups/modals

 * Weird code in to your `application.hbs`

 * Signals go up not down (did they close the popup?)

 * Ripped out after pain and suffering

Note: <ul><li>so it can handle click events at the proper `DOM` level</li><li>send action up with promise to communicate back down. Way too fancy</li><li>And my bias against them: sudden, obscuring changes in the UI seem like bad `<form>` UX.</li></ul>

---

### `easyform` library

A `rails`-y way of structuring forms

 * Difficult for the non-ember designer to wrap in the divs he wanted
 * Ripping it out meant we lost true `<label>` support

Note: <ul><li>label support!<li> <li>anecdotal evidence from a time-crunched project.</li> <li>dev had very little ember experience.</li> <li>Ember manages all the `id` properties on inserted elements and `easyform` is the only library willing to do the legwork to wire things up.</li> <li>should be done by `ember-data` library.</li>

---

### Async associations

 * Feels like a great way to reduce API calls

 * All accesses have additional burden of being a promise

Note: keep object relationships clean. breaks sequential flow of code (more callbacks!). you can cheat if you want and access content directly.

---

### Nested Components

 * Lack composability for actions

 * Added level of indirection in `sendAction`

 * Must maintain path for action bubbling

 * Component actions will fizzle out, not throw exceptions like controller/route actions

Note: move a component one-level down and your actions will silently disappear. deeply (?) nested components is considered an anti-pattern.

---

### Observers

 * Worked great on first look
 * Ember view lifecycle introduced many surprises
 * Don't use them
 * You may need to subclass `<input>` views to emit actions

Note: I am actively removing observers from code I have written

---

### Testing

 * Great scaffolded code shows you the ropes
 * Fast because it's hooked in to backburner
 * Built in to the standard tool
 * Hard to beat the ease of getting started

Note: overall very happy with this. but it has issues still...

---

### Testing (cont.)

 * Your asynchronous code won't work and you won't understand what to do or why
  * Idiomatic code isn't testable

Note: The docs are a lie: surprisingly difficult to figure out the "just do this" part. The trick is wrapping async code with `Ember.run`. Confusing? Right.

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
 * Under certain situations, a store load can fail

Note: you said you wanted a stateful UI...

---

### Ember gives you fantastic tools

### It also gives a few answers

### But not all.

And please:

  * Angular won't solve that
  * React.js won't solve that

Note: I still see posts online asking how to hook in to the view lifecycle. and other projects are not tackling client-side data

---

### Going back, would I use ember again?

 * Many failures are organizational
 * Many failures are solved by experience
 * Best way to implement all the UI designs as presented

Note: <ul><li>Will you find people with that experience?</li><li>Static HTML will be cheaper</li><li>... at the expense of fanstic UX</li><li>If you're willing to fight for the best UX, Ember is a credible way to scale that out</li></ul>

---

### You didn't answer the question!

---

#### Ember is not dead. It is not dying

---

#### Ember greases the wheels of collaboration

---

#### Ember is tackling <strong>HARD</strong> problems

---

#### Client-side applications are root of this evil

#### And they're here to stay <!-- .element: class="fragment" data-fragment-index="1" -->


---

#### I have not used a better tool for creating UX

---

<img src="/slide_assets/images/picgifs-deal-with-it-52101.gif"></img>

---

### How to work around these issues?

  * Be compulsive in learning what's going on in Ember
  * Be ready to defend your choice of framework
  * These pains are felt. Share them.
  * The learning curve is real

Note: You're building apps in a whole new way. Upon reflection: it feels like the "real" or "right" way to build apps. Code has the right home. But it will be hard work.

---

### Talks Similar To Mine

##### I stole some ideas from these folks

##### They're generally upbeat

[Ember.js: An Antidote To Your Hype Fatigue](https://speakerdeck.com/chancancode/ember-dot-js-an-antidote-to-your-hype-fatigue)

[Ember in the Real World](https://speakerdeck.com/tehviking/ember-in-the-real-world)

---

Xavier Lange

<xrlange@tureus.com>

`@tureus`

<em style="font-size: 20px">Pssst: I'm available to hire</em>
