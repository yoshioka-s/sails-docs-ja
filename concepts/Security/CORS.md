# Cross-Origin Resource Sharing (CORS)

<!--
Every Sails app comes ready to handle AJAX requests from a web page on the same domain.  But what if you need to handle AJAX requests
originating from other domains?
-->

[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a mechanism that allows browser scripts on pages served from other domains (e.g. myothersite.com) to talk to your server (e.g. api.mysite.com).  Like [JSONP](https://en.wikipedia.org/wiki/JSONP), the goal of CORS is to circumvent the [same-origin policy](http://en.wikipedia.org/wiki/Same-origin_policy); allowing your Sails server to successfully respond to requests from client-side JavaScript code running on a page hosted from some other domain.  But unlike JSONP, it works with more than just GET requests.  And it allows you to whitelist particular origins (`staging.yoursite.com` or `yourothersite.net`) and prevent requests from others (`evil.com`).

Sails can be configured to allow cross-origin requests from a list of domains you specify, or from every domain.  This can be done on a per-route basis, or globally for every route in your app.

### Enabling CORS

For security reasons, CORS is disabled by default in Sails.  But enabling it is dead-simple.

To allow cross-origin requests from a whitelist of trusted domains to _any_ route in your app, simply enable `allRoutes` and provide an `origin` setting in [`config/cors.js`](http://sailsjs.com/docs/reference/configuration/sails-config-cors):

```javascript
allRoutes: true,
allowOrigins: ['http://example.com','https://api.example.com','http://blog.example.com:1337','https://foo.com:8888']
```

To allow cross-origin requests from _any_ domain to _any_ route in your app, use `allowOrigins: '*'`:

```javascript
allRoutes: true,
allowOrigins: '*',
allowCredentials: false
```

Note that when using `allowOrigins: '*'`, the `credentials` setting _must_ be `false`, meaning that requests containing cookies will be blocked.  This restriction exists to prevent third-party sites from being able to trick your logged-in users into making unauthorized requests to your app.  You can lift this restriction (at your own risk!) using the [`allowAnyOriginWithCredentialsUnsafe`](http://sailsjs.com/docs/reference/configuration/sails-config-security-cors) setting.


See [`sails.config.security.cors`](http://sailsjs.com/documentation/reference/configuration/sails-config-security-cors) for a comprehensive reference of all available options.


### Configuring CORS For individual routes
Besides the global CORS configuration in `config/security.js`, you can also configure these settings on a per-route basis in [`config/routes.js`](http://sailsjs.com/anatomy/config/routes-js).

If you set `allRoutes: true` in `config/cors.js`, but you want to exempt a specific route, set the `cors: false` in the route's target:

```javascript
'POST /signup': {
   action: 'user/signup',
   cors: false
}
```

To enable or override global CORS configuration for a particular route, provide `cors` as a dictionary:

```javascript
'GET /videos': {
   action: 'video/find',
   cors: {
     allowOrigins: ['http://example.com','https://api.example.com','http://blog.example.com:1337','https://foo.com:8888'],
     allowCredentials: false
   }
}
```

### Notes

> + CORS support is only relevant for HTTP requests.  Requests made via sockets are not subject to cross-origin restrictions.  To ensure that your app is secure via sockets, configure the [`onlyAllowOrigins`](http://sailsjs.com/documentation/reference/configuration/sails-config-sockets) setting (typically in [`config/env/production.js`](http://sailsjs.com/documentation/anatomy/config/env/production-js).
> + CORS is not supported in Internet Explorer 7.  Fortunately, it is supported in IE8 and up, as well as in all other modern browsers.
> + Read [more about CORS from MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
> + Read the [CORS spec](https://www.w3.org/TR/cors/)

<docmeta name="displayName" value="CORS">
