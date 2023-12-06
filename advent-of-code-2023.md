+++
title = "Advent of Code 2023"
description = "Master post of Advent of Code for 2023"
authors = ["Brandon Phillips"]
template = "blog_post_toc.html"
date = "2023-12-02"
updated = "2023-12-03"
draft = false
[taxonomies]
tags = ["aoc", "python"]
+++

This is my Advent of Code master post for 2023! I know it's already day 2, but I just had the idea so I'm doing it now. What's the purpouse of this master post? What *is* a "master post?" Well, basically, I'll be placing my solutions for each AOC puzzle here, and updating the same blog post as I go. I'll walk through the steps I took to arrive at the solution, and some things I may have learned along the way. Hopefully, it'll turn out useful for someone somewhere.
<!-- more -->

---
## Day 1
---
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

---
## Day 2
---
### Part One

This one had my brain rolling at first, but then after understanding how the data needed to be structured, it was fairly simple. First, we pulled the results of the game from each line in the input file.
```python
def get_results(line):
    return line.split(": ")[1].split("; ")
```

This function splits the line at the single colon, then returns a list from another split on the `;`, which represents each result of the game.

Next, we examine each of the results and run it through a function that uses the `match` keyword to test each color's result against a pre-defined constant, and returs if the result is possible or not.
```python
RED = 12
GREEN = 13
BLUE = 14

def checkif_possible(pair):
    is_possible = True
    quant = int(pair[0])

    match pair[1].rstrip():
        case "red":
            if quant > RED:
                is_possible = False
        case "green":
            if quant > GREEN:
                is_possible = False
        case "blue":
            if quant > BLUE:
                is_possible = False
        case _:
            raise ValueError("Not a color")

    return is_possible
```

The default `case` statement here actually exposed a subtle problem in the input data; some of the strings contained `\n` characters in them which caused the `match` to default, which raised the `ValueError()`. Because of this, I added the `rstrip()` method on the `match` input to get rid of these, so we could match on everything.

Now, we build the main loop, and a quick little function to extract the Game ID number, then `sum()` all the IDs we pull from the loop.
```python
def get_id(game):
    return int(game.split(": ")[0].split(" ")[1])

with open("input.txt") as input:
    all_games = []
    for line in input:
        results = get_results(line)
        game_is_possible = True
        for result in results:
            for pair in result.split(", "):
                if not checkif_possible(pair.split(" ")):
                    game_is_possible = False
                    break

        if game_is_possible:
            all_games.append(get_id(line))

    print(sum(all_games))
```

### Part Two
This one was trickier, but my issue with it actually came down to a misunderstanding of how Python uses the `+=` opperator on strings, and my mistake of not casting the types for the digits right away.

So the first things I did was extract each of the results of each line into a dict with the keys set to the colors, and the values set to all the results of that color.
```python
def order_results(results):
    ordered_results = {
        "red": [],
        "blue": [],
        "green": []
    }

    for result in results:
        for pair in result.split(", "):
            ordered_results[pair.split(" ")[1].rstrip()].append(int(pair.split(" ")[0]))

    return ordered_results
```

Now that we've organized all of the results for each color, we need to find the largest result of each color, for each game, and multiply those results together, and throw that result into a list.
```python
from functools import reduce

def find_largest(results):
    return int(max(results))

def get_power(results):
    return reduce(lambda x, y: x*y, results)

with open("input.txt") as input:
    all_power = []
    for line in input:
        current_results = []
        for color, results in order_results(line.split(": ")[1].split("; ")).items():
            current_results.append(find_largest(results))

        all_power.append(get_power(current_results))

    print(sum(all_power))
```

At the end there, we take the list of power results we've built and add them all together, producing our result. Voila, the answer.

