---
layout: post
title: rails4 rest api
categories: ['rails']
tags: ['rails']
published: false
---

it depends on your preference and needs.

If you are working with Ember.js front-end, I'd lean towards active_model-serializers since Ember.js was basically crafted to work well with it (Yehuda Katz is one of the mainters of active_model-serializers and is on the core team for Ember.js; he gave a talk on the topic a while back).

Quick breakdown:

Active Model Serializers

Separates the serialization concern with its own folder /app/serializers and a generator, and it behaves more like ActiveRecord in that you can define associations in the serializer, and it'll do the right things for you automatically. Ryan Bates has an excellent RailsCast episode on the topic: http://railscasts.com/episodes/409-active-model-serializers

Jbuilder

Jbuilder takes almost the opposite approach in that it considers JSON format construction is just another Rails view. You build your responses in the corresponding /app/views/ directories just like you would with view templates. And it can take on many of the characteristics of a view template like understanding what current_user is out of box (this is not as straight forward with AMS), chaining relations (@user.posts)... etc. And of course, Ryan Bates also made a RailsCast on the subject: http://railscasts.com/episodes/320-jbuilder

Alternative: Rabl

Ryan Bates (naturally) made a RailsCast on Rabl as well: http://railscasts.com/episodes/322-rabl. In concept, it's a lot closer to Jbuilder than AMS. And it's also been around longer. Personally I am not very fond of its syntax. But that's a matter of opinion.

If I wasn't working on an Ember.js project, I'd go with Jbuider for its simplicity and the more approachable concept.

As for performance, at least one user claims that you can make Jbuilder a lot faster than both Rabl and AMS: https://medium.com/@lgmspb/how-we-increased-the-speed-of-json-generation-by-3000-times-ca9395ab7337