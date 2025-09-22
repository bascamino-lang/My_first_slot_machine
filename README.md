This is my slot machine.

THE GAME:
In every game, you spin five (given) reels. Every entry of every reel is one of the eight symbols 
in the set {H1,...,H4,L1,...,L4}. For every spin, each reel shows 4 (consecutive) entries; therefore, every
spin returns a matrix 4x5 (the slot screen). You win something if there are three (or more) consecutive reels with a common
symbol. In particular, if there are n-consecutive reels with a common symbol, then you count how many
ways you can go from the left-most to the right-most reel and every path gives you a score.

HOW TO PLAY:
Simply run Main.java. It prints the slot screen (the 4x5 matrix) with the symbols and the winning score of this spin.
Moreover, in Main.java you can find three commented sections:
    1) lines 19-27. This section contains the for cycle that checks the slot RTP, which is 0.961 (the former one was 0.98).
    2) lines 30-51. This section computed the "statistic matrix" of the slot. The matrix indicates how frequently a particular combination of a given symbol appears in the slot machine.
    3) lines 53-88. Starting from the statistical matrix, I computed that one can obtain RTP = 0.96 if the payout for a 3OAK of L4 is changed from 0.1 to 0.02.
    I used this section to perform a theoretical double-check of the RTP, based on the statistical matrix and the new pricing criterion."

THE CLASSES
NewReels.java
This class contains the function that reads the reels from a .json file.
These reels are strings of characters H1, H2, etc. The function takes these strings and converts them into integer
vectors (1...8). I did this because it requires less memory and is easier to work with.
The file data_Int.json contains the translated reels 

OneFlip.java
This class contains the function playWithInt. This function takes as input the reels, read as integer vectors, and
as output, a 4×5 integer matrix that corresponds to the screen of our slot machine.
This function generates a random position for each reel using the function ThreadLocalRandom.current().nextInt(0, reelLength);
and then builds the 4×5 matrix by storing the next four entries of the reel.

WinCalc.java
This class computes the payout of the 4×5 integer matrix corresponding to the screen of our slot machine.
First, we define the matrices wins and prices. The wins matrix is a 8x5 matrix that describes the six winning patterns that can occur.
For example, the row {1,0,1,1,1} means that all reels except the second one contain a common symbol; in particular, this corresponds to a 3OAK.
The matrix prices is an 8×3 matrix. The eight rows correspond to the eight symbols, and the columns correspond, in order,
to the 3OAK, 4OAK, and 5OAK payouts. For example, the first row {1.0, 1.5, 2.0} means that a 3OAK of H1 pays 1.0, and so on.
Remark: if the 3OAK of L4 is equal to 0.1, then the RTP is 98%, whereas if it is equal to 0.02, the RTP is 96%.

The function positionOfPatterInPrice, given a pattern row, returns the corresponding winning score from the prices matrix.
This is needed because wins has 8 rows, whereas prices has only 3 columns.
The function contZeros counts how many zeros an array contains. elementWiseMultiply takes two arrays and multiplies them entry 
by entry. Finally, multplyNonZeros takes an array and returns the product of its non-zero entries.

The function symbMatrix takes as input the 4x5 matrix corresponding to the slot screen and returns an 8x5 matrix, where
the entry (i, j) indicates how many times the i-th symbol appears in the j-th reel. This matrix, which will be called the
symbols matrix, is the one from which the winning patterns and the corresponding winning scores are calculated.

The function computeScore calculates the total winning score for a given slot machine screen. This function takes as an input
the symbols matrix of the slot screen. The function compares the lines of the symbols matrix against all defined winning
patterns to identify which patterns are present. For each matched pattern, it retrieves the corresponding payout from the
prices matrix, multiplies it by the number of possible symbol combinations, and sums all contributions.
It returns a double representing the total winning score for that screen.

The function theWinninPattersOfTheMatrix works similarly to computeScore. It takes as input the symbols matrix and
returns a 8x8 matrix whose (i, j) entry indicates how many times the j-pattern appears in the i-th symbol. This function
is needed to compute the statistic matrix.

MatrixPrinter.java
This class contains the functions for printing the matrices.
We just report the function printMatrixTableIntReels that takes as an input the 4x5 integer matrix corresponding to the
slot screen and prints the 4x5 matrix (in symbols H1...L4) with the corresponding winning score.
