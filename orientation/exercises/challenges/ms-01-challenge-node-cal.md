# Node cal
If you type `cal` into your terminal, you will see an output of the current month in calendar form. Type `cal` and just a year, you'll see a whole year. Type `cal` plus a month and any year from 0 to 9999, and you'll see that month's calendar. It's pretty cool. Guess what? You can build a clone of `cal` right now.

All you need is a little help from Christian Zeller, the man behind [Zeller's congruence](https://en.wikipedia.org/wiki/Zeller's_congruence). Thanks to him, we can calculate the day of the week of the first day of any month of (almost) any year. As our gift to you, we have included a snippet that shows the formula, plus how to implement it in JavaScript. We accept tokens of gratitude in cash or food.

```
/***
* h = calculated day
* q = day
* m = month
* y = year
* note: calculations must be in integers - do not use floats or it will fail.
*
*      /    |(m+1)*26|       | y |       | y |   | y |    \
* h = | q + |--------| + y + |---| + 6 * |---| + |---| - 1 | mod 7
*      \    |   10   |       | 4 |       |100|   |400|    /
*
***/
function zeller(month, day, year) {
  if (month < 3) { month += 12; year -= 1; }
  var h = (day + parseInt(((month + 1) * 26) / 10) + year + parseInt(year / 4) + 6 * parseInt(year / 100) + parseInt(year / 400) - 1) % 7;
  return h;
}
```

## Requirements
The base requirement is to build a command line app using TDD, that prints any month, from 1754 to 9999 (apparently there's an issue with errors prior to 1754).

The primary focus of this project is developing a test suite and thinking modularly, not the app logic itself.

Make sure to cover as many edge cases as you can. Play with the real `cal` by passing in all kinds of numbers and strings to the month and year arguments. See what works and what breaks it. What kind of error messaging do you get back?

Make sure you compare the output of `cal` to your `node-cal` in a test. Are they exactly the same? hint: pay attention to spaces and linebreaks. Comparing the two will involve invoking them from the command line, which is best handled by spawning child processes. If that wasn't covered in class, ask your instructor for a quick overview, or check the [docs](https://nodejs.org/api/child_process.html#child_process_child_process). Here's a snippet to get you started on using child processes:

```
const { assert: { equal, deepEqual } } = require('chai')
const { exec } = require('child_process')

// child_process.exec(): spawns a shell and runs a command within that shell,
// passing the stdout and stderr to a callback function when complete.
describe('cli-stuff', () => {
  it('should handle no args cal comparison', (cb) => {
    exec('bin/node-cal', (err, stdout) => {
      exec('cal', (err2, stdout2) => {
        deepEqual(stdout, stdout2);
        cb();
      });
    });
  });
});
```

Expected:

```bash
$ ./node-cal.js # returns current month (Assuming current month is March 2017)
    March 2017
Su Mo Tu We Th Fr Sa
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31

```

```bash
$ ./node-cal.js 2 2017 # returns specific month
   February 2017
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28

```

## Bonus
+ Print an entire year

```bash
$ ./node-cal.js 2017 # returns specific year
                             2017

      January               February               March
Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa
 1  2  3  4  5  6  7            1  2  3  4            1  2  3  4
 8  9 10 11 12 13 14   5  6  7  8  9 10 11   5  6  7  8  9 10 11
15 16 17 18 19 20 21  12 13 14 15 16 17 18  12 13 14 15 16 17 18
22 23 24 25 26 27 28  19 20 21 22 23 24 25  19 20 21 22 23 24 25
29 30 31              26 27 28              26 27 28 29 30 31

       April                  May                   June
Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa
                   1      1  2  3  4  5  6               1  2  3
 2  3  4  5  6  7  8   7  8  9 10 11 12 13   4  5  6  7  8  9 10
 9 10 11 12 13 14 15  14 15 16 17 18 19 20  11 12 13 14 15 16 17
16 17 18 19 20 21 22  21 22 23 24 25 26 27  18 19 20 21 22 23 24
23 24 25 26 27 28 29  28 29 30 31           25 26 27 28 29 30
30                                          
        July                 August              September
Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa
                   1         1  2  3  4  5                  1  2
 2  3  4  5  6  7  8   6  7  8  9 10 11 12   3  4  5  6  7  8  9
 9 10 11 12 13 14 15  13 14 15 16 17 18 19  10 11 12 13 14 15 16
16 17 18 19 20 21 22  20 21 22 23 24 25 26  17 18 19 20 21 22 23
23 24 25 26 27 28 29  27 28 29 30 31        24 25 26 27 28 29 30
30 31                                       
      October               November              December
Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa
 1  2  3  4  5  6  7            1  2  3  4                  1  2
 8  9 10 11 12 13 14   5  6  7  8  9 10 11   3  4  5  6  7  8  9
15 16 17 18 19 20 21  12 13 14 15 16 17 18  10 11 12 13 14 15 16
22 23 24 25 26 27 28  19 20 21 22 23 24 25  17 18 19 20 21 22 23
29 30 31              26 27 28 29 30        24 25 26 27 28 29 30
                                            31
```

+ Add a symlink or install the completed code a global module on your system.

```
$ node-cal 02 2017 # no './' needed
   February 2017
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28
```
