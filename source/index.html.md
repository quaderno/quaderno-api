---
title: Quaderno API Reference

language_tabs:
  - shell
  - ruby
  - php
  - swift
  - python

toc_footers:
  - <a href='http://quaderno.io'>Sign Up for an Account</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Quaderno API! You can use our API to access all our features, designed around the idea of making tax management and invoicing easy for small businesses.

The Quaderno API is based on the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architectural style, and comprised of resources with predictable, human-readable and resource-oriented URLs. We make use of standard HTTP features (HTTP Authentication, Verbs, and Response Codes), understandable by any off-the-shelf HTTP client. We return [JSON](http://www.json.org/) for everything (including errors!), and expect to receive JSON for all `POST` and `PUT` requests.

We have language bindings for the Shell, [Ruby](https://github.com/quaderno/quaderno-ruby), [PHP](https://github.com/quaderno/quaderno-php), [Swift](https://github.com/quaderno/quaderno-swift) and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top-right.

If you have any suggestions, tips, or questions that you feel aren't answered here or in our [Knowledge Base](http://support.quaderno.io), please [get in touch](mailto:hello@quaderno.io) and let us know!

# Available API Libraries

We have several pre-built libraries for interacting with the Quaderno API. If one does not exist in your chosen language, feel free to create one and reach out to let us know. We'd be happy to help!

## Ruby

> Available as a gem. To install:

```shell
sudo gem install quaderno
```

> If you use Bundler you can use this line in your gemfile:

```
gem 'quaderno', :require => 'quaderno-ruby'
```

Source is available on [Github](https://github.com/quaderno/quaderno-ruby).

## PHP

> Simply add "quaderno/quaderno" to your `composer.json` file:

```json
{
  "require": {
    "quaderno/quaderno": "1.*"
  }
}
```

The PHP library is installed via [Composer](https://getcomposer.org/). 

Check out the source on [Github](https://github.com/quaderno/quaderno-php).

## Swift

> Add Quaderno to your Podfile if you are using CocoaPods:

```
pod "Quaderno", "~> 0.0.1"
```

> Otherwise, just drag the `.swift` source files into the `Source` directory in your project.

[Eliezer Talón](https://github.com/elitalon) was kind enough to create a Swift library for the Quaderno API. Thanks Eliezer!

You can find the source on [Github](https://github.com/quaderno/quaderno-swift).

<aside class="notice">
Embedded frameworks require a minimum of **iOS 8**. To use Quaderno with application targets that do not support embedded frameworks, such as iOS 7, you must include all source files directly in your project.
</aside>

# Endpoint URL Structure

> A basic GET looks like this:

```shell
curl -u API-KEY:x 
     -X GET 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

All URLs start with the root `https://ACCOUNT-NAME.quadernoapp.com/api/v1/`. 

- `ACCOUNT-NAME` is different for each user, and is used as the identifier for which user to affect with an API call. 
- Note that the API version is also included in the root of every call. This ensures that updates to the API will not break older code, but when you are ready to make the switch (and when we have a new version) you can do so by updating this root.

The resource in question will follow, ending in `.json` to signify that it returns and accepts JSON, like so:

`https://ACCOUNT-NAME.quadernoapp.com/api/v1/RESOURCE.json`

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

