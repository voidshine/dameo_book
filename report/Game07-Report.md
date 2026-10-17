# Game 7 — engine review of `Game07.md`

Mojmir Hanes vs Beginner_2017, 2:0. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

The game record is right: the main line matches `Game07.agt` move for move, every move is legal, and all 29 diagrams show the positions they should. The tempo count at Diagr.3 (40 against 37) is correct.

White's play is confirmed throughout. 17.f3f6! and 18.f4g5! are the only moves, 22.b3c4! and 23.c1c3! are best, 24.a3b4!! and 25.c2b3!! are a sound combination, and 29.h1h2!! ends the game. The opening comments also hold: 13...c7e5 was better than 13...h8h7, and 14.f2f3?! is a small inaccuracy.

The engine's disagreements concern where Black lost the game, and the long analysis of 23...h3h2:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | 23...h5g4? and Diagr.27 | The losing mistake; the combination "completely turned the game around" | 23...h5g4 is one of Black's best moves, and Diagr.27 is about level. The losing move is 28...e4f3 | Incorrect assessment |
| 2 | Continuation d6d5, move 28 | 27...d6d5! is "much stronger" and leads to a draw | 27...d6d5 loses to 28.c5c6!. The book's 28.c3c6 is the weaker move | Error (analysis) |
| 3 | Continuation d6d5, moves 32–52 | A drawn line with best play | Three moves change the result: 38.f7g8 loses to 38...d6f4!, 52...f6a6 misses the win 52...f6g5!, and 32.a6a7 is better than 32.a6b7! | Error (analysis) |
| 4 | Diagr.10: 26...f7g6! | "A much stronger move" than 26...h5h4 | 26...e5e4 and 26...d6d5 are better than both | Incorrect assessment |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations. A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it.** The engine often does not see a sacrifice until it is played. It reads the position before 29.h1h2!! as 1.11 and the position after it as 1.98. So where a verdict depends on a shot of that kind, I looked for it with a separate check that tries every legal move and every reply, and each finding below rests on a line I checked move by move. Differences under about 0.08 mean nothing.

## Findings in detail

### 1. Where Black lost: 28...e4f3, not 23...h5g4

The book marks 23...h5g4 with a "?" and, after 24.a3b4!! g4:e4 25.c2b3!! a5:a3 26.c3a5 a3:c5 27.a5:a7, says White has "completely turned the game around". At Diagr.28 it asks "How can Black save itself?"

The engine agrees that the combination is the best line for White, but not that it wins.

- **At Diagr.9** it rates 23...h5g4 (0.99) level with its own first choice, 23...f7e6 (1.01), and ahead of the 23...h3h2 that the chapter analyses at length (1.13).
- **At Diagr.27** White has a man about to king and Black has nine men against six. The engine has 1.09. Black's best is 27...c5d4. The standard play-out from the diagram was drawn; the wide play-out was won by White.
- **After 28.a7a8** the engine plays **28...d7d5** (1.06), supporting the centre and keeping the e4 man where it blocks. The standard play-out was drawn; the wide play-out was won by White.
- **28...e4f3** is the move that loses. I checked every White reply: 29.h1h2!! is 1.93 at 50k, and nothing else is above 1.18.

So White has the easier game after the combination, and the play-outs lean his way, but Black was still fighting until move 28. The "?" would sit more accurately on 28...e4f3.

The note that 29.a8f8 b6b5 "would have prolonged the resistance" is right, and no more than that (1.77).

### 2. Continuation d6d5: 28.c5c6!

At Diagr.11 the book examines 27...h5h4?!, 27...b6b5? and 27...d6d5!, and calls the last "much stronger". After 27...d6d5 it plays 28.c3c6. White has a better move with the other man:

```
27...d6d5   28.c5c6!   b6:d6    (forced)
29.a4b5     a5:c5
30.c4:e4
```

Engine: 1.70 after 28.c5c6. White won both play-outs.

The book's 28.c3c6 is 0.80, and then 28...d5:b5 keeps it there; the reply given, 28...b6:d6, is 1.16.

