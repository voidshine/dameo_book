# Game 4 — engine review of `Game04.md`

Kirin vs Beginner_2017, 2:0. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

The game record is sound. The main line matches `Game04.agt` move for move, every move is legal, and all 33 diagrams show the positions they should. Two moves in the variations are written with the wrong starting square (see the errata).

Much of the chapter is confirmed: 21.f3f4!! is the best move, the long ending after it is played accurately by White from move 32, the two-kings-against-one technique is right, and the ending at Diagr.11 is won for Black.

The engine's disagreements are about the turning points. The most important is that it does not accept the chapter's conclusion about its own central idea:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | 16...g6g5!? and 18...e6g4! | The idea "did not lead me to success. I should have played more simply" | The idea is right, and 16...g6g5 is the best move. The slip is 18...e6g4: 18...e6e4! does the same thing and takes one man more | Missed opportunity (Black), incorrect assessment |
| 2 | Diagr.6: 18.h1g2! | White's key move; Black must find 18...f5e4! and 19...e6d5! to survive | 18.h1g2 loses outright to 18...g6e4! | Error (analysis) |
| 3 | Continuation 2: 21.h3h5! | White "should play decisively" | 21.h3h5 loses to 21...a6a5!. The move is 21.c6b7!, and the two lines the book dismisses are good for White | Error (analysis) |
| 4 | 26.d1a4 and 27...b2a1 | "It makes little difference with accurate play" | 26.c6c7! wins; 26.d1a4 lets Black hold with 27...a7a4! | Missed opportunity (both), incorrect assessment |
| 5 | 30.c6b7? | Loses to the column | Only 31.b7a8? loses. After 31.b7c8! White keeps the advantage | Incorrect assessment |
| 6 | Continuation 1: 22.b1a2! | Wins, 2:0 | 22.h6h7! is the convincing move; the 22.b1a2 line was drawn in the play-out | Incorrect analysis |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations. A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it.** The engine often misses a sacrifice until it is on the board, so the alternatives were checked one by one, and each finding rests on a line I checked move by move. Differences under about 0.08 mean nothing.

## Findings in detail

### 1. The idea was right: 18...e6e4!

The chapter ends: "My idea with the move 16...g6g5 did not lead me to success. I should have played more simply."

The engine thinks the opposite about move 16. Of Black's 25 moves at Diagr.5 it rates **16...g6g5** best (0.65), ahead of 16...h8h7 (0.88) and 16...h6g5 (0.99, 50k).

The slip comes two moves later. After 17.h5:h7 h8:h6 18.g4:g6 the book plays 18...e6g4!, "the crux of Black's idea". The e6 man can vault the other way:

| | 18th move | White | Black | Position after | Engine |
|:--|:--|:--|:--|:--|--:|
| Game | 18...e6g4 | 19.g6:c6 | 19...g4:g2 (one man) | 20.f4:f6: 10 men v 8 | 1.28 |
| Engine | **18...e6e4!** | 19.g6:c6 (forced) | 19...e4:g2 (e4:g4:g2, two men, forced) | 9 men v 9 | 0.64 |

From e4 the man takes f4 on its way to g2, so White never gets 20.f4:f6. Black reaches the same position on g2 a man better off and with White's f6 wedge gone.

It is not a forced win. The standard play-out was drawn, and the wide play-out was lost after a later slip by Black. But it is the difference between a worse game and a better one.

### 2. Diagr.6: 18.h1g2! loses to 18...g6e4!

In the line 16...h8h7 17.g3g5 f8g7 the book gives 18.h1g2! and studies 18...d7d5? and 18...f5e4!. Black has a third move:

```
18.h1g2?   g6e4!
19.f4:d4   d6f4!     (19.f4:d4 is forced)
20.g4:e4   e5:e1     (forced; e5:e3:g3:g1:e1, four men, and a king)
21.g5:c7   e1:g4     (g5:e5:e7:c7 forced; e1:a1:a4:g4, three men)
22.h4:f4   h6:h2     (both forced; h6:h4:h2)
```