---
## Day 3
---
### Part One
This one really threw me for a loop. I only realized at the tail end of my head-banging that this is effectively navigating and operating on a matrix; had I thought of it this way *earlier*, I would have looked at [NumPy's Matrix](https://numpy.org/doc/stable/reference/generated/numpy.matrix.html) class, and probably would have had an easier time.

Firstly, I wanted to represent the numbers we're concerned with by using a `Number` class.
```python
class Number:
    position: (int, int)
    num: int
    line: str
    is_part: bool
    match: re.Match

    def __init__(self, x, y, num, line, match):
        self.position = (x, y)
        self.num = num
        self.line = line
        self.is_part = False
        self.match = match

    def get_len(self):
        return int(len(self.num))

    def get_position(self):
        return self.position

    def get_x(self):
        return self.position[0]

    def get_y(self):
        return self.position[1]

    def is_start(self):
        if self.position[0] == 0:
            return True
        else:
            return False

    def is_end(self):
        if self.match.end() == len(self.line):
            return True
        else:
            return False
```

When parsing the input (using regex again, I'll get to that), we'll be building this class using any number we find. The class will contain the information regarding the number, where it's at in the matrix, what the number is, what its length is, and the actual `Match` object returned from the regex method. 

We'll use this method to find the numbers:
```python
def find_numbers(line, index):
    regex = re.compile(r"\d+")
    numbers = []
    for i in regex.finditer(line):
        if i is not None:
            numbers.append(Number(i.start(), index, i.group(), line, i))

    return numbers
```

After we've found a number, we add it to a list:
```python
with open("input.txt") as input:
    lines = defaultdict(str)
    index = 0
    all_numbers = []

    for line in input:
        lines[index] += line.rstrip()
        index += 1

    for index, line in lines.items():
        all_numbers += find_numbers(line, index)
```

Once we have our list, we'll run through it and check all the adjacent indices for any non-`\d` and non-`\.` characters, and change the `is_part` attribute of our class to `True`. We also need to account for *where* the number exists on the line in the matrix, because our checks will be slightly different for `[0]` and `[-1]` indexed numbers.

```python
def check_adjacent(num, lines):
    range = ()
    if num.get_x() == 0:
        range = (num.get_x(), num.get_x() + len(num.num) + 1)
    elif num.is_end():
        range = (num.get_x() - 1, num.get_x() + len(num.num))
    else:
        range = (num.get_x() - 1, num.get_x() + len(num.num) + 1)

    regex = re.compile(r"[^\d\.]")
    for line in lines:
        results = regex.finditer(line, pos=range[0], endpos=range[1])
        for result in results:
            if result is not None:
                num.is_part = True
```

Fetching the adjacent lines to check is similar, so we check the `index` attribute of the class and compare it to our matrix's `y` axis (which is a dict, with each key being the line number, and each value being that line, so key=y, and value=x), to check for the first and last lines, so we don't get an indexing error.

After all the collection, math, and headaches, we then add up the numbers who's `is_part` is true by pushing them to a list, and running the `sum(parts)` method, pooping out the answer!
```python
with open("input.txt") as input:
    lines = defaultdict(str)
    index = 0
    all_numbers = []
    parts = []

    for line in input:
        lines[index] += line.rstrip()
        index += 1

    for index, line in lines.items():
        all_numbers += find_numbers(line, index)

    for num in all_numbers:
        adjacent_lines = []
        adjacent_lines.append(num.line)

        if num.get_y() == 0:
            adjacent_lines.append(lines[num.get_y() + 1])
        elif num.get_y() == len(lines) - 1:
            adjacent_lines.append(lines[num.get_y() - 1])
        else:
            adjacent_lines.append(lines[num.get_y() - 1])
            adjacent_lines.append(lines[num.get_y() + 1])

        check_adjacent(num, adjacent_lines)

        if num.is_part:
            parts.append(int(num.num))

    print(sum(parts), parts)
```

Here's the final result:
```python
import re
from collections import defaultdict


class Number:
    position: (int, int)
    num: int
    line: str
    is_part: bool
    match: re.Match

    def __init__(self, x, y, num, line, match):
        self.position = (x, y)
        self.num = num
        self.line = line
        self.is_part = False
        self.match = match

    def get_len(self):
        return int(len(self.num))

    def get_position(self):
        return self.position

    def get_x(self):
        return self.position[0]

    def get_y(self):
        return self.position[1]

    def is_start(self):
        if self.position[0] == 0:
            return True
        else:
            return False

    def is_end(self):
        if self.match.end() == len(self.line):
            return True
        else:
            return False


def check_adjacent(num, lines):
    range = ()
    if num.get_x() == 0:
        range = (num.get_x(), num.get_x() + len(num.num) + 1)
    elif num.is_end():
        range = (num.get_x() - 1, num.get_x() + len(num.num))
    else:
        range = (num.get_x() - 1, num.get_x() + len(num.num) + 1)

    regex = re.compile(r"[^\d\.]")
    for line in lines:
        results = regex.finditer(line, pos=range[0], endpos=range[1])
        for result in results:
            if result is not None:
                num.is_part = True


def find_numbers(line, index):
    regex = re.compile(r"\d+")
    numbers = []
    for i in regex.finditer(line):
        if i is not None:
            numbers.append(Number(i.start(), index, i.group(), line, i))

    return numbers


with open("input.txt") as input:
    lines = defaultdict(str)
    index = 0
    all_numbers = []
    parts = []

    for line in input:
        lines[index] += line.rstrip()
        index += 1

    for index, line in lines.items():
        all_numbers += find_numbers(line, index)

    for num in all_numbers:
        adjacent_lines = []
        adjacent_lines.append(num.line)

        if num.get_y() == 0:
            adjacent_lines.append(lines[num.get_y() + 1])
        elif num.get_y() == len(lines) - 1:
            adjacent_lines.append(lines[num.get_y() - 1])
        else:
            adjacent_lines.append(lines[num.get_y() - 1])
            adjacent_lines.append(lines[num.get_y() + 1])

        check_adjacent(num, adjacent_lines)

        if num.is_part:
            parts.append(int(num.num))

    print(sum(parts), parts)
```
