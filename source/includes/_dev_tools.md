# Dev tools

We provide a testing environment, and a few endpoints and libraries to help with the development experience with the Quaderno set of APIs and products.

## Testing environment

We provide a separate environment for testing purposes, with its own URL and credentials. We call it [Quaderno Sandbox](https://developers.quaderno.io/#quaderno-sandbox). Follow the link to learn about its special facilities to improve the development experience!

## Purging Quaderno Sandbox

```shell
curl --user sk_your-secret-key:x \
     --request POST \
     --url http://{{your-account-name}}.sandbox-quadernoapp.com/api/purge
```

Our testing environment, Quaderno Sandbox, has a global 200 documents cap. You can manually purge all data with:

`POST /purge`

## Pinging the API

```shell
curl --user sk_your-secret-key:x \
     --url https://quadernoapp.com/api/ping
```

You can `ping` Quaderno to check if the service is up, your credentials are correct, or to know your [remaining API requests](#rate-limiting) without doing an accountable request:

`GET /ping`


## Libraries

> To install our Ruby library, add the following to your Gemfile:

```ruby
  gem 'quaderno', require: 'quaderno-ruby'
```

You can use our API wrappers to connect and handle our APIs in a nicer way. We provide a Ruby library and a PHP library.

You may eventually find also a Swift library in our Github repos, but it doesn't support the [latest API additions](#20170628).

### Ruby Wrapper

> To configure our Ruby gem, just add this to your initializers:

```ruby
  Quaderno::Base.configure do |config|
    config.auth_token = 'my_authenticate_token'
    config.url = 'https://my_subdomain.quadernoapp.com/api/'
    config.api_version = API_VERSION # Optional, defaults to the API version set in your account
  end
```

Our Ruby library code is in [https://github.com/quaderno/quaderno-ruby](https://github.com/quaderno/quaderno-ruby).

Please follow the [README instructions](https://github.com/quaderno/quaderno-ruby#readme) to learn more.

### PHP Wrapper

> To setup the library:

```php
require_once 'quaderno_load.php';
QuadernoBase::init('YOUR_API_KEY', 'YOUR_API_URL', API_VERSION);
QuadernoBase::ping();   // optionally test the API connection
```

Our PHP library code is in [https://github.com/quaderno/quaderno-php](https://github.com/quaderno/quaderno-php).

We don't support PHP package managers at the moment, just copy the files into your project to install.

Please follow the [README instructions](https://github.com/quaderno/quaderno-php#readme) to learn more.