The engine's own choice at Diagr.11 is a fourth move, **27...g6g5** (1.22; the play-out was drawn). Of the three the book considers, 27...h5h4 is then the best (1.28 at 50k), and the verdicts on it and on 27...b6b5? stand.

### 3. Continuation d6d5: three later moves

Since the line is already decided at move 28, these matter only if the analysis is kept as an illustration.

- **32.a6b7!** carries the note "This is stronger than a6a7". The engine has it the other way: 32.a6a7 is 1.34 and 32.a6b7 is 0.82.
- **38.f7g8** loses to **38...d6f4!** (0.13): the white king must capture, 39.g8:c5 (g8:g5:c5), and 39...f4:c6 takes it. Black won both play-outs. The book plays 38...g6f5 (1.33) and reaches Diagr.21. White should king on f8 or e8 instead (0.87).
- **52...f6a6** allows the saving 53.c1a3!, as the book shows. But **52...f6g5!** wins (0.22; Black won both play-outs), so Diagr.24 is not a draw. The engine has Diagr.23 at 0.73 and Diagr.24 at 0.64, and Black won the play-outs from both.

### 4. Diagr.10: 26...f7g6!

The book prefers 26...f7g6! to 26...h5h4 because it blocks the f4 man. The engine prefers neither: 26...e5e4 and 26...d6d5 are 1.14 and 1.15 at 50k, 26...h5h4 is 1.34 and 26...f7g6 is 1.36 (1.39 at 2M). After 26...f7g6 it answers 27.f3g4 (1.39) and not 27.c2c5 (1.21).

This is the engine's opinion on a quiet position, so it carries less weight than the other three findings.

## Smaller points

- **22...a7b6** (1.01). The engine prefers 22...d6d5 (0.84).
- **14...h7h4?!** against 14...d6e5!. The engine sees less in it than the author does: 0.89 after each, and 0.97 after the game continuation to Diagr.8. Diagr.6 is 0.89, and Black won the play-out from it, so the recommendation is sound; the difference is just small.
- **13.e3e4** (0.87). 13.c2e4 is 0.99.
- **Continuation h5h4.** 34.e8:b1 a5b4!! is 1.12, so White keeps a little after all. Before that the engine rates 34.e8:b1 and 34.e8:b4! the same (1.30).

## Confirmed by the engine

- 13...c7e5 "looks more reliable": 0.87, against 0.99 for 13...h8h7. (13...e7e5 is the same.)
- 14.d3f5? d5:d3 15.c3:e3 h6g5 16.f4f6 g5:e5! is good for Black (0.72).
- 14.f2f3?! (0.90; 14.f2e3 is 1.00).
- At Diagr.5, 16.f6g7 h7g6 is bad for White (0.79).
- At Diagr.7, 17.e3e5?? d6d5 (0.54), and 17.f3f6! and 18.f4g5! are the only moves (the alternatives are 0.75 and 0.22).
- Black was right to decline 21...h3h2 22.h1:h3 h4:h2 23.g1g2 h2:f2 24.f1:f3 (1.15).
- 22...h5g4? 23.c4c5 wins for White (1.71).
- 24.a3b4!! and 25.c2b3!! are the best moves, and 26.c3a5 is the only follow-up (the alternative is 0.55).
- 29.h1h2!! h3:h1 30.f1e2 h1:a4 31.a8:h7 (1.98).
- Continuation b6b5: 27...b6b5? is weak (1.69), and Diagr.17 with 33.a3b4 is won (1.91; White won the play-out).
- Continuation h5h4: Diagr.15 is won for White (1.81; White won the play-out).
- At Diagr.25, 53.c1h1 a6h6 loses (0.04), and the final position after 57.b5b6 is drawn (1.00).

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 42 | "(а8-а5)" is written with two Cyrillic а (U+0430) | Latin a |
| 193 | `27.с2с5` is written with two Cyrillic с (U+0441) | Latin c |
| 239 | "3×3 king endgame" uses a multiplication sign; Games 2, 4 and 6 write "3x3", "2x2" | "3x3" |
| 468 | «groggy» uses guillemets; the rest of the book uses ‘ ’ or “ ” | ‘groggy’ |
| 289–445 | Continuation d6d5 | See findings 2 and 3 |
| 462 | `23... h5g4?` | See finding 1 |

