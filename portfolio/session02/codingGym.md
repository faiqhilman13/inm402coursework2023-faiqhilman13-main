---
id: litvis

narrative-schemas:
  - ../../lectures/narrative-schemas/teaching.yml
---

@import "../../lectures/css/datavis.less"

<!-- Everything above this line should probably be left untouched. -->

# Coding Gym 2 : Lists

_These exercises are designed to help you build confidence programming with Elm. They assume no prior experience of coding in any language other than that arising from previous weeks of this module. This document is designed to be viewed in VSCode with litvis so you can add your own code blocks to answer each question._

## 2.1. Lists of items

In the previous gym you created functions that generated single values such as an integer, a floating point number or a String of text. We move on now to consider [lists of items](https://package.elm-lang.org/packages/elm/core/latest/List). Each item can again be a number, some text etc. but now we can create an ordered collection of them.

Lists are indicated with square brackets and items separated with commas. Lists can also be empty. For example:

```elm {l}
people : List String
people =
    [ "Ada Lovelace", "Alan Turing", "Jacques Bertin" ]


birthYears : List Int
birthYears =
    [ 1815, 1912, 1918 ]


todoList : List String
todoList =
    []
```

{(task|}

Create separate functions, each in their own code block, that display the following. Remember to include `elm {l r}` in each code block header so you can see the results. If created successfully, you should see the evaluation of each function appear in the preview window.

- the names of two cities you have visited and two you would like to visit
- the integers 70 to 75 inclusive.
- the [triangular numbers](https://en.wikipedia.org/wiki/Triangular_number) with one or two digits.
- The first 5 numbers in the sequence 1/1, 1/2, 1/3,... etc.
- The colours of the rainbow.
- A list of mammals that have eight legs.

```elm {l r}
cities : List String
cities =
    [ "New York", "Tokyo"]


seventy : List Int
seventy =
    [ 70,71,72,73,74,75 ]


triangle : List number
triangle =
    [6,10]

fivenumbers : List Float
fivenumbers =
    [1/1,1/2,1/3,1/4,1/5]

colors : List String
colors =
    ["yellow","red","blue"]

mammals : List String
mammals =
    ["there are no mammals with eight legs, there are a bunch of insects tho!"]
```

{|task)}

## 2.2. In-built functions to generate lists

Elm has a number of functions that make it easier to create lists. In particular [List.range](https://package.elm-lang.org/packages/elm/core/latest/List#range) for creating a range of integers; [List.repeat](https://package.elm-lang.org/packages/elm/core/latest/List#repeat) for creating a list of repeated items, the [cons operator `::`](<https://package.elm-lang.org/packages/elm/core/latest/List#(::)>) for adding an item to a list and the [++](https://package.elm-lang.org/packages/elm/core/latest/Basics#++) operator for combining two lists. For example:

```elm {l r}
firstTen : List Int
firstTen =
    List.range 1 10


fiveGoldRings : List String
fiveGoldRings =
    List.repeat 5 "gold ring"


newKidOnTheBlock : List String
newKidOnTheBlock =
    "New kid" :: [ "old kid", "another old kid" ]


oddsAndEvens : List Int
oddsAndEvens =
    [ 1, 3, 5, 7, 9 ] ++ [ 2, 4, 6, 8 ]
```

{(task|}

Choosing from the list-generating functions above, create separate functions to do the following:

- List the whole numbers (integers) 1992 to 2023 inclusive
- List the integers between -250 and -88 inclusive
- Add your own name added to this group of travellers: `[ "Ripley", "Vasquez", "Bishop", "Burke" ]`
- The word "dozen" repeated half a dozen times.
- Create a single list made up by combining a list of three animals, three vegetables and three minerals.

{|task)}

```elm {l r}
years : List Int
years =
    List.range 1992 2023

numbers : List Int
numbers =
    List.range -250 (-88)

travellers : List String
travellers =
    [ "Ripley", "Vasquez", "Bishop", "Burke", "Faiq" ]

repeated : List String
repeated =
    List.repeat 6 "dozen"

animals : List String
animals =
    [ "Lion", "Elephant", "Giraffe" ]

vegetables : List String
vegetables =
    [ "Carrot", "Potato", "Tomato" ]

minerals : List String
minerals =
    [ "Gold", "Silver", "Copper" ]

combined : List String
combined =
    animals ++ vegetables ++ minerals

```

## 2.3. Summarising Lists

Elm has some in-built functions that can summarise the contents of a list including [List.length](https://package.elm-lang.org/packages/elm/core/latest/List#length), [List.sum](https://package.elm-lang.org/packages/elm/core/latest/List#sum) and [List.product](https://package.elm-lang.org/packages/elm/core/latest/List#product). For example:

```elm {l r}
firstTenTotal : Int
firstTenTotal =
    List.sum (List.range 1 10)


numberOfAnimals : Int
numberOfAnimals =
    List.length [ "cat", "dog", "chicken" ]


power : Int
power =
    List.product [ 2, 3, 4 ]
```

{(task|}

Using the list-summarising functions above, create separate functions to calculate the following:

- The total of the integers from -250 to -88
- The number of items in the combined list `[] ++ [ "Ripley", "Vasquez", "Bishop", "Burke" ]`
- The mean (average) of all integers from -250 to -88
- 7 factorial (equivalent to `7 * 6 * 5 * 4 * 3 * 2 * 1`)

```elm {l r}
num : List Int
num =
    List.range -250 (-88)

totality : Int
totality =
    List.sum numbers

travel : List String
travel =
    [ "Ripley", "Vasquez", "Bishop", "Burke" ]

counter : Int
counter =
    List.length travellers

mean : Float
mean =
    let
        total = List.sum numbers
        count = List.length numbers
    in
    toFloat total / toFloat count

factorial : Int
factorial =
    List.product (List.range 1 7)

```

{|task)}

## 2.4 Tuples

Sometimes we might wish to operate on a very small collection of items rather than a list of unspecified length. For this we can use a [tuple](https://package.elm-lang.org/packages/elm/core/latest/Tuple) which can contain two or three items only. An unlike a list, the items do not need to be of the same type.

Tuples are indicated with round rather than square brackets. For example:

```elm {l r}
ada : ( String, Int )
ada =
    ( "Ada Lovelace", 1815 )
```

{(task|}

Create functions that return tuples or lists of tuples that represent:

- Your name and commuting time.
- A point in 3d space (requiring 3 coordinates)
- A list of 2d coordinates (i.e. each coordinate requiring a pair of coordinate values, but a number of them)

```elm {l r}
type alias CommutingInformation =
    { name : String, commutingTime : Float }

commutingInformation : CommutingInformation
commutingInformation =
    { name = "Faiq Hilman", commutingTime = 35.0 }

type alias Point3D =
    { x : Float, y : Float, z : Float }

point : Point3D
point =
    { x = 1.0, y = 2.0, z = 3.0 }

type alias Point2D =
    { x : Float, y : Float }

points : List Point2D
points =
    [ { x = 1.0, y = 2.0 }, { x = 3.0, y = 4.0 }, { x = 5.0, y = 6.0 } ]

```

{|task)}
