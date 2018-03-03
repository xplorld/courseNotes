# Final Guide to Python Encoding stuff

## TL;DR

When source encoding is unknown, use `utf-8` encoding, with `error='surrogateescape'`

When source encoding is known, use it.

## background

encodings, string vs bytes

ascii, latin-1 and utf-8

## what's in a python3 `str`?

## interacting with the world (outside of python)

converting string <-> bytes

assumptions on `bytes` encodings


| actual encoding in bytes | `sys.getfilesystemencoding()` | error handling options | you get in python | comment |
|-|-|-|-|-|
| `ascii` | whatever | by default `strict` | great `str` | (almost) all encodings are compatible with `ascii`
| `utf-8` | `utf-8` | by default `strict` | great `str` | if actual encoding == environment, life is better |
|`latin-1` | `utf-8` | by default `strict` | `raise ValueError` | `utf-8` and `latin-1` conflicts! |
|`latin-1` | `utf-8` | `ignore` | conflicting characters -> nothing (filtered out) | causes data loss |
|`latin-1` | `utf-8` | `replace` | conflicting characters -> char `?` | causes data loss |
|`latin-1` | `utf-8` | `surrogateescape` | conflicting characters -> non-`UTF-8` codepoints | can convert back to `bytes` losslessly |
|`latin-1` | `utf-8` | your own | behaves as you like | use `codecs.register_error` to add your own handler |



 
