# Nether Avenue

[![Code Climate](https://codeclimate.com/github/netherphp/avenue/badges/gpa.svg)](https://codeclimate.com/github/netherphp/avenue) [![Build Status](https://travis-ci.org/netherphp/avenue.svg?branch=redux)](https://travis-ci.org/netherphp/avenue)  [![Packagist](https://img.shields.io/packagist/v/netherphp/avenue.svg)](https://packagist.org/packages/netherphp/avenue) [![Packagist](https://img.shields.io/packagist/dt/netherphp/avenue.svg)](https://packagist.org/packages/netherphp/avenue)

A Request Router. Again. A simple [in my opinion] router. I've written about 10 of these now. But this one... this is the one. First you have to setup your webserver so that all requests get pumped to your www/index.php. This can be done multiple ways, and should be familiar to you if you have done web development before. Once you have everything piped to index.php, your file can be as simple as this...

	<?php $router = (new Nether\Avenue\Router)
	->AddRoute('{@}//index','Routes\Home::Index')
	->AddRoute('{@}//about','Routes\Home::About')
	->AddRoute('{@}//member,'Routes\MemberList::ViewList')
	->AddRoute('{@}//member/(#)','Routes\MemberList::ViewByID')
	->Run();

## Route Conditions
Route conditions are straight regular expressions at the end of the day, however I have provided several shortcuts to make writing the routes easier. You can also define your own shortcuts if you find yourself doing something frequently.

#### Shortcut Types

There are two types of shortcuts - slotted and unslotted. Slotted (similar to default preg) is surrounded by parens and the value within them will be passed to the route that will be executed. Unslotted is surrounded by braces and that data will not be passed.

* (@) - slotted - whatever we found will be passed to the routing method.
* {@} - unslotted - whatever we found will not be passed to the routing method.

#### Available Shortcuts

You can have several slotted or unslotted shortcuts in a route condition. All slotted shortcuts (and slotted regex) will be passed to the routing method in the order they were given.

There are several shortcuts for matching data we need to reference out of URLs often.

* @ = match anything, as long as there is something to match.
* ? = match anything, even if there is nothing to match.
* # = match a number (or series of)
* $ = match a string as a path fragment. anything between forward slashes, not including them.
* domain = match a string that is a domain name. it will match the full domain like "www.nether.io", however, it will only pass the relevant domain to the route e.g. without subdomains, in this case "nether.io".

Route shortcuts MUST be surrounded by either () or {}.

Additionally, you can go hardmode with straight on Perl Regex just like you were dumping it into preg_match(). Or you can mix and match straight regex with my shortcuts.

#### Example Route Conditions

	Matches for the homepage request on any domain.
	- {@}//index
	+ domain.tld/ => Route::Method();
	
	Matches the homepage request on any domain.
	Straight Perl instead of shortcuts.
	- .+?//index

	Matches for the homepage request on any domain.
	Passes the domain to the routing method.
	- (@)//index
	+ domain.tld/ => Route::Method($domain);

	Matches for the homepage on a beta domain.
	- beta.{@}//index
	+ beta.domain.tld/ => Route::Method();

	Matches for any page on any domain.
	Passes everything after the domain to the routing method as one long string argument.
	- {@}//(@)
	+ domain.tld/slender/man/needs/you => Route::Method($path);

	Matches for a two path part request.
	Passes both parts to the routing method as two separate string arguments.
	- {@}//($)/($)
	+ domain.tld/user/create => Route::Method($namespace,$action);

	Matches a members path with an integer.
	Passes the integer to the routing method.
	- {@}//members/(#)
	+ domain.tld/members/42 => Route::Method($id);

	Matches a members path with an integer.
	Straight Perl instead of shortcuts.
	Passes the integer to the routing method.
	- .+?//members/(\d+)
	+ domain.tld/members/42 => Route::Method($id);

## Installing
Require this package in your composer.json.

	require {
		"netherphp/avenue": "dev-redux"
	}

Then install it or update into it.

	$ composer install --no-dev
	$ composer update --no-dev


## Testing
This library uses Codeception for testing. Composer will handle it for you. Install or Update into it.

	$ composer install --dev
	$ composer update --dev

Then run the tests.

	$ php vendor/bin/codecept run unit
	$ vendor\bin\codecept run unit


