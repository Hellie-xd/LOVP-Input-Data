# Input data lexicon

\<input type\>(\<number of people\>\[,\[\<number of people on concentration floor number\>-\]\<number of people going to concentration destination floor\>\])-\<xth number input in this subtype\>.csv
Ex: 
Random(5)-2.csv --> Random (type 1) with 5 people, 2nd input from this subset

ConcentratedRandom(20,10-4)-7.csv --> Concentrated Random (type 2), where out of 20 people 10 are random, 10 come from one floor, and out of those 10 from one floor 4 want to go to the same floor

Concentrated(10,3)-6.csv --> Concentrated (type 3) with 10 people, out of which all 10 are concentrated on one floor, and out of those 3 want to go to the same floor, the other 7 want to go to another

## Type 1: Random
Every person is fully random where they start, and where they leave
Subtypes:
- 25x5 people
- 25x10 people
- 25x20 people

## Type 2: Concentrated Random
Half of the people (truncated to the next full integer if not a whole number) are concentrated on one floor, and some of them want to go to a single floor, whilst some want to go to other floors (perhaps a class ended on that level in that moment, and a lot of people wanted to leave), plus random people coming to the lift naturally
Subtypes:
- 25x5 people, 3 concentrated, 2 same target
- 25x10 people, 5 concentrated, 3 same target
- 25x20 people, 10 concentrated, 5 same target
- 25x5 people, 3 concentrated (all same)
- 25x10 people, 5 concentrated (all same)
- 25x20 people, 10 concentrated (all same)


## Type 3: Concentrated
Every person comes from one level, and some want to go to one level, some want to go to others
Subtypes:
- 25x5 people, 2 same target
- 25x10 people, 3 same target
- 25x20 people, 5 same target
- 25x5 people, 3 same target
- 25x10 people, 6 same target
- 25x20 people, 12 same target


## csv columns
each person in one input file looks like the following: 

\<weight in kg\>;\<from which floor\>;\<destination floor\>;\n