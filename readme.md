# Multiple Randomized Round Robins KSP Script for Kontakt

Randomized round robins something that is offered out of the box with Kontakt. However, the implementation that Kontakt offers does not protect you against repeating the same sample twice in a row. For a long time, KSP developers have made use of various scripts to offer "random (no-repeat)" functionality. Most of these scripts will only track one set of round robins per instrument. This script makes it possible to create multiple, independently tracked round robins within a single instrument.

# How to Use

1. Open your instrument in Kontakt. 
2. Open the script editor. 
3. Simply copy the contents of the multi-rr.ksp script into one of the empty tabs in the script editor. When choosing tabs
4. Configure the script (make sure to read the comments within the script carefully. Kontakt array definition is...interesting.)
5. Hit Apply

# Configuration

There are three variable that you will need to configure in order to set up your round robins:

## $NUM_SETS

The number of independent round robin sets the engine should track.
This is the number of independent round robin sets the engine should track:
```
declare const $NUM_SETS := 3
```

## $MAX_RR

The maximum number of round robins. In other words, the number of round robins in the set that contains the most different groups. 
```
declare const $MAX_RR := 5
```

## %round_robin_groups

The actual round robin groups. This multi-dimensional array variable contains the group IDs for each individual round robin set. You'll want to replace these group numbers with the IDs of the groups for each set. It's in a very particular format, so you'll want to study the example very carefully. Basically it's set up so that each line contains the group for a different "set" of round robins that need to be tracked. There should be exactly the same number of "rows" in this multi-dimensional array as you put in your $NUM_SETS variable above. So if you have four sets of round robins, then you need to have four rows below. Since multi-dimensional arrays are perfectly rectangular, every line needs to have the same number of elements. Specifically, the number of elements in every line needs to match what you put in $MAX_RR above. That being said, you don't need to have that many round robins in each group. If you have fewer than the $MAX_RR round robins in a given set, fill the remainder of the line with -1s. 

So, for example, if you have one round robin set that has four (4) groups, and two that have only three (3) groups, you would set your $MAX_RR to 4. Then, you would configure your array so that the first set has four 
groups specified. On the following two lines, you would specify the other two sets. Since each of those two sets only has three round robin groups, the final element in each line will be a -1:
```
declare %round_robin_groups[$NUM_SETS * $MAX_RR] := (...
 0,  1,  2,  3, -1, ...
 4,  5, -1, -1, -1, ...
 6,  7,  8, -1, -1)    
```
Another thing to take of note of in the example above is the way array assignments are formatted in KSP. Specifically, every line needs to end with three dots (`...`) except for the last line. Also, all of the assignment lines end with a comma (`,`), whereas the last line ends with a close parentheses (`)`).
 