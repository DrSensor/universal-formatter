# Universal Formatter

An **experimental** code/text formatter that utilize [Rosie Pattern Language](https://rosie-lang.org) 

> still just an idea

### Usage
Given this _json5.rpl_
```dhall
package json_fmt

import word, num

local string = word.q
local number = num.signed_number
local true = "true"
local false = "false"
local null = "null"

key = word.any / string
primitive_value = string / number / true / false / null

--- null_first ---
member = key ":" primitive_value ","?
$_scope = "{" member+ "}"
$_place_at_start = "{"

$1_place = key ":" null ","?
$1_before = key ":" (string / number / true / false) ","?
$2_place_before_end = $1_before

$_place_at_end = "}"
------------------

--- no_comma ---
$_scope = key ":" primitive ","
$_remove = ","
----------------
```
then execute
```console
$ echo '{"a":123,"b":null}' | ufmt --grammar json5.rpl --enable null_first,no_comma
{
  "b": null
  "a": 123
}
```
