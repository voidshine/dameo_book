# Game 6 — engine review of `Game06.md`

Kirin vs Beginner_2017, 0:2. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

The game itself is annotated correctly from start to finish. The main line matches `Game06.agt` move for move, every move is legal, and all 30 diagrams show the positions they should. The engine confirms each step of the story: 10...b6b4 walks into 11.c4d5! and 12.e3f4!, 15.c1c2 is the only move, "THREAT No. 1" is real, 16.a1b2? loses to it, and Black's play from 17...h6g5! to the end is the best available.

The chapter's weight is in the three long analyses from Diagr.5, and here the book does better than the engine does unaided. The engine cannot see THREAT No. 1 from a distance: its own first choice at Diagr.5 is 16.h3h4, one of the "neutral" moves the book says lose, and it only notices when the combination is on the board. The book's list of moves that fail is right.

Inside the three continuations the engine finds one conclusion that does not hold and several places where a stronger move changes the line:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | Continuation g3g5, Diagr.26–28 | 43.f3h5 f1h3! 44.h5g6! and a draw | 43...f1f8! wins at once, and Diagr.26 is lost for White whatever he plays. White has to improve at move 38 | Error (analysis) |
| 2 | Continuation e2e3, Diagr.11–13 | A 20-move line shows White's game is the harder one | Black has a stronger shot twice (20...a6c4!, 25...a6c4!), and White has 23.h4f6! in between. The line is not best play for either side | Omission |
| 3 | Continuation e2d3, Diagr.7–9 | "Only Black has a chance of winning"; 28...c8d8! "solves the matter" | Diagr.7 is level, and 28...c8d8 is answered by 29.a3a4! | Incorrect assessment |
| 4 | Diagr.5 | Three moves meet the threat: e2d3, e2e3, g3g5 | 16.d2e3 and 16.h3f5 seem to meet it too (low confidence) | Omission |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations. A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it.** In this chapter, only at the end of a line. The combinations here are five to seven moves deep with quiet moves in them, and the engine regularly rates the first move of one as a mistake and then agrees once the last move is played. After 16.h3h4, for instance, it reads 1.18, and after 17...b5b4 in the same line it reads 0.35. So I have used its numbers for positions where the tactics are finished, and I report a correction only where it gives a concrete move that I then checked.

## Findings in detail

### 1. Continuation g3g5: the ending is lost, not drawn

The line runs to Diagr.26 (after 42...e2f1) and continues 43.f3h5 f1h3! 44.h5g6! h3h4 45.h8:a2 a1:h7 46.g4g5, a draw at Diagr.28. That last position is indeed drawn (1.01).

But at move 43 Black can give up a king to deflect the white one:

```
43.f3h5   f1f8!
44.h8:b1  a1:g8    (h8:b8:b1 is forced; the black king then takes b1, g4 and g7)
```

White is left with the single man on h5 against a king. Engine: 0.04. Both play-outs ended in four more moves.

The moves 43.g7f8 and 43.g7g8 lose as the book says (0.02 each), and the engine's best try, 43.h8h1, loses as well (0.20; Black won both play-outs). So Diagr.26 is simply lost (0.19), and the draw depends on 43...f1h3.

White's score falls in two steps before that:

- **38.e2h5** (0.81). The engine plays 38.d2e3 at once (1.22), the move the book plays a turn later.
- **40.h5h6!** (0.31). The alternative 40.e3f4 is 0.77, though Black won that play-out too.

Up to move 37 the line stands as written, with White a little better (Diagr.20 is 1.27, Diagr.23 is 1.31). All the side notes along the way are right; they are listed under "Confirmed".

### 2. Continuation e2e3: stronger moves for both sides

The book offers this line to show that "White has a more difficult game than Black", and ends it with a win for Black at Diagr.17. That final position is won (0.17). On the way there the engine changes three moves.