White has three men left against seven. Engine: 0.20 after 18...g6e4. Black won both play-outs.

So Diagr.6 is not a position where "the strong connection on the right flank clearly favours White". The engine has it at 0.88, slightly in Black's favour, and White's sensible 18th moves are quiet ones on the other wing (18.b1c2 0.81, 18.b3a4, 18.a3b4).

The long line from 18...f5e4! to Diagr.16 is still worth having, and its verdict stands: Diagr.11 is won for Black (0.30; Black won both play-outs). The engine's corrections inside it are small:

- 25.f3f5! (0.42) is weaker than 25.f2e3 (0.68).
- 33...c1h6 (0.51) is weaker than 33...a7a5 (0.30), and 35.b3a4 (0.35) is weaker than 35.g1a1 (0.64).

### 3. Continuation 2: 21.h3h5! loses to 21...a6a5!

After 20...f8g7 the book rejects 21.f6e7 and 21.c6c7 and recommends 21.h3h5!. The engine reverses all three.

**21.h3h5?** (0.97) allows a combination:

```
21...a6a5!   22.h5:f7     (h5:h7:f7, forced)
22...a5b4    23.b3:b5     (forced)
23...d7e6    24.f6:d6     (forced)
24...g2g1
```

Black has four pieces against ten, but the king cannot be caught and the white men cannot be held. Engine: 0.28. Black won both play-outs.

**21.c6b7!** a7:c7 (forced) 22.h1h2 is the engine's move (1.36). White won both play-outs.

The dismissed lines are also good for White: 21.f6e7 d7:f7 22.c6d7 e5d4 is 1.42, and 21.c6c7 d7:b7 22.f6e7 e5d4 is 1.51.

The rest of the line has slips on both sides (25...a8a5 is 1.35 where 25...d7d6 is 0.76; 26.e8d8 is 0.90 where 26.e8b8 is 1.35), but its end is right: Diagr.25 is won for White (1.72).

On the wider question, 20...f8g7 (1.33) and the game's 20...g2g1 (1.29) are equal. Neither was "probably Black's best chance" over the other.

### 4. 26.d1a4 and 27...b2a1

At Diagr.28 the book reasons schematically and says of 26.d1a4 that "it makes little difference with accurate play". The engine sees a large difference.

- **26.c6c7!** wins: 1.58, and White won both play-outs. The book's own scheme (king to f1, exchange on g1, second king to control h1) works with a tempo more.
- **26.d1a4** drops to 0.99. After 26...a3b2 27.a4d1 Black can play **27...a7a4!** (1.00), bringing the column up before kinging. Both play-outs were drawn.
- The game's **27...b2a1** (1.31) puts White back on top.

`Game04.agt` contains a 26.c6c7 line that did not make it into the chapter.

### 5. 30.c6b7 is not a mistake

The book gives 30.c6b7? a5a3! 31.b7a8 a4a2! 32.a8:a5 a2a1 33.a5:a2 a1:e1, 0:2. The line is correct as far as it goes: after 32...a2a1 the white king's only legal move is to take on a3, and the black king then takes all four white pieces.

But 31.b7a8 is the losing move. After **31.b7c8!** White keeps the advantage (1.36), and the engine rates 30.c6b7 (1.34) the same as the game's 30.c6d7! (1.33).

The move number 33 has a slip in it; see the errata.

### 6. Continuation 1: 22.h6h7!

After 20...h6h5 21.h4:h6 g2g1 the book is right that 22.c6c7 and 22.f6e7 lose (0.15 and 0.17). It then gives 22.b1a2! g1:g6 23.h6:f6 and a line to Diagr.21, "2:0".

