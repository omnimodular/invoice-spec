# Invoice specification / schema

# Field description

Required fields:

- `version`
  - Fixed string "v2".
  
- `site`
  - Customer namespace
  
- `id`
  - The invoice id must be unique within the `site` namespace.
  
- `stage`
  - The invoice stage must be unique for the Inovoice the same `id
    Examples of stages are: `input`, `output`, and `final`.

Optional fields:

- `headers`
  - Array of `{ name, value }` objects.
- `rows`
  - array of array of `{ name, value }` objects.
- `items`
  - array of array of `{ name, value }` objects.
- `flow`
  - array of array of { name, value } objects.
- `attachments`
  - array of `{ name, value }` objects. the value field must be base64 encoded.
- `text`
  - A base64 encoded string of OCR parsed data; 
  - and is not expected to need a token -> attachment / page logic.

- `labels`
  - is an optional non-empty array of restricted strings.

- `metrics`
  - is an optional non-empty array of name, value objects;
  ```json
  {
    "name": "accuracy",
    "value": 0.85
  }
  ```

- `images`
  - is an optional non-empty array of name, value objects (holding files); 
  - the width of the images *should* be 900-1000 pixels.  The value
    field must be base64 encoded.
  ```json
  {
    "name": "Attachment X - page Y of Z.png",
    "value": "..."
  }
  ```

## Azure conformance

The site and id field must confirm to the following rules:

- Must start with a letter or number, and can only contain letters, numbers, and the dash (-) character.
- The first and last letters must be alphanumeric.
- All letters must be lowercase.
- The dash (-) character cannot be the first or last character. Consecutive dash characters are not permitted.
- Must be from <strike>3</strike> 1 through <strike>63</strike> 50 characters long.
- The name has been limited to give some headroom for pre- and post fixes, up to 13 characters.

Note that the field itself can be shorter than the Azure minimum of 3 characters so be sure to validate or prefix with a sufficiently long prefix. 
2 characters should be enough, e.g., `s-`.

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
^[a-z0-9](?:-?[a-z0-9]+)*$
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
