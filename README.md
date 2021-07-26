# budget-trackers

## Description 

I was asked to add functionality to an existing Budget Tracker application to allow offline access and functionality.
The user will be able to add expenses and deposits to their budget with or without a connection. When entering transactions offline, they should populate the total when brought back online.

## Getting Started

```npm i``
To install all of the dependencies you'll need

```mongod```
To start the database

## What we did

Key Files

index.js - Main application JavaScript file
service-worker.js - Application's service worker file

In order to enable the offline Functionality of entering deposits and expenses offline, we registered service-worker and applied caching resources. 

- Register service worker
Created a service worker file, so to make sure the changes show up, then to register it in our application I added the following code to the top of index.js file:

```if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
      .then((reg) => {
        console.log('Service worker registered.', reg);
      });
  });
}
```
The code above registers the empty service-worker.js service worker file once the page has loaded. 

- Cache resources
In order to get offline functionality working, i wrote the code in service-worker.js so that the browser should be able to respond to network requests and choose where to route them. 

During the service worker's install event, a named cache is opened using the Cache Storage API. The files and routes specified in cache Resources are then loaded into the cache using the cache.addAll method. 

The above method is called "precaching" because it caches the set of files during install time as opposed to caching them when they're needed or requested.

Once the service worker is controlling the site, requested resources pass through the service worker like a proxy. 

Each request triggers a fetch event that, in this service worker, searches the cache for a match, if there's a match, responds with cached resource. If there isn't a match, the resource is requested normally.

Caching resources allows the app to work offline by avoiding network requests. Now the app can respond with a 200 status code when offline!

## Link