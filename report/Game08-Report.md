# Game 8 — engine review of `Game08.md`

Kirin vs Beginner_2017, 0:2. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

The game record is right: the main line matches `Game08.agt` move for move, every move is legal, and all 24 diagrams show the positions they should.

The chapter's two big moments are confirmed. 23.h1d5? does lose to 23...f5g4!!, which both players missed, and 28.d5e6 does lose to 29...b5b3!!. The tactical notes around moves 13 and 14 are right, and the final position is won for Black.

The engine adds one more missed combination to the game, and it disagrees with several verdicts in the side analysis, most of them in the same direction: Black's resources are better than the chapter thinks.

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | 12.b1a2 h8h6 | White "strengthens his left flank" | 12.b1a2 allows 12...a5b4!, a forced five-man capture that turns a man down into a man up. Both players missed it | Missed opportunity (Black), omission |
| 2 | Diagr.17: 28.a3a4 | "An excellent chance of winning"; 28...b5b3 no longer works | 28...b5b3! still works: 29.c3:a3 loses to 29...f5e4! (the book plays 29...f7e6), and 29.a4:c4 is only level | Error (analysis) |
| 3 | Diagr.13 and Diagr.14 (line C) | "Two white kings will break through"; "White is gradually winning" | Both positions are lost for White to ...b6a5! | Error (analysis) |
| 4 | Diagr.12, lines A and B | Both end "2:0" | Both final positions are level after 31...b5c4!. White wins only with a different 29th move | Error (analysis) |
| 5 | Diagr.9, option 2 | 26.d7e8 wins | 26.d7e8 loses to 26...f7f4!. The winning try is 26.f3e4 | Error (analysis) |
| 6 | Diagr.6: 18.d3c4 | "An overwhelming advantage" | The same as the game's 18.c1c3 (1.22 and 1.24) | Incorrect assessment |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations. A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it.** The engine often misses a sacrifice until it is played. It rates 27...b7b4 at 1.35 because it does not see 28...b5b3 coming; once that move is on the board it reads 0.80. So each finding below rests on a line I checked move by move, and where the engine's first number was blind I say so. Differences under about 0.08 mean nothing.

## Findings in detail

### 1. 12.b1a2 allows 12...a5b4!

At Diagr.4 White is a man up. The book says that with 12.b1a2 "Kirin strengthens his left flank". The move loses the extra man and one more:

```
12.b1a2?   a5b4!
13.a3:c5   f7d5      (a3:a5:c5 is forced)
14.c5:e5   e6:g6     (forced; then e6:e4:e2:g2:g4:g6, five men)
```

Black's e6 man takes e5, e3, f2, g3 and g5 in one move. White has 9 men against 10. Engine: 0.73 after 12...a5b4, against 1.25 for the game's 12...h8h6. Black won both play-outs.

White should play 12.a1c3 (1.22).

### 2. Diagr.17: 28.a3a4 b5b3! still works

The book says that after 27...b7b4?! Kirin "should have cooled his blood to zero and chosen the continuation 28.a3a4", and that with no black man on e6 the combination 28...b5b3 "does not have the desired effect". Its line is 28...b5b3?! 29.c3:a3 f7e6 30.a4:c4 f6e5 and White wins (Diagr.18–20).

At move 29 Black has a different second move:

```
28.a3a4    b5b3!
29.c3:a3   f5e4!
30.f4:d4   f6e5
31.d5:f5   b6c5
32.a4:c4   c5:e5     (forced; c5:c3:e3:g3:g5:e5, five men)
```

White has three men left against four. Engine: 0.43 after 29...f5e4. Black won both play-outs.

White's better capture is 29.a4:c4 b3:b1, and that is level (0.94; both play-outs were drawn).

So after 27...b7b4 White has no advantage to play for. 28.a3a4 comes to 0.94, and the two other reasonable moves are no better: 28.d5d6 is 1.02 and 28.c3d4 is 0.97 against Black's best reply (50k). That makes 27...b7b4 about as good as 27...f7e6 (1.05), and the "?!" undersells it.

The line the book gives as an alternative for Black, 28...b6c5 (Diagr.21–22), is the weaker choice: 1.37, and White won both play-outs.

### 3. Diagr.13 and Diagr.14: lost for White

In line C (27...f7e6 28.g4h5 e6g4 29.f4:h4 f6f4 30.h4h6! f4:f2 31.h5h7 f2f1 32.h6h8) the book reaches Diagr.13 and says "two white kings will break through any defence". After 32...f1h1 33.h8:a8 h1:h8 34.d5e6 it reaches Diagr.14: "White is gradually winning".

In both diagrams Black has the same quiet move, and it wins.

**Diagr.13: 32...b6a5!** The man leaves b6, and now the white king's compulsory capture runs along the b-file and the sixth rank and has to end on the f-file:

```
32...b6a5!
33.h8:f3   f1:a4     (h8:b8:b6:f6:f3, four men; then f1:f5:c5:c2:a2:a4, five pieces)
```

