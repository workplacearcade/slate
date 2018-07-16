---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - newsfeed
  - messaging
  - socket
  - profile

search: true
---

# Introduction

This documentation details the endpoints that will be exposed as part of the Workplace Arcade < > RTPos integration. For stage one this includes:

* Newsfeed
* Chat
* Profile
* User Creation

# Authentication

Workplace Arcade currently uses JWT tokens to authorize individual users. This token is attached to the header of requests requring authentication in the form of:

`X-USER-TOKEN: <token>`

We are going to be be building bespoke logic for the integration with RTPos so will look to them to dictate the authentication method.

