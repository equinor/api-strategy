# API Strategy - Naming conventions and guidelines

This document is split into 2 parts:

* [APIM / API Naming Standards](#APIM-and-API-Naming-Standards)
* [General Guidelines](#General-Guidelines)

## APIM and API Naming Standards (draft)

To ensure consistency, naming of API’s should be aligned with the overall
Equinor Data Architecture guidelines and thus make use of 
[Data Areas](https://eita.equinor.com/companyea/) as a basis for naming. 
The use of organisational units, project names and obscure abbreviations is 
strongly discouraged. 

The following conventions are used below: 

* [] Indicates an optional argument
* | Indicates one of several options 
* L1, L2, L3 refer to the names of the different levels within the EITA Data 
  Areas list.  
* For data area names you can drop ‘Data’ and other stop words like (and,or, ..)
  from such naming if included e.g. Well Data -> Well

### API Naming standards (applies to APIM and API) 

#### API name:  Unique identity for each API  

The API name should be short and descriptive. It will be used in referring the 
API in APIM and API documentation 

Naming should be in the form below, use CamelCasing and with levels separated by 
'-'.

```
[L1-L2-L3] 
```

e.g. `OperationMaintenance-SafeWork-WorkPermit`

If the API is very project specific, needs further grouping, or contains only a 
subset of the functionality within a specific data area then naming might be: 

```
[L1-L2-L3] Custom Name | Function 
```

#### Endpoints (hierarchy) 

The endpoint is what is visible in the url to those consuming the service. 

Naming should be in the form below, in lowercase and with levels separated by 
'-'.

```
L1/L2/L3 
```
e.g. `operation-maintenance/safe-work/work-permit`

As a fallback if the API is very project specific, needs further grouping, or 
contains only a subset of the functionality within a specific data area naming 
might be: 

```
[L1/L2/L3] Custom Name | Function 
```

e.g. `operation-maintenance/safe-work/work-permit/organization-data`
 
#### Operations - Operation is specific to Function  

There are two types of operations: collection operations and item operations. 

* *Collection* operations act on a collection of resources 
* *Item* operations act on an individual resource.  

Both collection and item operations should use plural in naming    

e.g. `work-permits`

e.g. (plural): `https://api.example.com/operation-maintenance/safe-work/work-permits`

e.g. (Singular): `https://api.example.com/operation-maintenance/safe-work/work-permits/id`

#### Data Model 

The data model used for interacting with the service should where possible 
adopt the following guidelines.

* Use standard data structures.
* Use standard data formats / models.
* Use English words for markup.

### APIM Specific 

#### Display name 

The APIM display name is used to discover APIs using APIM developer portal.   

The display name should follow the same standard as defined for API naming above
however rather than using CamelCase, spaces can be used to separate words and 
‘|’ to separate data areas: 

e.g. `Operation and Maintenance | Safe Work | Work Permit`

#### Products 

APIM products are used for grouping of API’s on the bases of visibility and 
access control. A single API can be contained within multiple products. 

e.g.

`L1/ L2/ Custom Name - Internal`<br />
`L1/ L2/ Custom Name - Open`<br />
`L1/ L2/ Custom Name - Restricted`

A product description must be provided that indicates the target group of the 
product and describes any abbreviations used.  

#### Tags  

APIM Tags are used to allow for grouping and discovery of API’s by custom tags 

Multiple tags should be assigned to API's for each of the different data areas
including:  

* L1
* L2
* L3
* Project Name
* Function, Custom 

### Other Recommendations 

* Use forward slash (/) to indicate a hierarchical relationship 
* Do not use trailing forward slash (/) in URIs 
* Do not use file extensions 

## General Guidelines

These general guidlines and conventions are exactly that: guidelines, not rules.

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
