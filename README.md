[![Build
Status](https://travis-ci.org/isaiah/transit-erlang.svg)](https://travis-ci.org/isaiah/transit-erlang)

transit-erlang
==============
[transit-format](https://github.com/cognitect/transit-format) implementation in Erlang.

Test and developed on Erlang/OTP R17.

Usage
-----

```shell
rebar get-deps compile
erl -pa ebin deps/*/ebin
```

```erlang
A = transit:write(#{<<"a">> => <<"b">>, 3 => 4}, [{format, json}]).
%% => <<"[\"^ \",\"a\",\"b\",3,4]">>
transit:read(A, [{format, json}]).
%% => #{<<"a">> => <<"b">>, 3 => 4}

%%% JSON Verbose mode
transit:write(#{<<"a">> => <<"b">>, 3 => 4}, [{format, json_verbose}]).
%% => <<"{\"~i3\":4,\"a\":\"b\"}">>

%%% msgpack
transit:write(#{<<"a">> => <<"b">>, 3 => 4}, [{format, msgpack}]).
%% => <<149,162,94,32,161,97,161,98,163,126,105,51,4>>
```

Benchmarks
--------------------

These benchmarks are run on a Lenovo Thinkpad W540 with a 16 Gigabyte RAM configuration and the following CPU core:

	Intel(R) Core(TM) i7-4900MQ CPU @ 2.80GHz

Timings run 300 rounds of encoding of the file `transit-format/examples/0.8/example.json` and then we divide down to get the
encoder time for each round. This then forms the base benchmark.

| Commit | Test | Format | Timing ms |
| ------ | ---- | ------ | --------- |
| 3d3b04ee6aad | JSON | Read | 9.976 |
| 3d3b04ee6aad | JSON | Write | 20.810 |
| 3d3b04ee6aad | JSON | ISO | 31.987 |
| 3d3b04ee6aad | MsgPack | Read | 4.901 |
| 3d3b04ee6aad | MsgPack | Write | 12.072 |
| 3d3b04ee6aad | MsgPack | ISO | 15.911 |
| 3d3b04ee6aad | JSON_Verbose | Read | 9.724 |
| 3d3b04ee6aad | JSON_Verbose | Write | 25.638 |
| 3d3b04ee6aad | JSON_Verbose | ISO | 34.236 |


Current limitations
--------------------

* We can't generate a keyword 'true' due to the current mapping of atoms into keywords.

Default type mapping
--------------------

| Transit type | Write accepts             | Read returns              |
| ------------ | -------------             | ------------              |
| null         | undefined                 | undefined                 |
| string       | binary()                  | binary()                  |
| boolean      | true, false               | true, false               |
| integer      | integer()                 | integer()                 |
| decimal      | float()                   | float()                   |
| keyword      | atom()                    | atom()                    |
| symbol       | transit\_types:symbol()   | transit\_types:symbol()   |
| big decimal  | float()                   | float()                   |
| big integer  | integer()                 | integer()                 |
| time         | transit\_types:datetime() | transit\_types:datetime() |
| uri          | transit\_types.URI        | transit\_types.URI        |
| uuid         | uuid.UUID                 | uuid.UUID                 |
| array        | list                      | list                      |
| list         | transit\_types:list()     | transit\_types:list()     |
| set          | sets, gb\_sets, ordsets   | sets                      |
| map          | map                       | map                       |