The black king takes the white king and four men. White has two men left. Engine: 0.23. Black won both play-outs. (The white king can stop on f2 or f4 instead of f3; the result is the same.)

**Diagr.14: 34...b6a5!** 35.a8:a4 (forced) f5g4 36.a4:h4 (forced) h8:a8 (h8:h2:a2:a8). Engine: 0.13. Black won the play-out.

White's mistakes are 32.h6h8 (0.24), where 32.c3c4 keeps the game level (1.00), and before that 30.h4h6! (1.00), where 30.d3e4 is better (1.38).

### 4. Diagr.12, lines A and B

Both lines end with 30.h5g6 f6:h6 31.f4:d6 and "2:0". In both, Black then has 31...b5c4! 32.c3:c5 (forced) b7c6 33.c5:c7 (forced) c8:e6, which leaves four men each.

- **A)** 28...c6c5? 29.a3a4 c5:e5 30.h5g6 f6:h6 31.f4:d6 — 1.02, and Black won the play-out. White wins with **29.d3c4!** c5:e5 30.h5g6 (1.47; White won the play-out).
- **B)** 28...e6e5? 29.d5d6! c6:e6 30.h5g6 f6:h6 31.f4:d6 — 0.99, and the play-out was drawn. White is better after **29.d5e6** e5:e7 (1.19; White won the play-out).

So the "?" marks on 28...c6c5 and 28...e6e5 can stay, but the refutations need changing.

Line D is right as it stands: 29.h5g6! is the correct order (1.23), and 29.a3a4? loses to 29...b5b3!! against either capture (0.18 and 0.22).

At Diagr.12 itself the engine's first choice for Black is 28...c8a6 (1.07 at 50k), which the chapter does not consider.

### 5. Diagr.9: 26.d7e8 loses

The book says that after 24.d4d6! Black "has three capture options, but none of them offers any relief".

- **Option 2**, 24...e5:e3 25.d5d7! e6:c6 26.d7e8, is met by **26...f7f4!** 27.e8:e1 f4:h2 (f4:f2:h2). The king has to take on the e-file and can only choose between e1 and e2. White has a king and five men against nine men. Engine: 0.37. Black won both play-outs. The book's 26...f5e4 does lose (1.77). White's move is **26.f3e4!** e3:e5 (forced) 27.d7e8 (1.13; White won the play-out).
- **Option 1**, 24...e5:c5 25.d6d7, is given as "2:0" without a further move. The engine has 0.98 there. White did win both play-outs, so the verdict may be right, but it is not immediate.
- **The game move**, 24...e6:c6, is 1.07, and White won that play-out too.

So the engine's numbers for all three captures are close to level, while its play-outs all went White's way. "Overwhelming" is more than it can confirm, but it found no defence either.

### 6. Diagr.6: 18.d3c4

The book says 18.d3c4 c5:c3 19.c2:c4 would give White "an overwhelming advantage" and that the game's 18.c1c3, played by a technical error, is weaker. The engine has 1.22 after the first and 1.24 after the second, and Black won the play-out after 18.d3c4.

In the same way it has Diagr.7 at 1.18, a moderate edge, and the play-out from it was drawn. The description of Black's difficulties there is a fair account of how the position feels, but the engine does not see it as close to lost.

## Smaller points

- **11...b6:b8** (1.19). The other capture, 11...a4:a2, is 1.07.
- **5.h3h5** (0.85) and **5...f6e5** (0.98) each cost about 0.1 against 5.b2a3 and 5...e7g5.
- **9...c6a4?** costs little by the engine's count (1.06 against 0.99 for 9...f7d5), although 10.h4g5! is certainly the best reply.
- **15.a2b3!**, **19.c2c4** and **21.e3e4** are each about 0.1 below the engine's choice (15.f3e4 1.32, 19.b2b3 1.28, 21.c3c4 1.21).
- **26...b8b5** (1.20). The engine prefers 26...b6a5 (1.08).
- **27.g3f4** (1.06). The engine prefers 27.a3a4 (1.21).
- **Diagr.11.** 27...f7e6 (1.05) is the engine's first choice, with the game's 27...b7b4 about equal to it (finding 2). The next move, 27...f5e4, is 1.17 at 50k.

## Confirmed by the engine

- 10.h4g5! wins a man.
- 13.d2d4!: "Black cannot immediately play h7h5" because of 14.f2c5 (1.46).
- 13...b5b4!: 14.c2b3 is met by 14...f8f6 (0.76).
- 14.f2f3!: 14...h7h5 loses a man to 15.e3f4 (1.51).
- 23.h1d5? f5g4!! 24.g3:g5 e7e5 25.d5:f5 f6:h2 wins for Black (0.27; Black won the play-out), and 23...e7e5? misses it (1.03).
- 26.g2g4 is the engine's move too.
- Diagr.12, line D: 29.h5g6! is right, and 29.a3a4? loses to 29...b5b3!! (Diagr.15 and Diagr.16).
- Diagr.20 and Diagr.22 are won for White (1.80 and 1.49; White won both play-outs).
- 28.d5e6 loses (0.33). 29...b5b3!! is the only move: 29...b6b3 loses as shown (1.63).
- The final position is won for Black (0.28; Black won both play-outs).

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 335 | The label reads `Diagr.18`, without the space every other label has | `Diagr. 18` |
| 360 | "return to Diagram 18:" is followed by Diagr. 17, which is the position meant | "return to Diagram 17:" |
| 65–67 | The comment on 12.b1a2 | See finding 1 |
| 100–102 | 18.d3c4 | See finding 6 |
| 174–178 | Option 2 at Diagr.9 | See finding 5 |
| 224–260 | Lines A, B and C at Diagr.12 | See findings 3 and 4 |
| 326–358, 398 | The verdict on 28.a3a4 | See finding 2 |

