---
title: Quaderno API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href="https://quaderno.io" target="_blank">Quaderno Homepage</a>
  - <a href="https://support.quaderno.io" target="_blank">Knowledgebase and Support</a>
  - <a href="https://github.com/quaderno/quaderno-api" target="_blank">Contributing to API Docs</a>
  - <a href="https://github.com/tripit/slate" target="_blank">Documentation Powered by Slate</a><br /><br />

includes:
  - setup
  - changelog
  - contacts
  - products
  - events
  - tax_ids
  - tax_rates
  - tax_jurisdictions
  - tax_codes
  - evidence
  - taxes
  - invoices
  - credits
  - expenses
  - recurring
  - document_items
  - attachments
  - payments
  - checkout_sessions
  - coupons
  - accounts
  - transactions
  - reporting_requests
  - webhooks

search: true

code_clipboard: true
---

# Introduction

> BASE URL

```
https://ACCOUNT_NAME.quadernoapp.com/api/
``` 

Welcome to the Quaderno API! You can use our API to access all our features, designed around the idea of making tax management and invoicing easy for small businesses.

The Quaderno API is based on the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architectural style, and comprised of resources with predictable, human-readable and resource-oriented URLs. We make use of standard HTTP features (HTTP Authentication, Verbs, and Response Codes), understandable by any off-the-shelf HTTP client. We return [JSON](http://www.json.org/) for everything (including errors!), and expect to receive JSON for all `POST` and `PUT` requests.

If you have any suggestions, tips, or questions that you feel aren't answered here or in our [developer site](http://developers.quaderno.io), please [get in touch](mailto:support@quaderno.io) and let us know!




# The Sandbox

The Quaderno Sandbox mirrors the features found on the Quaderno Production servers and lets you test our API.

The Sandbox has parity with the Quaderno main feature set supported by the live environment. This means you can test your Quaderno processes and know they will behave the same on the production servers as they do in the Sandbox environment.

<aside class="notice">
  <strong>Your credentials for the Production environment don't work on the Sandbox.</strong> You need to <a href="https://sandbox-quadernoapp.com/signup" target="_blank">create a new account</a>.
</aside>

By using your Sandbox account, you can test and debug your application without referencing any real Quaderno users or their live Quaderno accounts. The Sandbox lets you operate your application in a safe environment and provides you a way to fine tune your Quaderno routines before moving your product into production.
