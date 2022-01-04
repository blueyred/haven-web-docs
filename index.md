---
layout: default
title: Introduction
nav_order: 1
description: "Developer Documentation for the Haven Protocol Web Vault."
permalink: /
---

# Haven Protocol Web App
This is a guide to help developers wanting to work with the Haven Protocol web eco-system to get started on new developments.

The web ecosystem spans a few different git repos;

![Ecosystem Diagram]({{ site.baseurl }}/assets/images/ecosystem.png)

- [haven-web-app](https://github.com/haven-protocol-org/haven-web-app) the ReactJS code

- [haven-web-core](https://github.com/haven-protocol-org/haven-web-core) exposes compiled WebAssembly to JS 

- [haven-web-cpp](https://github.com/haven-protocol-org/haven-web-cpp) a C++ wrapper to create a reusable API from the core C++ library

- [haven-main](https://github.com/haven-protocol-org/haven-main) the core Haven C++ library


Haven is a fork of [Monero](https://www.getmonero.org/) a best of breed privacy coin with the mint and burn of assets that track fiat and other currencies, allowing storage of assets without exposure to the volitality of cryptocurrenciey markets.
Currently Haven is build ontop of the Monero v0.16.



---
[Getting Started]({{ site.baseurl }}{% link docs/getting-started.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## About the project

[Haven Protocol](https://havenprotocol.org) is an untraceable cryptocurrency that proposes a new way of achieving a stable fiat value storage while being traded at market value.


### Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. Read more about becoming a contributor in [our GitHub repo](https://github.com/haven-protocol-org/tbc#contributing).

### Code of Conduct

HAven Protocol is committed to fostering a welcoming community.

[View our Code of Conduct](https://github.com/haven-protocol-org/tbc/CODE_OF_CONDUCT.md) on our GitHub repository.

### Theme License

This documentation uses Just the Docs &copy; 2017-{{ "now" | date: "%Y" }} by [Patrick Marsceill](http://patrickmarsceill.com) distributed by an [MIT license](https://github.com/pmarsceill/just-the-docs/tree/master/LICENSE.txt).

#### Thank you to all the theme contributors

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li class="d-inline-block mr-1">
     <a href="{{ contributor.html_url }}"><img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}"/></a>
  </li>
{% endfor %}
</ul>