The move 27...b7b4?! is printed twice as a main-line move (lines 315 and 396). That is a deliberate restatement after the analysis, not an error.

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | e1g3 | 1.00 | 1.00 | g7h6 | 0.99 | 1.00 |  |
| 2 | g2h3 | 0.98 | 0.98 | b8b6 | 0.98 | 0.98 |  |
| 3 | d1b3 | 0.94 | 0.94 | d8a5 | 0.93 | 0.95 |  |
| 4 | f2h4 | 0.92 | 0.94 | g8h7 | 0.93 | 0.96 |  |
| 5 | h3h5 | 0.86 | 0.85 | f6e5 | 0.98 | 0.98 | 5.b2a3 0.97; 5...e7g5 0.85 |
| 6 | e2g4 | 0.95 | 0.96 | b7b5 | 0.95 | 0.96 |  |
| 7 | b3b4 | 0.98 | 0.97 | b5:b3 | 0.98 | 0.97 |  |
| 8 | c3:a3 | 0.99 | 0.97 | d7b5 | 1.01 | 1.01 |  |
| 9 | g1f2! | 1.00 | 0.99 | c6a4? | 1.00 | 1.06 |  |
| 10 | h4g5! | 1.02 | 1.08 | h6:f4 | 1.02 | 1.08 |  |
| 11 | f3:b7 | 1.02 | 1.08 | b6:b8 | 1.03 | 1.19 | 11...a4:a2 1.07 |
| 12 | b1a2 | 0.80 | 0.74 | h8h6 | 1.23 | 1.25 | 12.a1c3 1.22; 12...a5b4 0.73 |
| 13 | d2d4! | 1.24 | 1.26 | b5b4! | 1.23 | 1.26 |  |
| 14 | f2f3! | 1.25 | 1.26 | a8b7 | 1.28 | 1.31 |  |
| 15 | a2b3! | 1.19 | 1.23 | a4:a2 | 1.19 | 1.23 | 15.f3e4 1.32 |
| 16 | a1:a3 | 1.18 | 1.16 | e7d6 | 1.15 | 1.23 | 16...e6f5 1.15 |
| 17 | b3:b5 | 1.15 | 1.23 | a5:c5 | 1.15 | 1.23 |  |
| 18 | c1c3 | 1.18 | 1.24 | h7h5 | 1.26 | 1.25 |  |
| 19 | c2c4 | 1.16 | 1.18 | h5:f5 | 1.16 | 1.18 | 19.b2b3 1.28 |
| 20 | c4:c6 | 1.16 | 1.18 | d6:b6 | 1.16 | 1.18 |  |
| 21 | e3e4 | 1.12 | 1.11 | f8f6 | 1.11 | 1.11 | 21.c3c4 1.21 |
| 22 | f1g2 | 1.03 | 1.07 | e8e7 | 1.02 | 1.11 |  |
| 23 | h1d5? | 0.94 | 0.32 | e7e5? | 1.10 | 1.03 | 23.b2b3 1.12; 23...f5g4 0.29 |
| 24 | d4d6! | 0.94 | 0.97 | e6:c6 | 1.04 | 1.07 | 24...e5:c5 0.98 (finding 5) |
| 25 | e4:g6 | 1.04 | 1.07 | h6:f6 | 1.04 | 1.07 |  |
| 26 | g2g4 | 1.12 | 1.08 | b8b5 | 1.10 | 1.20 | 26...b6a5 1.08 |
| 27 | g3f4 | 1.12 | 1.06 | b7b4?! | 1.19 | 1.35 | 27.a3a4 1.21; for Black see finding 2 |
| 28 | d5e6 | 0.42 | 0.33 | f6:d6 | 0.42 | 0.33 | 28.a3a4, about level (finding 2) |
| 29 | f4:f8 | 0.42 | 0.33 | b5b3!! | 0.38 | 0.32 |  |
| 30 | f8:b5 | 0.38 | 0.31 | b3:b1 | 0.37 | 0.31 |  |
| 31 | b5:b2 | 0.36 | 0.29 | b1:h4 | 0.35 | 0.28 |  |
| 32 | a3a4 | 0.37 | 0.29 | h4d8 | 0.34 | 0.28 |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw. The 1.35 after 27...b7b4 is the search's reading before it has seen 28.a3a4 b5b3; with that line the position is about level.
