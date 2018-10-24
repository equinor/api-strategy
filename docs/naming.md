# API Strategy - Naming conventions and guidelines

## Context

These guidlines and conventions are exactly that: guidelines, not rules.

## Guidelines

1. Prefer [simple, concise, and intuitive](#simple,-concise,-and-intuitive)
   names
2. Prefer [lower-case, hyphen-separated](#lower-case,-hyphen-separated) names
3. Prefer [single-word](#single-word-names) names
4. Prefer [7-bit ascii](#7-bit-ASCII)

## Simple, concise, and intuitive

Simple and concise names are easy to remember, easy to talk about, and aid
communication. If possible, avoid non-standard abbreviations and avoid
overloading names - prefer different names for different concepts.

Some name reuse is unavoidable, in which case there should be sufficienct
_context_ available for the concept itself to become unambiguous. Consider a
service that simply returns the GPS coordinates for various well properties
(wellhead, slot etc):

```
https://api.equinor.com/well-location/coordinates
```

In this context, _well_ means a hole in the seabed, key points along the well
path, and contact point with the platform). Now consider a service that queries
accumulated historical production of different hydrocarbons, in this case for
well 15/9-F-12 (Volve):

```
https://api.equinor.com/annual-historical-production/well/15/9-F-12
```

In this context, _well_ refers to a produced volume.

In both cases, _well_ is a reasonable name for a resource, because sufficient
context is provided.

When deciding on a name, look to familiar terminology, and consider both
established terms and concepts in computer programming, in addition to the
domain.

## Lower-case, hyphen-separated

URLs are [case sensitive](#urls-are-case-sensitive), except for the host
component, but in practice a lot of systems are still case insensitive (like
IIS and the Windows file system).

To help reduce friction, prefer lower-case hyphen-separated names, both in URLs
and possibly fields. Consider a function _get wells matching_; assuming case
insensitivity and the option of camelcase, pascalcase, and hyphenation:

```
https://api.equinor.com/wells/wells-matching
https://api.equinor.com/wells/WellsMatching
https://api.equinor.com/wells/wellsmatching
```

The latter could be the same function, and they could not be - it depends on
client software, and sets up for miscommunication. Additionally, it's slightly
harder to make out different words since there is no horizontal space at all.

The hyphen (\-) is preferred over the underscore (\_), since the underscore in
some fonts and renderers is difficult to separate from the whitespace, and
leaves no room for doubt. The hyphen is well suited as a word separator,
because it does not have to be [escaped](#RFC1738) in an URL.

## Single-word names

## 7-bit ASCII

While in particular useful for identifiers, prefer pure 7-bit ASCII whenever
possible, i.e. try and avoid language-specific letters whenever you can. While
all clients and servers should be UTF-8 (or in general encoding) aware and
robust, a lot of potential friction is reduced when sticking to 7-bit ASCII.

## References

### URLs are case-sensitive

[RFC 3986](https://tools.ietf.org/html/rfc3986)

> the scheme and host are case-insensitive and therefore should be normalized
> to lowercase. For example, the URI <HTTP://www.EXAMPLE.com/> is equivalent to
> <http://www.example.com/>. The other generic syntax components are assumed to
> be case-sensitive unless specifically defined otherwise by the scheme

[RFC 2616](https://tools.ietf.org/html/rfc2616)

> When comparing two URIs to decide if they match or not, a client SHOULD use a
> case-sensitive octet-by-octet comparison of the entire URIs, with these
> exceptions:

[RFC 7230](https://tools.ietf.org/html/rfc7230)

> The scheme and host are case-insensitive and normally provided in lowercase;
> all other components are compared in a case-sensitive manner.

### RFC1738

[RFC 1738](http://www.ietf.org/rfc/rfc1738.txt)

> Scheme names consist of a sequence of characters. The lower case letters
> "a"--"z", digits, and the characters plus ("+"), period ("."), and hyphen
> ("-") are allowed. For resiliency, programs interpreting URLs should treat
> upper case letters as equivalent to lower case in scheme names (e.g., allow
> "HTTP" as well as "http").
