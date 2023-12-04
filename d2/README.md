
Both the solutions expect the input to be in a file named `defin`. This input is implicitly read by MATL as a character array, and placed on the stack.

## Explanation of solution to part 1

% make clipboard H empty

    []XHx

% For each input line i.e. each game

    10&Yb"@g

% If the line is empty, this is end-of-input, so sum up the valid Game IDs and exit

          tn~?Hs.]

% Push the game ID on to clipboard H

          HX@hXHx

% For each reveal of cube-set separated by ';' = 59

          59&Yb"@g

% For each colour

                'red green blue'Yb"

% Duplicate the current set of revealed cubes

                                   t

% create the regex '(\d+) colourname'

                                   '(\d+) '@gh

% Match and get the digit part alone on stack

                                   6&XXggU

% If there was no match (no balls of this colour), turn [] into 0

                                   s

% If the number of balls is > the total num of cubes of that colour,

                                   11X@+>?

% zero out the current game ID.

                                           H0J(XHx
                                         ]
                                   ]

%  delete the current cube-set

                x

## Explanation of solution to part 2

% Initialize the power-sum to 0

    0w

% For each input line i.e. each game

    10&Yb"@g

% If the line has any content i.e. isn't end-of-input

          tn?

% For each colour

            'red green blue'Yb"

% Duplicate the current line

                               t

% create the regex '(\d+) colourname'

                               '(\d+) '@gh

% Match and get the number parts alone on stack as an array

                               6&XXgXU

% Keep only the maximum number for the current ball colour

                               X>

% Bring the current line back on top of the stack

                               w
                               ]

% Delete the current line, multiply the number of cubes of each colour

             x**

% Add this power value to the overall power-sum

             +

