---
title: "What to do for a MVP"
published: true
layout: single
tags: [startup, mvp]
---

Knowing what to do is critical for early stage.

I have been in this position of having 100+ ideas, and just a couple of weeks of work.

It happened to me in SayFly! and during the development of Finza.

There are many, many things to do, but here there are some tips for developing that MVP.

1. Use an OAuth provider.
2. Build what hurts.
3. Cloud Providers.
4. Build Vs Buy.
5. 1 Platform.

## Use an OAuth provider.

Creating an account for a user is more complex than it looks on the surface.

You have to: Avoid duplicates, Manage passwords, Account stealing/retrieval, Manage a token policy...

The best approach for this problem is to avoid it as much as possible.

You don't manage passwords, neither account stealing or support, almost all of that is managed by google(Or your provider of choice)

Use Google OAuth and make your users connect through google, in the future you can add more options, but this
is a time saver.

## Build what hurts.

Build whatever makes your client throw a pile of money to your face.

Simple as that, there are many "Nice to have" that are for later.

If your user isn't complaining of how slow a certain menu is, just ignore it for later.

It is so easy to continue building, just throw that idea away and focus on what is the pain of the client.

If that pain is the menu, congrats you have clients that care enough for your product that they need some QoL features.

## Cloud Providers.

There are many options, and you won't give your PC to the client.

And setting your own infra comes with friction and risks.

So choosing a good provider is key to avoid this friction.

In this stage I would rule out almost anything that doesn't let you make a service from a repository.

No need to handle VPS, serverless and other different services.

You just need to push your changes to the repo and have a DB for data storage.

Railway is a good provider for early stage.

You don't need anything else.

## Build Vs Buy.

That's the eternal debate.

"Why pay for this SaaS when I could do it for free?" - *everyone*

Because it saves time, the SaaS model isn't sustained from a high barrier of entry, it is sustained by laziness.

And maybe you need to be a bit lazy, once in the future you can improve your cost structure.

But TODAY you want to test, and test fast.

You are paying for speed, so any buy you do using this mindset is great.

## 1 Platform.

Make it for only 1 platform, either mobile, desktop, or web.

And preferably it should be web.

If you use a multiplatform framework, you can jump from web to mobile in a week.

And desktop users usually have access to a web browser, and so do mobile users.

The Web is the "universal" platform, and you should leverage that platform to your advantage.

## My recommendations.

If you are building a MVP today I would pick google oauth, with railway for infra, creating a webapp with React Native(A cross platform framework for web and mobile).

This would save time and resources, and give you flexibility for the future.
