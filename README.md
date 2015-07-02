bayeselo
by Rémi Coulom
http://remi.coulom.free.fr/Bayesian-Elo/

To compile under Linux, `cd src/ && make`.

This software is protected under the terms of the GNU GPL.
See `Copying.txt` or http://www.gnu.org/copyleft/gpl.html .


Bayeselo is a command-line tool. This section presents typical examples of use.

The example below shows how to compute ratings from a PGN file (wbec.pgn, located in the directory where the program is executed).

    ResultSet>readpgn wbec.pgn
    37 game(s) loaded, 0 game(s) with unknown result ignored.
    ResultSet>elo
    ResultSet-EloRating>mm
    Iteration 100: 1.60455e-005 
    00:00:00,00
    ResultSet-EloRating>exactdist
    00:00:00,04              
    ResultSet-EloRating>ratings
    Rank Name                  Elo    +    - games score oppo. draws 
       1 Dragon 4.7.5          120  167  148     8   75%     5   50% 
       2 Cerebro 2.05a          60  229  211     4   63%     1   25% 
       3 Movei 0.08.336          9  138  138    12   50%     6   17% 
       4 Zarkov 4.67             2  146  150     9   44%    18   44% 
    ...

If you wish to get the output of the "ratings" command in a file, you may redirect stdout:

    ratings >myfile.txt

bayeselo can produce a likelihood-of-superiorty matrix:

    ResultSet-EloRating>los 0 5 3
                         Dr Ce Mo Za Co
    Dragon 4.7.5            59 81 85 71
    Cerebro 2.05a        40    57 60 68
    Movei 0.08.336       18 42    51 51
    Zarkov 4.67          14 39 48    50
    Comet B.68           28 31 48 49

Bayeselo can also produce predictions for round-robin tournaments. For instance, if wbec1to9.pgn contains games of many players, it is possible to predict the outcome of a round-robin tournament between 4 of them and an unknown "Toto", with this kind of script:

    readpgn wbec1to9.pgn
    elo
     mm
     prediction
      rounds 2 ;This indicates that each player plays 4 games, 2 with each color.
      results
       addplayer Esc 1.16
       addplayer Pharaon 2.62
       addplayer Gandalf 4.32h
       addplayer TheCrazyBishop 0045
       addplayer Toto
      x
      players ;Displays the list of players
      simulate ;This runs 100000 random simulations of the tournament
      x
     x
    x

Sending the commands above to bayeselo will produce an output that looks like:

    Num                 Name      Elo Std.Dev.
      0             Esc 1.16  205.142  13.6535
      1         Pharaon 2.62   464.88  13.7103
      2        Gandalf 4.32h  537.769  15.2113
      3  TheCrazyBishop 0045  340.416  15.5046
      4                 Toto        0     1000
    (5 players)

    Rank Player name           Points EPoints  StdDev   ERank    1  2  3  4  5
       1 Gandalf 4.32h            0.0   11.55    2.06    1.59   52 37  9  1  0
       2 Pharaon 2.62             0.0   10.14    2.14    2.20   18 49 28  5  0
       3 TheCrazyBishop 0045      0.0    7.59    2.19    3.32    1  9 51 34  4
       4 Toto                     0.0    5.73    6.44    3.60   29  4  4  6 58
       5 Esc 1.16                 0.0    4.99    2.12    4.29    0  0  7 55 38

The EPoints column indicates the expected final score. ERank is the expected final rank. The matrix to the right indicates the probability in percent for every player and every rank.

This prediction tool may also be applied to a running tournament, where some of the games have already been played. In order to do this, simply replace the addplayer commands in the script by

    readpgn partial.pgn

where partial.pgn contains the current games of the round robin. If partial.pgn does not contain all the players of the tournament yet, you may add more players with the addplayer command. In case the rating of one participant is unknown, you can set it manually with the `elo` command at the level of the prediction interface.

The prediction tool also lets you change the number of points awarded for a win, a loss, and a draw. This way, it can be applied to generate predictions in the French Football Championship (soccer in the US), where a victory is 3 points, a draw 1 point and a loss 0 point. Default values are 1, 0.5, and 0.

If you want more usage information, you can get a list of available commands with the `?` command when running bayeselo.
