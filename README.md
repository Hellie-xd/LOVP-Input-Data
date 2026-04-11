# Input data lexicon

```<input type>(<number of people>[,[<number of people on concentration floor number>-]<number of people going to concentration destination floor>])-<xth number input in this subtype>.csv```

**examples**: 
- ```Random(5)-2.csv``` --> Random (type 1) with 5 people, 2nd input from this subset

- ```ConcentratedRandom(20,10-4)-7.csv``` --> Concentrated Random (type 2), where out of 20 people 10 are random, 10 come from one floor, and out of those 10 from one floor 4 want to go to the same floor, 7th input from this subset

- ```Concentrated(10,3)-6.csv``` --> Concentrated (type 3) with 10 people, out of which all 10 are concentrated on one floor, and out of those 3 want to go to the same floor, the other 7 want to go to another, 6th input from this subset



## csv columns
each person in one input file looks like the following: 


`<weight in kg>;<from which floor>;<destination floor>;\n`

## Type 1: Random
Every person is fully random where they start, and where they leave

**example:** ```Random(5)-0.csv``` $\to$ 
```
87;8;2;
97;4;6;
85;3;0;
72;4;5;
85;8;1;
```
*Subtypes*:
- 25 $\times$ 5 people
- 25 $\times$ 10 people
- 25 $\times$ 20 people

## Type 2: Concentrated Random
Half of the people (truncated to the next full integer if not a whole number) are concentrated on one floor, and some of them want to go to a single floor, whilst some want to go to other floors (perhaps a class ended on that level in that moment, and a lot of people wanted to leave), plus random people coming to the lift naturally 

**example:** ```ConcentratedPlusRandom(10,5-3)-17.csv``` $\to$ 

```
73;4;6;
79;4;1;
79;5;3;
77;3;0;
82;2;7;
86;7;0;
85;7;0;
87;7;0;
82;7;8;
81;7;6;
```

*Subtypes*:
- 25 $\times$ 5 people, 3 concentrated, 2 same destination
- 25 $\times$ 10 people, 5 concentrated, 3 same destination
- 25 $\times$ 20 people, 10 concentrated, 5 same destination
- 25 $\times$ 5 people, 3 concentrated (all same)
- 25 $\times$ 10 people, 5 concentrated (all same)
- 25 $\times$ 20 people, 10 concentrated (all same)


## Type 3: Concentrated
Every person comes from one level, and some want to go to one level, some want to go to others

Subtypes:
- 25 $\times$ 5 people, 2 same destination
- 25 $\times$ 10 people, 3 same destination
- 25 $\times$ 20 people, 5 same destination
- 25 $\times$ 5 people, 3 same destination
- 25 $\times$ 10 people, 6 same destination
- 25 $\times$ 20 people, 12 same destination

# Optimal results
## Methodology
Used GAMS with Cplex with the following constraints:
```gams
Option  MIP = Cplex;  
Option  ResLim = 500;  
Option  Threads = 0;
```

For the accuracy (`optcr`):
- 0.0 (0%) if 5 people
- 0.05 (5%) if 10 people
- 0.1 (10%) if 20 people

Unfortunately due to computational limitations these thresholds are rarely achieved even after 500s of runtime per input.

## Optimal result files
The currently calculated optimal results for each input can be found in the *Optimal_Results/* subfolders, with their corresponding typename sorted. 

**example**: `Optimal_Results/Type1`

<hr>

In each folder, you will find a "*Batch_Lift_Results.txt*" file, verbosely depicting all of the runs in readable format, showing the most optimal result that the solver found.

**example**:
`Type1/Batch_Lift_Results.txt`
```
...

===============================================================
 RESULTS FOR FILE: Random(5)-1
 TOTAL TIME: 49 | BEST POSSIBLE: 49.00
 ABS GAP:    0.00 | REL GAP: 0.00 % (TARGET: 0%)
---------------------------------------------------------------
  1   |   0   | Doors Open. P3 IN.
  2   |   1   | Doors Open. P2 IN.
  3   |   4   | Doors Open. P1 IN. P4 IN.
  4   |   3   | Doors Open. P2 OUT.
  5   |   2   | Doors Open. P1 OUT.
  6   |   5   | Doors Open. P5 IN.
  7   |   7   | Doors Open. P4 OUT.
  8   |   7   | Idle.
  9   |   7   | In transit...
 10   |   9   | Doors Open. P3 OUT. P5 OUT.

...
```
`TOTAL TIME`: The currently depicted solution's total time
`BEST POSSIBLE`: Best *technically* possible time calculated by Cplex, not necessarily possible
`REL GAP`: Relative gap *(in %)* between `TOTAL TIME` and `BEST POSSIBLE`

<hr>

The *Optimal_Results.csv* file contains all of the total time, best possible times, relative gaps for each input

**example:**
`Type1/Optimal_Results.csv`
```csv
filename			;optimal_result	;best_possible	;gap_percent	;target_gap
Random(10)-24.csv	;74				;62.40			;15.67%			;5%
Random(20)-7.csv	;136			;100.83			;25.86%			;10%
Random(10)-21.csv	;74				;70.30			;5.00%			;5%
Random(20)-14.csv	;130			;88.25			;32.12%			;10%
Random(5)-1.csv		;49				;49.00			;0.00%			;0%
...
```
<center><i>tabulated for readability, in the actual file no tabs exist</i></center>

<hr>

In the *json* directory you can find each individual solution's itiniary plan

**example:**
`Random(5)-0-stops.json`
```
[
  {
    "floor": 8,
    "people_got_out": [],
    "people_got_in": ["person-1", "person-5"]
  },
  {
    "floor": 4,
    "people_got_out": [],
    "people_got_in": ["person-2", "person-4"]
  },
  ...
 ]
```
<hr>

In the *txt* directory you can find each individual solution's itiniary plan in readable format (like in "*Batch_Lift_Results.txt*")

**example:**
`Random(5)-0-result.txt`
```
===============================================================
 ROUTE ITINERARY FOR: Random(5)-0
 TOTAL TIME: 48 | BEST POSSIBLE: 48.00
 ABS GAP:    0.00 | REL GAP: 0.00 % (TARGET: 0%)
---------------------------------------------------------------
  1   |   8   | Idle.
  2   |   8   | Doors Open. P1 IN. P5 IN.
  3   |   4   | Doors Open. P2 IN. P4 IN.
  4   |   6   | Doors Open. P2 OUT.
  5   |   5   | Doors Open. P4 OUT.
  6   |   3   | Doors Open. P3 IN.
  7   |   3   | In transit...
  8   |   2   | Doors Open. P1 OUT.
  9   |   1   | Doors Open. P5 OUT.
 10   |   0   | Doors Open. P3 OUT.
```