The engine prefers **22.h6h7!** (1.58; White won the play-out) to 22.b1a2 (1.33). Diagr.21 itself is 1.28, and the play-out from it was drawn. So the conclusion that 20...h6h5 does not save Black is right, but the convincing reason is 22.h6h7.

## Smaller points

- **22...a8a5** (1.54) is weaker than 22...f8g7 (1.29), though White still won that play-out.
- **31...a5a2** (1.69) is weaker than 31...a4a2 (1.49); the play-out after 31...a4a2 was drawn.
- **13...c8e6** (1.00) and **15.e3g5!** (0.93) each cost about 0.08 against 13...d7c6 and 15.e2e4. Both are within the range where the engine's opinion is weak.

## Confirmed by the engine

- 17.g4:g6 "simply leads to a permutation": both captures are 0.65.
- 19.g6:c6 is the right capture (19.g6:e4 is 0.46).
- 21.f3f4!! is the best move (1.30). White won the play-out.
- "After 21.f6g7 g1:a5 22.g7h8 d7e6 ... Black gets a difficult but advantageous endgame": 0.39, and Black won the play-out.
- In the Diagr.10 note, 30.f5e6 h2h1 31.e6e8 h1h2! is good for Black (0.30).
- In the Diagr.11 note, 34.b3a4? c6c8! wins for Black (0.08).
- Diagr.15 and Diagr.16 are won for Black (0.26 and 0.17), and so is 44.a7a8 b3b2 (0.13).
- Diagr.25 and Diagr.26 are won for White (1.72 and 1.79), and 30...f8h8 does not save Black (1.84).
- Diagr.30: "Black is losing" (1.70; White won the play-out).
- At Diagr.33, 46.h8b2 d5d1 47.a8a1 and 46...d5h1 47.b2b7 both win (1.99 and 1.97).

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 239 | `42.g7a7`: the white king is on h7 after 41.h6h7 | `42.h7a7` |
| 489 | `33.a8:a2`: the king went to a5 on move 32 | `33.a5:a2` |
| 99–145 | The 18.h1g2! lines | See finding 2 |
| 362 | `21.h3h5!` | See finding 3 |
| 466–467 | "It makes little difference with accurate play" | See finding 4 |
| 568 | The closing verdict on 16...g6g5 | See finding 1 |

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | c2b3 | 0.97 | 0.98 | e6e5 | 1.02 | 1.03 |  |
| 2 | d3e4 | 0.96 | 0.96 | g8g6 | 1.00 | 1.00 |  |
| 3 | e4:e6 | 1.00 | 1.00 | e7:e5 | 1.00 | 1.00 |  |
| 4 | e1g3 | 0.95 | 0.93 | e5e4 | 0.91 | 0.91 | 4.b3c4 1.01 |
| 5 | e3:e5 | 0.91 | 0.91 | d8d5 | 0.88 | 0.90 |  |
| 6 | e5:c5 | 0.88 | 0.90 | c6:a2 | 0.88 | 0.90 |  |
| 7 | a1:a3 | 0.88 | 0.90 | b8a7 | 0.89 | 0.91 |  |
| 8 | f2h4 | 0.91 | 0.92 | g7h6 | 0.90 | 0.92 |  |
| 9 | d1g4 | 0.91 | 0.91 | e8h5 | 0.90 | 0.91 |  |
| 10 | g1h2 | 0.92 | 0.91 | h5:h3 | 0.92 | 0.91 |  |
| 11 | h2:h4 | 0.92 | 0.91 | f7h5 | 0.92 | 0.93 |  |
| 12 | g2h3 | 0.93 | 0.93 | b7a6 | 0.92 | 0.93 |  |
| 13 | c1e3!? | 0.93 | 0.92 | c8e6 | 1.01 | 1.00 | 13...d7c6 0.92 |
| 14 | d2f4 | 1.03 | 1.01 | c7e5 | 1.01 | 1.00 |  |
| 15 | e3g5! | 1.01 | 0.93 | h5:f5 | 1.01 | 0.93 | 15.e2e4 1.02 |
| 16 | e2h5 | 1.01 | 0.93 | g6g5!? | 0.70 | 0.65 |  |
| 17 | h5:h7 | 0.70 | 0.65 | h8:h6 | 0.70 | 0.65 |  |
| 18 | g4:g6 | 0.70 | 0.65 | e6g4! | 1.15 | 1.28 | 18...e6e4 0.64 |
| 19 | g6:c6 | 1.17 | 1.28 | g4:g2 | 1.16 | 1.28 |  |
| 20 | f4:f6 | 1.16 | 1.28 | g2g1 | 1.17 | 1.29 |  |
| 21 | f3f4!! | 1.27 | 1.30 | g1:g4 | 1.27 | 1.30 |  |
| 22 | h4:f4 | 1.28 | 1.30 | a8a5 | 1.51 | 1.54 | 22...f8g7 1.29 |
| 23 | f6g7 | 1.52 | 1.54 | e5d4 | 1.50 | 1.54 |  |
| 24 | g7h8 | 1.23 | 1.56 | d4c3 | 1.21 | 1.56 |  |
| 25 | h8:d1 | 1.31 | 1.57 | c3:a3 | 1.31 | 1.57 |  |
| 26 | d1a4 | 1.00 | 0.99 | a3b2 | 0.97 | 0.98 | 26.c6c7 1.58 |
| 27 | a4d1 | 0.85 | 1.00 | b2a1 | 1.08 | 1.31 | 27...a7a4 1.00 |
| 28 | d1f1 | 1.15 | 1.32 | a1:g1 | 1.15 | 1.32 |  |
| 29 | h1:f1 | 1.15 | 1.32 | a7a4 | 1.19 | 1.34 |  |
| 30 | c6d7! | 1.22 | 1.33 | a6a3 | 1.23 | 1.39 |  |
| 31 | d7e8 | 1.26 | 1.39 | a5a2 | 1.41 | 1.69 | 31...a4a2 1.49 |
| 32 | e8e4 | 1.43 | 1.70 | h6h5 | 1.68 | 1.77 |  |
| 33 | e4d5 | 1.67 | 1.77 | a4a1 | 1.78 | 1.80 |  |
| 34 | d5h1 | 1.79 | 1.80 | a1:g1 | 1.79 | 1.81 |  |
| 35 | h1:a1 | 1.80 | 1.81 | a3b2 | 1.79 | 1.81 |  |
| 36 | a1:a8 | 1.80 | 1.82 | b2a1 | 1.82 | 1.83 |  |
| 37 | a8h8 | 1.84 | 1.84 | a1f1 | 1.82 | 1.84 |  |
| 38 | h8:h4 | 1.83 | 1.84 | f1:f6 | 1.84 | 1.85 |  |
| 39 | h4h8 | 1.85 | 1.86 | f6g7 | 1.85 | 1.86 |  |
| 40 | h3h4 | 1.86 | 1.87 | g7f6 | 1.88 | 1.88 |  |
| 41 | h4h5 | 1.87 | 1.88 | f6g7 | 1.86 | 1.88 |  |
| 42 | h8a8 | 1.89 | 1.89 | g7b7 | 1.89 | 1.89 |  |
| 43 | h5h6 | 1.90 | 1.90 | b7d5 | 1.90 | 1.91 |  |
| 44 | h6h7 | 1.91 | 1.92 | d5e4 | 1.92 | 1.93 |  |
| 45 | h7h8 | 1.93 | 1.94 | e4d5 | 1.94 | 1.95 |  |
| 46 | h8b2 | 1.95 | 1.96 | d5e5 | 1.96 | 1.99 |  |
| 47 | b2b5 | 1.99 | 2.00 | e5:a5 | 2.00 | 2.00 |  |
| 48 | a8:a1 | 2.00 | 2.00 |  |  |  |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw.
