+++
title = "Advent of Code 2023"
description = "Master post of Advent of Code for 2023"
authors = ["Brandon Phillips"]
template = "blog_post.html"
date = "2023-12-02"
draft = false
[taxonomies]
tags = ["aoc", "python"]
+++

This is my Advent of Code master post for 2023! I know it's already day 2, but I just had the idea so I'm doing it now. What's the purpouse of this master post? What *is* a "master post?" Well, basically, I'll be placing my solutions for each AOC puzzle here, and updating the same blog post as I go. I'll walk through the steps I took to arrive at the solution, and some things I may have learned along the way. Hopefully, it'll turn out useful for someone somewhere.
<!-- more -->

## Day 1
### Part One
Part one was, for the most part, pretty straight forward. Find all the ditigs and put the first and last digits together to make a number, then add all of those numbers.

First, I extracted the numbers using the `regex` package, and returned them in a list.
```python
def get_numbers(line):
    return regex.findall(r"\d", line)
```

Easy peasy. Just use the `\d` regex pattern to match any digit. Next, we needed to combine the first and last digits into a single digit. This was a simple one-liner that indexed the first and last elements in the list of found digits and added them together as strings, then cast them back to `int`.
```python
def combine_digits(digits):
    return int(str(digits[0] + digits[-1]))
```

Getting the solution was a simple `for` loop over all the lines, running them through both of the previous functions to get what I needed, then adding the resulting numbers to another list, which was passed into `sum()`.
```python
with open("input.txt") as input:
    digits = []
    for line in input:
        digits.append(combine_digits(get_numbers(line)))

    print(sum(digits))
```

Part one, completed.

### Part Two
The second part was a bit more complicated. Now, in addition to extracting the digits in order, we also needed to extract the numbers that were *spelled out* in the string. Ultimately, I landed on using regex again, however there was a consideration for *overlapping* number words, in additon to the digits. Luckily, the `regex` library had another function to use with an argument that considered overlapping results.
```python
def get_numbers(line):
    matches = list(re.finditer(r"[0-9?]|(one|two|three|four|five|six|seven|eight|nine)", line, overlapped=True))
    return [str(matches[0].group(0)), str(matches[-1].group(0))]
```

Our `get_numbers()` function changed a bit. Firstly, we're using the [`finditer()`](https://docs.python.org/3/library/re.html#re.finditer) method instead of `findall()`, which comes with a neat little flag that will also return overlapping results. There were many instances of stuff like `oneight` or `twone` in the input strings that were hard to catch and kind of a subtle challenge. The regex itself now includes `[0-9?]` to find each single digit, or (`|`) one of the nine word numbers. The return now has to call the `group(0)` index of the `Match` objects found and returned by the method, which are the string literals of what we're looking for.

Next, I made a quick and dirty substitution from the word numbers to their digits using a dict lookup:
```python
def sub_string(str):
    num_dict = {
        'one': '1',
        'two': '2',
        'three': '3',
        'four': '4',
        'five': '5',
        'six': '6',
        'seven': '7',
        'eight': '8',
        'nine': '9',
        'zero': '0',
    }

    return num_dict[str]
```

Then, we pass the found pairs into a function to determine what are the words, what are the digits, and use the same string-adding-then-cast-to-int trick we did earlier.
```python
def to_int(pair):
    ints = []
    for num in pair:
        if len(num) > 1:
            ints.append(sub_string(num))
        else:
            ints.append(num)

    return int(str(ints[0] + ints[-1]))
```

Now we loop over it all, same as before, and profit.
```python
with open("input.txt") as input:
    results = []
    for line in input:
        results.append(to_int(get_numbers(line)))

    print(sum(results))
```
