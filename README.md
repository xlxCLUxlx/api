# The Bungie.Net API

Full Documentation: https://bungie-net.github.io/multi/index.html

Documentation as one single, gigantic page: https://bungie-net.github.io

This section will be added to as time goes by - we're working with it as we go!

12-12-2017 - The API has been updated again!  See the new CHANGELOG.md (thanks for the suggestion hskrasek!) where I'll put the details of subsequent releases from now on instead of cramming them all in the readme.

# The State of the API

The API itself is online, however several endpoints are still in a beta or unusable state.  Stay posted and keep an eye peeled on the documentation for further updates.

# I'm New to the API.  What can I do with it?
- (TODO: Create a grand overview tour of Destiny API features for new users in wiki, including how to get mobile databases and what the heck a mobile database is)

# How do I register to use the API?
- (TODO: Move registration documentation to wiki)

# Are there any restrictions on the API?
- (TODO: Pay more attention to the man behind the curtain.  Also write about the throttles etc...)

# What about the Destiny 1 API?
- It is alive and well, under the /d1/Platform/Destiny/ root URL.  We unfortunately don't have the time to write documentation for the D1 API, but community member Lowlines made [this fantastic documentation](http://destinydevs.github.io/BungieNetPlatform/docs/Getting-Started).  Buy him a beer sometime, or name your first kid after him or something.  Or donate to him if he accepts donations, which he should because the work he did in D1 was totally boss.

# I'm a Destiny 1 API Veteran.  What's new in the API?

- ### Component-based Destiny Requests
  - [Destiny2.GetProfile](https://bungie-net.github.io/multi/operation_get_Destiny2-GetProfile.html#operation_get_Destiny2-GetProfile), [Destiny2.GetCharacter](https://bungie-net.github.io/multi/operation_get_Destiny2-GetCharacter.html#operation_get_Destiny2-GetCharacter), and [Destiny2.GetItem](https://bungie-net.github.io/multi/operation_get_Destiny2-GetItem.html#operation_get_Destiny2-GetItem) are now the fundamental building blocks of Destiny data, replacing our large and disparate specialized endpoints.
  - GetCharacter and GetItem are provided for situations where you know the specific character or item you're looking at in your UI, but we fully anticipate that most calls will be made through GetProfile.  At any layer of depth, you can now get any data for the layer below by passing the components you want returned in the ?components= querystring parameter of the method.  (NOTE: I would link to DestinyComponentType here, but I just noticed it's not being output yet.  I will fix that in the morning.  In the meantime, you can infer the components by examining the Response objects from the methods linked above)
  - For example, you can pass the CharacterInventories component to GetProfile, and you will receive the full inventory for all characters (provided that they have granted you permission to access that data, in the same way as in Destiny 1: our privacy policies and settings have not changed since D1, and still apply to D2 data).
  - In this way, my hope is that we can reduce the number of round trips you're currently having to make, while letting you control how much bandwidth you use up.  Ask for only the components that you need!

