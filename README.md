serrano
=========

[![gem version](https://img.shields.io/gem/v/serrano.svg)](https://rubygems.org/gems/serrano)
[![Build Status](https://api.travis-ci.org/sckott/serrano.png)](https://travis-ci.org/sckott/serrano)
[![codecov.io](http://codecov.io/github/sckott/serrano/coverage.svg?branch=master)](http://codecov.io/github/sckott/serrano?branch=master)

`serrano` is a low level client for Crossref APIs

Docs: http://www.rubydoc.info/gems/serrano

Other Crossref API clients:

- Python: [habanero](https://github.com/sckott/habanero)
- R: [rcrossref](https://github.com/ropensci/rcrossref)

## Changes

For changes see the [Changelog](CHANGELOG.md)

## API

Methods in relation to [Crossref search API][crapi] routes

* `/works` - `Serrano.works()`
* `/members` - `Serrano.members()`
* `/prefixes` - `Serrano.prefixes()`
* `/funders` - `Serrano.funders()`
* `/journals` - `Serrano.journals()`
* `/licenses` - `Serrano.licenses()`
* `/types` - `Serrano.types()`

Additional methods built on top of the Crossref search API:

* DOI minting agency - `Serrano.registration_agency()`
* Get random DOIs - `Serrano.random_dois()`

Other methods:

* [Conent negotiation][cn] - `Serrano.content_negotiation()`
* [Text and data mining][tdm] - `Serrano.text()`
* [Citation count][ccount] - `Serrano.citation_count()`
* [get CSL styles][csl] -  `Serrano.csl_styles()`

## Install

### Release version

```
gem install serrano
```

### Development version

```
git clone git@github.com:sckott/serrano.git
cd serrano
rake install
```

## Setup

Crossref's API will likely be used by others in the future, allowing the base URL to be swapped out. You can swap out the base URL by passing named options in a block to `Serrano.configuration`.

This will also be the way to set up other user options, as needed down the road.

```ruby
Serrano.configuration do |config|
  config.base_url = "http://api.crossref.org"
end
```

## Examples

Search works by DOI

```ruby
require 'serrano'
Serrano.works(doi: '10.1371/journal.pone.0033693')
```

Search works by query string

```ruby
Serrano.works(query: "ecology")
```

Get links

```ruby
res = Serrano.works(filter: {has_full_text: true})
# entire links metadata
res.links
# just links URLs
res.links(true)
# just xml links, if present
res.links_xml(true)
# just pdf links, if present
res.links_pdf(true)
```

Search journals by publisher name

```ruby
Serrano.journals(query: "peerj")
```

Search funding information by DOI

```ruby
Serrano.funders(ids: ['10.13039/100000001','10.13039/100000015'])
```

Get agency for a set of DOIs

```ruby
Serrano.registration_agency(ids: ['10.1007/12080.1874-1746','10.1007/10452.1573-5125'])
```

Get random set of DOIs

```ruby
Serrano.random_dois(sample: 100)
```

Content negotiation

```ruby
Serrano.cn(ids: '10.1126/science.169.3946.635', format: "citeproc-json")
```

Text mining

```ruby
res = Serrano.text(url: 'http://...');
```

## Meta

* Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.
* License: MIT

[crapi]: https://github.com/CrossRef/rest-api-doc/blob/master/rest_api.md
[cn]: http://www.crosscite.org/cn/
[tdm]: http://www.crossref.org/tdm/
[ccount]: http://labs.crossref.org/openurl/
[csl]: https://github.com/citation-style-language/styles
