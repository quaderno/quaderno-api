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

# Quaderno API Features

> Use this Base URL for production:

```
https://{{ACCOUNT_NAME}}.quadernoapp.com/api
```

Welcome to the Quaderno API! You can use our API to access all our features, designed around the idea of making tax management and invoicing easy for small businesses.

The Quaderno API is based on the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) architectural style, and comprised of resources with predictable, human-readable and resource-oriented URLs. We make use of standard HTTP features (HTTP Authentication, Verbs, and Response Codes), understandable by any off-the-shelf HTTP client. We return [JSON](http://www.json.org/) for everything (including errors!), and expect to receive JSON for all `POST` and `PUT` requests.

> Use this Base URL for Quaderno Sandbox testing:

```
https://{{ACCOUNT_NAME}}.sandbox-quadernoapp.com/api
```

If you have any suggestions, tips, or questions that you feel aren't answered here or in our [developer site](http://developers.quaderno.io), please [get in touch](mailto:support@quaderno.io) and let us know!