- ### Milestones
  - Advisors have disappeared, but in their place are ["Milestones"](https://bungie-net.github.io/multi/schema_Destiny-Definitions-Milestones-DestinyMilestoneDefinition.html#schema_Destiny-Definitions-Milestones-DestinyMilestoneDefinition).  Mirroring the game's concept of Milestones, these reflect all of the activities that a player can do in game.  Essentially these become Advisors with matching Definition data, which reduces the payload we have to return in live requests and makes including Milestone information as part of a Profile or Character request more feasible.
  - We will, as we did with Advisors, tack on additional data that we can associate with the Milestone where possible and add to it over time.
  - Milestones will, like Advisors, continue to evolve over time - and, like Advisors, I will probably come to regret how I implemented our view of them.  But for now, I feel hopeful.
  - Like Advisors, there are "Public" Milestones: views of what can be done in the game right now agnostic to an individual player's state.  For this information, you will want to seek out [Destiny2.GetPublicMilestones](https://bungie-net.github.io/multi/operation_get_Destiny2-GetPublicMilestones.html#operation_get_Destiny2-GetPublicMilestones).
  
- ### The Near-Deprecation of Talent Grids
  - In an act that I can only describe as Benevolence, the designers turned away from Talent Grids like they were going to turn into a pillar of salt.
  - Unfortunately, Talent Grids aren't dead entirely.  Items may still have them, and they have even more rules stacked on top of how they work now, to support the one place where they currently are still being used: Character Builds/Subclasses.
  - Please see [DestinyTalentGridDefinition](https://bungie-net.github.io/multi/schema_Destiny-Definitions-DestinyTalentGridDefinition.html#schema_Destiny-Definitions-DestinyTalentGridDefinition) to delve into the abyss.
  - Eventually, I would like to create a derivative definition of this specifically for Builds in an attempt to simplify what you have to understand: but I did not find time to do so before release (and I'm not 100% confident that I'd be able to remove enough complexity for it to be worth it to create a derivative definition).

- ### The Ascent of Sockets for Item Customization
  - In the place of Talent Grids, the notion of Sockets and Plugs are now the primary means of customization of items.
  - Read more in [DestinyItemSocketBlockDefinition](https://bungie-net.github.io/multi/schema_Destiny-Definitions-DestinyItemSocketBlockDefinition.html#schema_Destiny-Definitions-DestinyItemSocketBlockDefinition) and its children, as well as [DestinyItemPlugDefinition](https://bungie-net.github.io/multi/schema_Destiny-Definitions-Items-DestinyItemPlugDefinition.html#schema_Destiny-Definitions-Items-DestinyItemPlugDefinition) to find out how Sockets and Plugs work.  (to note, both of these are child properties of [DestinyInventoryItemDefinition](https://bungie-net.github.io/multi/schema_Destiny-Definitions-DestinyInventoryItemDefinition.html#schema_Destiny-Definitions-DestinyInventoryItemDefinition), that old venerable bolt-on of functionality.

- ### (Add more... probably redirect this to a set of wiki pages)

# What Endpoints are not ready?

- ### Vendors
  - We discovered breaking changes with how Vendors work under the surface pretty late in our development cycle, and were unable to recover in time to stabilize both it and our other features in time to ship.  Those of you who used Vendors in Destiny 1 remember the great Vendor meltdown of 2016.  The implementation that I built to work around said performance meltdown will no longer work in Destiny 2, which puts me back at the drawing board for finding a way to expose Vendor data in an efficient enough manner to melt neither our servers nor the game's servers.  I regret this deeply - particularly discovering it so late and thus not having time to pivot - but I am using this opportunity to pre-release the proposal for the format in which the Vendor calls will return data for review while I continue to work on making Vendors function again.  It is one of the highest priorities on my plate for post-ship. (2017-12-07 as a small update, in practice this ended up not being my highest priority: but I've been chipping away at it as time allows, and hope to have something usable soon.)
  
# Swagger/OpenAPI Specs (or, how to generate your own clients for the BNet API)

- Grab [openapi.json](https://github.com/Bungie-net/api/blob/master/openapi.json) for the OpenAPI 3.0.0 specs
- Grab [openapi-2.json](https://github.com/Bungie-net/api/blob/master/openapi-2.json) for the 2.0.0 specs

NOTE: There are currently bugs in the generated documentation: it worked well enough to build the HTML documentation you'll see at the links above, but users are reporting a variety of issues - and in some cases, missing data - when they go to generate clients.  I will be looking into those issues this weekend!

# Extension Properties on OpenAPI Specs (or, how to generate much cooler clients for the BNet API if you want to take the time to do so)

- **x-mapped-definition** = This property has hash identifiers that map to a Manifest Database definition.  This property will reveal which type of Manifest Database definition it points to.  If it's a uint32, it points to a single definition.  If it's a List<uint32>, each value points to a different definition.  If it's a Dictionary<uint32, X>, its' keys point to different definitions.

- **x-mobile-manifest-name** = If populated, this is the name of the Mobile Manifest table that holds this data as a first-class entity.  The existence of this field implies that this entity is a first-order Destiny Definition aggregate.

- **x-enum-values** = I really hate that the OpenAPI spec doesn't have a way to tell you both the identifier and the numeric value of an enum, much less documentation for enum values, so I added this extension as a way to return that info.  It'll give you way more than the basically useless base "enum" property.

- **x-destiny-component-type-dependency** = A new concept in the Destiny 2 API is "Components".  You will see that our Destiny Profile/Character calls have been mostly simplified down to just GetProfile/GetCharacter/GetItem.  This simplification is made possible by Components, which are identifiers you pass into the requests to specify how much data you want back.  An entity with this property will only be returned if you've passed the named Component Identifier into the GetProfile/GetCharacter/GetItem methods.

- **x-dictionary-key** = OpenAPI didn't have a good way for me to specify that the key of a dictionary was actually meant to be something other than a string (even if always encoded as a string in JSON).  This extension property lets me tell you what it *really* is, so you can strongly type it and convert it if desired.

- **x-preview** = This boolean indicates that the Operation in question is not yet ready for prime time.  You will have to read the documentation to find out just *how* not ready it is, at least for now.

# FAQ

- Certain APIs are marked as PENDING in the documentation, what does that mean?
  - This may mean that their tentative contracts are being exposed in the documentation for review, or that they're going to be live but in a "Beta" state.  Look at the description of the specific endpoint for details.

- Can you provide documentation for the specific stat properties and modes that will be returned?
  - For the time being, we can't reveal that data.  I apologize for the inconvenience!

- How do I do X?  What does Feature/Property Z mean?
  - If you've got specific questions, please add a [Question](https://github.com/Bungie-net/api/labels/question) to our Issues list!  We'll try to answer as time allows, and we'll use this input to improve our documentation!
  
- I have suggestions on how to improve the API/alternative ways I'd like to see the data returned!  Where can I submit that input?
  - Add an [Enhancement](https://github.com/Bungie-net/api/labels/enhancement) request to our Issues list!  As time allows, we'll comb through them.  This will be particularly useful for Pending endpoints like Vendors, where there's still time to improve on the contracts before we turn them on.
  - Ideally, I'd like to see these become a place where the dev community as a whole can debate on the benefits and drawbacks of a change to the API, which will help us all to make the experience better for everyone.
  - It'll be a bit harder to make contract-adjusting changes to endpoints that are already release ready, but we can discuss ways to extend it.  If the enhancement is compelling enough, it may even be worth versioning the API.  But we're going to try to avoid that if at all possible.
  
- I've found a bug!
  - Please start up a [Bug](https://github.com/Bungie-net/api/labels/bug) in the Issues list!  It'll give us a good place to organize bugs that are reported, and to report back on them when they are fixed.
  - Note that the API is only part of the work that we do, and we have ongoing Bungie.Net feature work that sometimes pulls us away from the API: as such, Bugs may take a while to be resolved depending on other issues on our plate.  But my hope is that this location will help to make community-located bugs more exposed and easier to track, which should help us in our own prioritization of tasks!
  
- I need technical support for the game! (or Please give me loot!)
  - None of us here have access to the specialized support tools required to diagnose in-game issues and/or give sweet/phat lootz.  Please hit up the #Help forums on Bungie.net if you have technical support questions aside from the API itself!

- Will I be denied access to GetProfile/GetCharacter/GetItem if I lack OAuth scopes?
  - You will *not* be denied results if you don't have particular scopes, but components will be denied to you.  I don't have a good way of encoding this in the spec at the moment, but I will look into doing so in the future (potentially as an extension property, as there's no good way to encode partial success due to OAuth scopes):
  - ReadDestinyVendorsAndAdvisors is required for characterActivities, characterProgressions, kiosks and characterKiosks
  - ReadDestinyInventoryAndVault is required for profileCurrencies, profileInventory, characterInventories and vendorReceipts
  - That is all in addition to the user's privacy settings: you may still be denied the component if you're not passing their auth token and you're asking for components that they've marked private.  Your app should always assume and be able to handle the situation where an expected Component is not returned to you, even if the handling is just showing the user an error message. (if you can of course, a more elegant handling of the situation is always preferred!)
  
- Where are my consumable items?
  - Consumables are now in an Account-level bucket called "Consumables".  They are shared across all of your characters, meaning that you don't have to transfer them back and forth anymore.  Oh, glorious day!
  
- I have a project I'd like to recruit help for, or one that I'd like to show off!
  - We're trying to keep the "Issues" section specific to the API itself, BUT you should definitely go to the officially unofficial Discord for the Bungie.Net API, set up by community member Xorth: https://discord.gg/WJDSUgj There are sections over there for project promotion, general discussion and so on!
  
- I am unable to get results from endpoints that require authentication.  I always get WebAuthRequired back, even though I am passing both my X-API-Key header and the Authorization header.
  - It turns out, if you hit the API with bungie.net rather than www.bungie.net, we currently redirect you and lose the authorization header in the process.  I'd like to fix this along with resolving other redirection issues covered in https://github.com/Bungie-net/api/issues/55, but for now be careful that you're using exactly https://www.bungie.net for your URLs, and make sure that your URLs are terminating with a trailing slash (/).  Otherwise, you'll encounter our undesirable redirections.

# Known Issues

- There is currently no way to know what reward item you will get for completing a Milestone's quest.  The DestinyMilestoneDefinition defines these rewards, but late in development we realized merely returning this information is insufficient: only a subset of these are possible to be obtained at any given time.  A future release will have the live Milestone data return the "active" rewards so that you can show the correct reward being offered at any given time.

- If our environment goes down entirely, to the point where we can no longer process your request, we are currently returning the same static error HTML page(s) to any API request that the website is returning.  If you get an HTML response to an API request, know that this is the undesirable side effect of our environment being down and handle it with your own messaging and error handling as needed.

- Vendors are still not terribly useful.  They exist in an extremely early pre-release form.  The items returned should be accurate for a given character, but their stats and any other attributes will almost certainly be incorrect.  The contract itself is still also subject to change.