- **Diagr.11, after 20.h3h4!** The book plays 20...c8c6 (1.43). The engine plays **20...a6c4!** (0.73): White must take, by 21.c3:c5 or 21.a4:a6, and after 21.c3:c5 e5c3 22.c5:e7 c3b2 a black man is through.
- **Diagr.12.** The book's 23.g4g6 (1.31) is weaker than **23.h4f6!** h6:f4 (forced) 24.f6:b4 (1.55). The next two moves, 24.g5h6 (0.94) and 25.h4h7 (0.60), give away the rest; 24.e2e4 is 1.33.
- **Diagr.13.** 25...e6e4!! is good (0.59), but **25...a6c4!** is better (0.28): 26.h7:f5 (forced) c4:c2.

So the line is a fair picture of the dangers, but with best play White is the one pressing at Diagr.12 (1.40), and Black had a quicker road at Diagr.11.

The engine also prefers 17...e8g6 (1.04) to 17...e8e6! (1.21) at the start, but given what is said above about trusting it early in a line, I would not act on that.

### 3. Continuation e2d3: level, and 28...c8d8 is not decisive

The combination 17...f7f6!! 18.d3:d5 b5c4!! 19.c3:c5 a5b4! is sound, and the engine does not find it by itself.

- **Diagr.7** is 1.03. Both play-outs were drawn. "Only Black has a chance of winning" is stronger than that.
- **28...c8d8!** is said to solve the matter after 29.f3h5 d8:d1 30.h3h6 d1d7 (Diagr.9, 0.61; Black won both play-outs from there). But White has **29.a3a4!** d8:d1 30.a4:c4 (1.30).
- One move earlier, **27...b5c4** (0.84) is better than 27...b5b4 (1.07).

### 4. Diagr.5: two more moves that meet the threat

The book names e2d3, e2e3 and g3g5. I checked every legal 16th move by playing the forced 16...c1:g1 17.h1:f1 and then letting `damleo eval` (50k) judge each black reply:

| 16th move | e2d3 | e2e3 | d2e3 | g3g5 | h3f5 | a1a2 | f3f4 | a1b2 | h3h4 |
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| After Black's best reply | 1.14 | 1.01 | 0.96 | 0.90 | 0.89 | 0.65 | 0.56 | 0.51 | 0.40 |

The three book moves hold, and the neutral moves the book mentions (h3h4, f3f4, a1a2) fail, as does the game's 16.a1b2. The two additions are **16.d2e3** and **16.h3f5**. Since this is a shallow check in a position where the engine is easily fooled, treat them as candidates to look at, not as corrections.

## Smaller points

- **13.h4g5** (1.17). The engine slightly prefers 13.g3g5 (1.26).
- **Continuation g3g5, move 22.** The book calls 22...c6c5 "very risky" and recommends 22...d7d6. The engine rates them the same or the other way round (1.13 and 1.25), and White won the play-outs after both. The book's view that Black is in difficulty either way is confirmed.
- **Continuation g3g5, move 36.** 36.e2g4 (1.35) is a little better than 36.h3g4 (1.20).

## Confirmed by the engine

In the game:

- 10...b6b4 is the mistake that allows the combination (1.18; 10...e8g6 is 0.99), and 11.c4d5! and 12.e3f4! are the best moves.
- 15.c1c2 is the only move: every other move is 0.82 or worse at 50k.
- 16.a1b2? (0.75, and 0.30 once 17...h6g5! is played). Diagr.29 is won for Black (0.26; Black won the play-out).

In Continuation e2d3:

- 23.f1e2! is the best move at Diagr.7; after 23.f1f2 or 23.f1g2 the reply c1b2 is strong (0.66).

In Continuation e2e3:

- 18.c3c4 d4:b4 19.a4:c4 a6a4 20.a1a2 c7b6 favours Black slightly (0.93).
- 19.e2d3? b5c4! 20.c3:e5 e6:e2 is good for Black (0.47).
- At Diagr.14, 28.d2:b2 is the only capture that fights (0.58); the other three are 0.26 or worse.
- 31.b2c3! is the best move at Diagr.15; 31.f5g6 loses (0.19).
- At Diagr.17, 39.h7h8 a7h7 (0.13) and 39.h7g8 h1h8 (0.20) both lose.

In Continuation g3g5:

