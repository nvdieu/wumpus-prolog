# Wumpus World game to learn Propositional logic and Predicate logic

## - install prolog
[https://www.swi-prolog.org/download/stable](https://www.swi-prolog.org/download/stable)
## - Clone
git clone https://github.com/nvdieu/wumpus-prolog.git
## - Wumpus game play
cd wumpus-prolog

swipl

-? consult('wumpus.qlf').

-? start.

## - Using help command in game to describe the commands of game
## - Each step, using propositional logic inference to design to go
## - Wumpus example in AIMA book (Stuart J. Russell and Peter Norvig)
<div align="center">
<img src="wumpus.png" alt="Wumpus in AIMA book" width="30%" height="30%">
</div>

## - Rule:

    1 wumpus, 1 gold, 1 or more pit.

    Start position at (0,0).

    Use climbdown to (1,1) position.

    You die if you enter a Cell with a Wumpus or a Pit.

    Go around to find and grab gold.

    Turn back to (1,1) position.

    Use climbup to (0,0) position and finish: Win

## - Knowledge Base: KB

## Position(x,y) we Percieve:

**KB(1):**

    Breeze(x,y) ⇔ (Pit(x+1, y), x < 4) ∨ (Pit(x-1,y), x > 1) ∨ (Pit(x, y+1), y < 4) ∨ (Pit(x,y-1), y > 1)

**KB(2):**

    Stench(x,y) ⇔ (wumpus(x-1, y), x > 1) ∨ (wumpus(x+1,y), x < 4) ∨ (wumpus(x, y-1), y > 1) ∨ (wumpus(x,y+1), y < 4)

**KB(3):**

    Glitter(x,y) ⇔ Gold(x,y)

**KB(4):**

    Bump ⇔ (goleft, x = 1) ∨ (goright, x = 4) ∨ (goup, y=4) ∨ (godown, y=1)

**KB(5):**

    Scream ⇔ wumpus is killed


## Percieve format (Breeze, Stench, Glitter, Bump, Scream)

## Start -> climbdown

    Position(1,1) , Percieve(¬Breeze, ¬Stench, ¬Glitter, ¬Bump, ¬Scream)

**KB(1) ⊨**

    ¬Breeze(1,1) ⇔ ¬(Pit(2, 1) ∨ Pit(1, 2))
                 ⇔ ¬Pit(2,1) , ¬Pit(1,2)

**KB(6):**

    ¬Pit(2,1)

**KB(7):**

    ¬Pit(1,2)

**KB(2) ⊨**

    ¬Stench(1,1) ⇔ ¬(wumpus(2, 1) ∨ wumpus(1, 2))
                 ⇔ ¬wumpus(2,1) , ¬wumpus(1,2)

**KB(8):** 

    ¬wumpus(2,1)

**KB(9):**

    ¬wumpus(1,2)

## Now we can go right or go up. If we choose go right:

    Position(2,1) , Percieve(Breeze, ¬Stench, ¬Glitter, ¬Bump, ¬Scream)

**KB(1) ⊨**

    Breeze(2,1) ⇔ Pit(3,1) ∨ Pit(2,2)

**KB(10):**

    Pit(3,1) ∨ Pit(2,2)

**KB(2) ⊨**

    ¬Stench(2,1) ⇔ ¬(wumpus(1, 1) ∨ wumpus(3, 1) ∨ wumpus(2, 2))

                 ⇔ ¬wwumpus(1, 1) , ¬wumpus(3, 1) , ¬wumpus(2, 2)

**KB(11):**

    ¬wumpus(1,1)

**KB(12):**

    ¬wumpus(3,1)

**KB(13):**

    ¬wumpus(2,2)

## Now we can'n go right or go up. We tern bach to (1,1) and go up:

    Position(1,2) , Percieve(¬Breeze, Stench, ¬Glitter, ¬Bump, ¬Scream)

**KB(1) ⊨**

    ¬Breeze(1,2) ⇔ ¬(Pit(2, 2) ∨ Pit(1, 1) ∨ Pit(1, 3))
                 ⇔ ¬Pit(2, 2) , ¬Pit(1, 1) , ¬Pit(1, 3)
**KB(14):**

    ¬Pit(2, 2)

**KB(15):**

    ¬Pit(1, 1)

**KB(16):**

    ¬Pit(1, 3)

**KB(2) ⊨**

    Stench(1,2) ⇔ wumpus(2, 2) ∨ wumpus(1, 1) ∨ wumpus(1, 3)

**KB(17):**

    wumpus(2, 2) ∨ wumpus(1, 1) ∨ wumpus(1, 3)

## CNF for Resolution:

    KB(17), KB(11), KB(13)

    (wumpus(2,2) ∨ wumpus(1,1) ∨ wumpus(1,3)), ¬wumpus(1,1), ¬wumpus(2,2)

    wumpus(1,3)

**KB(18):**

    wumpus(1,3)

## Now we can go right

    Position(2,2) , Percieve(¬Breeze, ¬Stench, ¬Glitter, ¬Bump, ¬Scream)

    {Nothing}

## Now we can go to up or right from (2,2). We choose go right:

    Position(3,2) , Percieve(Breeze, ¬Stench, ¬Glitter, ¬Bump, ¬Scream)

**KB(1) ⊨**

    Breeze(3,2) ⇔ Pit(2,2) ∨ Pit(4,2) ∨ Pit(3,1) ∨ Pit(3,3)

    (Pit(2,2) ∨ Pit(4,2) ∨ Pit(3,1) ∨ Pit(3,3)) , KB(14)

    (Pit(2,2) ∨ Pit(4,2) ∨ Pit(3,1) ∨ Pit(3,3)) , ¬Pit(2,2)

    Pit(4,2) ∨ Pit(3,1) ∨ Pit(3,3)

**KB(19):**

    Pit(4,2) ∨ Pit(3,1) ∨ Pit(3,3)

## Now we tern back to (2,2) {Nothing} and go up:

    Position(2,3) , Percieve(Breeze, Stench, Glitter, ¬Bump, ¬Scream)

**KB(3) ⊨**

    Glitter(2,3) ⇔ Gold(2,3)

## We find gold, Crab gold anh tern back to (2,2) -> (2,1) -> (1,1) then climb up to (0,0): Win




