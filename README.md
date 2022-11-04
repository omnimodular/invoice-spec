# Invoice specification / schema

## Azure conformance

The site and id field must confirm to the following rules:

Must start with a letter or number, and can only contain letters, numbers, and the dash (-) character.
The first and last letters must be alphanumeric.
All letters must be lowercase.
The dash (-) character cannot be the first or last character. Consecutive dash characters are not permitted.
Must be from 3 1 through 63 50 characters long.
The name has been limited to give some headroom for pre- and post fixes, up to 13 characters.

Note that the field itself can be shorter than the Azure minimum of 3 characters so be sure to validate or prefix with a sufficiently long prefix (2 characters should be enough, e.g., s-)

Reference: https://docs.microsoft.com/en-us/rest/api/storageservices/naming-queues-and-metadata#queue-names

## Validation

https://regex101.com/

https://extendsclass.com/json-schema-validator.html

Regexp:

```
^(?=.{1,50}$)[a-z0-9](?:-?[a-z0-9]+)*$
```

or simplified if length is enforced separately:

```
^(?=.{1,50}$)[a-z0-9](?:-?[a-z0-9]+)*$
```

Examples:

```
x
xy
foo
foobar
foo-bar
o-k
0actuallyok
cc80eb8a-35e0-11ed-90a9-38d547aac12a
00000000-35e0-11ed-90a9-000000000000
12345678901234567890123456789012345678901234567890
```

```
bad--bader
-bad
alsobad-
fooBar
123456789012345678901234567890123456789012345678901
000000000011111111112222222222333333333344444444455
```