- 28...f5e4? 29.h6h7! h8:h6 30.b4b5! c5:a5 31.a7a8 is good for White (1.06).
- 32.a6a8? b2a1 33.a8:g8 a1:a8! 34.g8h7 f7g6 is good for Black (0.28).
- 37.a8f8? a1a8 loses (0.09).
- All three notes at Diagr.24 are right: 39.h8g8?? a3b2 (0.04), 39.h8h6?? f5f4! (0.09), and 39.h5h6? a1d1 40.h6:f4 d1:g7! (0.22 at the end of the line).
- 41.e3:e5?? a1e1 loses (0.11).
- 44.h8h6?! b2b1 is about level (1.02), and 44.h5g6! is the best move after 43...f1h3.

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 479–501 | The end of Continuation g3g5 | See finding 1 |

Two things in `Game06.agt`, not in the chapter: the annotation on 9.d3c4 is stored as `!?s`, and diagram number 17 is attached to two sibling moves (38...g2h1 and 38...g2f1). The chapter and its image use 38...g2h1 and agree with each other.

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | e1g3 | 1.00 | 1.00 | g8h7 | 0.99 | 1.00 |  |
| 2 | f2h4 | 1.00 | 1.00 | g7h6 | 1.00 | 1.00 |  |
| 3 | g1g4 | 1.00 | 1.00 | h6g5 | 1.02 | 1.01 |  |
| 4 | g4:g6 | 1.02 | 1.01 | f6:h6 | 1.02 | 1.01 |  |
| 5 | g2h3 | 1.01 | 1.00 | h7h5 | 1.07 | 1.06 |  |
| 6 | d1g4 | 1.07 | 1.05 | b8b6 | 1.09 | 1.08 |  |
| 7 | b1b3 | 1.01 | 1.01 | d8a5 | 1.00 | 1.01 |  |
| 8 | b2a3 | 1.00 | 1.01 | b7a6 | 1.00 | 1.01 |  |
| 9 | d3c4!? | 0.95 | 0.99 | c6b5 | 0.95 | 0.99 |  |
| 10 | c2a4 | 0.96 | 0.99 | b6b4 | 1.00 | 1.18 | 10...e8g6 0.99 |
| 11 | c4d5! | 1.07 | 1.19 | b4:b2 | 1.06 | 1.20 |  |
| 12 | e3f4! | 1.23 | 1.25 | d6:d4 | 1.23 | 1.25 |  |
| 13 | h4g5 | 1.21 | 1.17 | h5:f5 | 1.22 | 1.17 | 13.g3g5 1.26 |
| 14 | f4:b8 | 1.22 | 1.17 | a8:c8 | 1.22 | 1.17 |  |
| 15 | c1c2 | 1.24 | 1.18 | b2c1 | 1.26 | 1.17 |  |
| 16 | a1b2? | 0.92 | 0.75 | c1:g1 | 0.92 | 0.75 | 16.e2d3 1.17 (see finding 4) |
| 17 | h1:f1 | 0.92 | 0.74 | h6g5! | 0.51 | 0.30 |  |
| 18 | g4:g6 | 0.51 | 0.30 | b5b4 | 0.31 | 0.28 |  |
| 19 | a4:e4 | 0.31 | 0.28 | f7f6 | 0.28 | 0.27 |  |
| 20 | g6:e6 | 0.27 | 0.27 | e7:g1 | 0.27 | 0.26 |  |
| 21 | f3g4 | 0.21 | 0.21 | c8c6 | 0.25 | 0.25 |  |
| 22 | h3f5 | 0.22 | 0.22 | g1a7 | 0.24 | 0.25 |  |
| 23 | g3h4 | 0.23 | 0.21 | a6a4 | 0.24 | 0.19 |  |
| 24 | a3b4 | 0.24 | 0.19 | a4:c4 | 0.24 | 0.19 |  |
| 25 | c3:c5 | 0.24 | 0.18 | c6:c4 | 0.23 | 0.18 |  |
| 26 | g4h5 | 0.17 | 0.11 | c4c3 | 0.12 | 0.08 |  |
| 27 | c2:c4 | 0.12 | 0.08 | a5b4 | 0.09 | 0.06 |  |
| 28 | c4:a4 | 0.08 | 0.06 | a7:f7 | 0.07 | 0.05 |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw. In the row for move 16 the search's own first choice was 16.h3h4 (1.18), which loses to the threat; the entry shown is the best move that survives the check in finding 4.
