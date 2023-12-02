Both the solutions expect the input to be in a file named `defin`. This input is implicitly read by MATL as a character array, and placed on the stack.

## Explaining the solution for Part 2 in p2.matl

(The explanation for the solution for Part 1 is contained within this; just exclude the lines from "Push this cell array" to "End loop over digit names")

Split the input into lines

    Yb

For each line

    "

Push the line on to the stack

      @

Line is in a "cell" now, extract it as a normal string

      Y:

Store the line in the clipbord `H` to be used later

      XH

If the line has any length (i.e. it's not yet End-Of-Input)

      t
      n
      ?

Push a cell array containing the names of digits on to the stack

        'one two three four five six seven eight nine'Yb

For each digit name

        "

Paste the input line from `H` on to the stack

          H

Search for the current digit's name within the input line with `strfind`

          @
          Y:
          Xf

This returns the indices where the digit name can be found within the line, for eg. with line `'78foursixfour'` and current digit `'four'`, this returns `[3 10]`. If the digit is not found, it returns the empty array `[]`. First we check for that: if the `strfind` result has any length

          t
          n
          ?

Then we push the current digit _as a number_ on to the stack

            X@

and `num2str` it i.e. change `4` to `'4'`

            V

Swap this digit-as-character and the `strfind` result indices, so that the indices are on top

            w

Assign values to indices i.e. in the input string, at each index that `strfind` returned, replace the character there with the digit-as-character. For eg. in `'78foursixfour'`, assigning `'4'` to indices `3` and `10` means that the line becomes `'784oursix4our'`. Doing it this way instead of using `regexprep` allows us to handle overlapping number names easily. For eg., `'eightwo'` becomes `'8igh2wo'` without needing multiple passes or other tricks. 

            (

Else if `strfind` returned an empty array, delete it since it's useless

          }
            x

End if

          ]

End loop over digit names

        ]

Look for the regex `'\d'` in the line

        '\d'
        XX

Discard everything but the first and last entry from the results

        5L
        X)

Join those two to form the two digit number

        h

`str2num` it

        U

Concatenate it with the sum we have so far

        v

And then sum those values up

        s

The end to the main loop over input lines is implicitly done by MATL.

At the end, the final sum value is left in the stack, which MATL automatically prints out.

## Explaining the solution for Part 2 in p2.baseencode.matl

Same as above, except instead of directly having `'one two three four five six seven eight nine'` in the code, we use an encoded form of that string, saving 2 bytes overall.

`'Ct[{&HJ72%]8axpXw08InAAC(TG^Lpo3'` is the result of `matl ' ''one two three four five six seven eight nine'' 2Y2'' ''h F Za'` - we take that list of space-separated digit-names as being encoded in a base of lowercase letters + space character, and re-encode in a base containing all printable ASCII except the `'` character.

This code is much slower because we decode the string afresh for every input. In the best traditions of codegolfing, we save a couple of bytes at the cost of a run time that's three times longer.
