---
layout: post
title: "Learning Log of Elm"
date: 2020-07-25 15:09:00 -0400
categories: Elm Functional Programming
excerpt_separator: <!--more-->
---

Elm programming language is a notation for writing programs that run on a web browser. The description of the language is split into two components: syntax and semantics.

Elm provides quite a few benefits that are lacking in most mainstream languages: immutable values, pure functions, fuzz testing, a powerful type system, pattern matching, and the absence of runtime errors.

<!--more-->

## Learning Log of Elm

Elm programming language is a notation for writing programs that run on a web browser. The description of the language is split into two components: syntax and semantics.

Elm provides quite a few benefits that are lacking in most mainstream languages: immutable values, pure functions, fuzz testing, a powerful type system, pattern matching, and the absence of runtime errors.

```elm
module Playground exposing (main)

import Html

escapeEarth myVelocity mySpeed fuelStatus =
    let
        escapeVelocityInKmPerSec =
            11.186

        orbitalSpeedInKmPerSec =
            7.67

        whereToLand =
            if fuel == "low" then
                "Land on droneship"

            else
                "Land on launchpad"
    in
    if myVelocity > escapeVelocityInKmPerSec then
        "Godspeed"

    else if mySpeed == orbitalSpeedInKmPerSec then
        "Stay in orbit"

    else
        whereToLand fuelStatus


computeSpeed distance time =
    distance / time


computeTime startTime endTime =
    endTime - startTime

revelation=
    """
    It became very clear to me sitting out there today
    that every decision I've made in my entire life has
    been wrong. My life is the complete "opposite" of
    everything I want it to be. Every instinct I have,
    in every aspect of life, be it something to wear,
    something to eat - it's all been wrong.
    """

main =
    {- Html.text (escapeEarth 11 (computeSpeed 7.67 (computeTime 2 3))) -}
    -- partial function application
    computeTime 2 3
        |> computeSpeed 7.67
        -- |> called forward function applicaion operator , or pipe operator ( pipe the result from previous expression to the next one)
        |> escapeEarth 11
        |> Html.text



{---------------------------------------}
{- backforward function application <| -}
add a b =
a + b

multiply c d =
c * d

divide e f =
e / f

main =
Html.text (String.fromFloat (add 5 (multiply 10 (divide 30 10))))

--forward
divide 30 10
|> multiply 10
|> add 5
|> String.fromFloat
|> Html.text

--backward
Html.text <| String.fromFloat <| add 5 <| multiply 10 <| divide 30 10


--Filter a string
isValid char= char /=  '-'
{-  
>isValid = \char -> char /= '-'    
<function>

>isValid '-'
False
-}

String.filter isValid "222-11-5555"
-- output: "222115555"

-- anonymous function
-- An anonymous function like (\param -> someFunction x param) can always be 
-- rewritten as (someFunction x) as long as param is the last argument.
String.filter (\char -> char /= '-') "222-11-5555"
-- output: "222115555"


{- Regular Expressions -}
import Regex

--Define a pattern that matches the time (09:32 a.m.) we're looking for
pattern = "\\d\\d:\\d\\d (a\\.m\\.|p\\.m\\.)"
-- "\\d\\d:\\d\\d (a\\.m\\.|p\\.m\\.)"

-- Create a regular expression by passing pattern to the Regex.fromString function
maybeRegex = Regex.fromString pattern
-- Just {} : Maybe Regex

-- Extract the regular expression inside Maybe container using the Maybe.withDefault function
regex = Maybe.withDefault Regex.never maybeRegex
-- {}

apollo11 = """
    On July 16, 1969, the massive Saturn V rocket 
    lifted off from NASA's Kennedy Space Center at 
    09:32 a.m. EDT. Four days later, on July 20, Neil
    Armstrong and Buzz Aldrin landed on the Moon.
    """

-- verify that the substring exists
isLaunchTimeIn = Regex.contains regex apollo11
-- True

-- extract the substring, Regex.find return a list of records
launchTimes = Regex.find regex apollo11
-- [{ index = 103, match = "09:32 a.m.", number = 1, submatches = [Just "a.m."] }]

--extract the match key inside the record
time= List.map (\launchTime -> launchTime.match) launchTimes
--["09:32 a.m."] : List String

{- descending sort -}
descending a b =
    case compare a b of
        LT ->
            GT
        GT ->
            LT
        EQ ->
            EQ

List.sortWith descending [ 316, 320, 312, 370, 337, 318, 314 ]
-- [370,337,320,318,316,314,312]


{- Returning Multiple Values From a Function -}
validateEmail email =
    let
        emailPattern =
            "\\b[A-Za-z0-9._%+1]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}\\b"

        regex =
            Maybe.withDefault Regex.never <|
                Regex.fromString emailPattern
        
        isValid =
            Regex.contains regex email
    in
    if isValid then
        ( "Valid email","green" )
    else
        ( "Invalid email","red" )

-- main =
    validateEmail "thedude@rubix.com"
        |> Debug.toString
        |> Html.text


{- Record -}
type alias  TVShow =
    { creator : String
    , episodes : Int
    , name : String
    }

firefly = TVShow "Joss Whedon" 14 "Firefly"
-- {creator="Joss Whedon", episodes=14, name="Firefly"}

-- Accessing values
hasCreator tvShow = String.length tvShow.creator > 0
-- <function> : { a | creator : String } -> Bool

hasCreator firefly
-- True :  Bool

got = TVShow "" 60 "Game of Thrones"
-- { Creator = "", episodes = 60, name = "Game of Thrones" }

hasCreator got
-- False : Bool

firefly.creator
--"Joss Whedon"
firefly.episodes
--14
firefly.name
--"Firefly"

--Special Accessor Functions
.creator firefly
--"Joss Whedon"

.episodes firefly
--14

.name firefly
--"Firefly"

--Modifyling a record
--all values in Elm are immutable. Elm doesn't modify an existing record.
--It always returns a new one that contains the modified value
incrementEpisode tvShow = { tvShow | episodes = tvShow.episodes + 1 }
--<function>
firefly.episodes
--14
incrementEpisode firefly
--{ creator = "Joss Whedon", episodes = 15, name = "Firefly" }

```

