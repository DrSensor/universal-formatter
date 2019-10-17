# Universal Formatter

An **experimental** code/text formatter that utilize [Rosie Pattern Language](https://rosie-lang.org) 

> still just an idea

### Usage
Given this _json5.rpl_ and _json5.yml_
```dhall
package json5

import word, num

local true = "true"
local false = "false"
local null = "null"

string = word.q
number = num.signed_number

key = word.any / string
primitive_value = string / number / true / false / null
member = key ":" primitive_value ","?
member_non_null = key ":" (string / number / true / false) ","?
```
```yml
version: '1'
grammar: [json5.rpl]
rules:
  no_comma:
    scope: json5.key ":" json5.primitive ","
    remove: ","
  null_first:
    scope: "{" json5.memeber "}"
    match_and_place:
      at_start: "{"
      at_end: "}"
    patterns:
    - place: json5.key ":" json5.null ","?
      before: json5.member_non_null
    - match_and_place:
        before_end: json5.member_non_null
```
then execute
```console
$ echo '{"a":123,"b":null}' | ufmt --grammar json5.yml --enable null_first,no_comma
{
  "b": null
  "a": 123
}
```
