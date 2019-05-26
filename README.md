# string-length-the-hard-way
What if you wanted to get the length of a string in Lua.. without using the `string.len` function or the length (`#`) operator? And make it run in either the standard Lua interpeter or in PICO-8?

Don't ask me why I tried to do this...

```lua
-- PICO-8 and Standard Lua lib compatibility
local _sub = string and string.sub or sub
local _flr = math and math.floor or flr

function string_length(str)
  -- we have undefined behavior if the
  -- data size is larger than this
  -- (max number in PICO-8)
  local search_max = 32767
  local search_min = 1
  -- binary search for index before empty string
  while true do
    local i = search_min + _flr((search_max + 1 - search_min) / 2)
    local char = _sub(str, i, i)
    if char == "" then
      search_max = i - 1
    else
      local after = i + 1
      local char_after = _sub(str, after, after)
      if char_after == "" then
        return i
      end
      search_min = after
    end
  end
end
```

And it works!

```lua
string_length("hello world") -- 11
```