The move 14.f2f3?! is printed twice as a main-line move (lines 85 and 120). That is a deliberate restatement after the digression, not an error.

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | b1b3 | 1.03 | 1.01 | b8a7 | 1.03 | 1.02 |  |
| 2 | d1a4 | 1.03 | 1.01 | g8g6 | 1.02 | 1.02 |  |
| 3 | b2a3 | 1.02 | 1.00 | d8b6 | 1.09 | 1.06 |  |
| 4 | d2b4 | 1.07 | 1.05 | b7a6 | 1.05 | 1.05 |  |
| 5 | e1d2 | 1.05 | 1.03 | b6a5 | 1.03 | 1.02 |  |
| 6 | e2c4 | 1.02 | 1.03 | g7g5 | 1.01 | 1.02 |  |
| 7 | d2f4 | 0.93 | 0.94 | e8h5 | 0.94 | 0.94 | 7.g2h3 1.03 |
| 8 | f3g4 | 0.97 | 0.94 | g5:g3 | 0.97 | 0.94 |  |
| 9 | g2:g4 | 0.97 | 0.94 | g6g5 | 0.96 | 0.96 |  |
| 10 | g4:g6 | 0.96 | 0.96 | f6:h6 | 0.96 | 0.96 |  |
| 11 | c4c5! | 0.96 | 0.94 | c6:c4 | 0.96 | 0.94 |  |
| 12 | b4:d4 | 0.97 | 0.94 | d7d5! | 0.97 | 0.99 |  |
| 13 | e3e4 | 0.89 | 0.87 | h8h7 | 1.00 | 0.99 | 13.c2e4 0.99; 13...e7e5 0.87 |
| 14 | f2f3?! | 0.90 | 0.90 | h7h4?! | 0.91 | 0.89 | 14.f2e3 1.00 |
| 15 | d3f5 | 0.84 | 0.97 | d5:d3 | 0.84 | 0.97 |  |
| 16 | c3:e3 | 0.84 | 0.97 | h6g5 | 0.84 | 0.97 |  |
| 17 | f3f6! | 1.08 | 0.98 | g5:e5 | 1.06 | 0.97 |  |
| 18 | f4g5! | 1.09 | 0.98 | e6:g4 | 1.09 | 0.98 |  |
| 19 | e4:g8 | 1.09 | 0.98 | g4h3 | 1.08 | 0.97 |  |
| 20 | g8:b8 | 1.08 | 0.97 | a8:c8 | 1.08 | 0.97 |  |
| 21 | e3f4 | 1.02 | 0.93 | c7e5 | 0.93 | 0.87 |  |
| 22 | b3c4! | 0.90 | 0.85 | a7b6 | 1.01 | 1.01 | 22...d6d5 0.84 |
| 23 | c1c3! | 1.01 | 1.01 | h5g4? | 1.01 | 0.99 |  |
| 24 | a3b4!! | 0.99 | 1.07 | g4:e4 | 0.97 | 1.07 |  |
| 25 | c2b3!! | 1.08 | 1.08 | a5:a3 | 1.08 | 1.08 |  |
| 26 | c3a5 | 1.11 | 1.09 | a3:c5 | 1.11 | 1.09 |  |
| 27 | a5:a7 | 1.11 | 1.09 | c8d7 | 1.17 | 1.10 |  |
| 28 | a7a8 | 1.15 | 1.07 | e4f3 | 1.20 | 1.11 | 28...d7d5 1.06 (finding 1) |
| 29 | h1h2!! | 1.92 | 1.98 | h3:h1 | 1.93 | 1.98 |  |
| 30 | f1e2 | 1.97 | 1.99 | h1:a4 | 1.98 | 1.99 |  |
| 31 | a8:h7 | 1.95 | 1.98 |  |  |  |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw. The entry for 28...e4f3 (1.11) is the search's reading before it has seen 29.h1h2; the true value of that position is the 1.98 in the next row.
