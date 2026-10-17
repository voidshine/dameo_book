# Game 5 — engine review of `Game05.md`

Ray Garrison vs Beginner_2017, 0:2. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

This is the cleanest chapter so far. The main line matches `Game05.agt` move for move, every move is legal, and all 26 diagrams show the positions they should. The analysis is mostly confirmed as well: 20...g6f5!! is the best move, 25.c5c6! and 26...g6h7 are the only moves, the side lines at Diagr.8 and Diagr.12 are right, Diagr.22 is won for White, 46.b2b4?? is the blunder the book says it is, and the final ending is won for Black.

The engine disagrees on three points, all about where the game turned:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | 36.f8h8!! h2h1 | 36.f8h8 "saves White"; there is no win any more, and 36...h2h1 "does not change the evaluation" | Black still wins with 36...f6g5!. 36...h2h1 is the move that turns a winning game into a worse one | Missed opportunity (Black), incorrect assessment |
| 2 | 29.h3g4!! | "Ray saw one step ahead" and "has finally broken through" | The combination trades kings and leaves White worse (0.49). 29.d2d3 keeps the game about level | Incorrect assessment |
| 3 | 39...f4f3? | "A very, very heavy mistake"; 39...h8g8! draws by repetition | White was already better after move 36, and need not repeat. 39...f4f3 costs little more | Incorrect assessment |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations (1,000,000 where noted). A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it.** The engine is a self-taught network, not a tablebase, so each finding rests on a line I checked move by move. Differences under about 0.08 mean nothing.

## Findings in detail

### 1. 36...f6g5! still wins

At Diagr.18 the author "believed I was already winning", and the engine agrees: 0.31. After 35.a3b4 a4:c4 36.f8h8!! the book says the move "saves White" and lists three replies: 36...h5g4, 36...c4d3 and 36...h2h1.

There is a fourth. The white king is about to capture along h8–h4–a4–a6, and in the game it goes on from a6 to take the f6 man as well. Moving that man out of the way first costs nothing:

| | 36th move | White's capture | Then | Material | Engine |
|:--|:--|:--|:--|:--|--:|
| Game | 36...h2h1 | 37.h8:g6 (h8:h4:a4:a6:g6, four men) | 37...f5f4 | king and 3 men v king and 2 men | 1.30 |
| Engine | **36...f6g5!** | 37.h8:a6 (h8:h4:a4:a6, three men) | 37...h2h1 | king and 3 men v king and 3 men | 0.28 |

In the second line Black has two connected men on f5 and g5 ready to run, and the white king is far away on a6. Black won all three play-outs (standard, wide, and standard at 1,000,000 simulations).

So 36.f8h8 is White's best try (0.29; the next best is 0.18), but it does not save the game, and the sentence "This risky move does not change the evaluation of the position" is where the chapter goes wrong. 36...h2h1 is the decisive mistake.

The other two replies are as the book says: 36...h5g4 is 0.84 and 36...c4d3 37.h8:h1 d3:f3 38.h1g2 is 0.81 (the play-out was drawn).

### 2. 29.h3g4!!

After 29.h3g4 h5:h3 (forced) 30.g4h5 h6:h4 (forced) 31.h8:h6 White has given two men to capture the black king. He is left with a king and five men against eight men, two of which are already on h3 and h4.

The engine has 0.49 here, against 0.84 for the quiet 29.d2d3 (or 29.d2e3). Black won both play-outs after 29.h3g4; the play-out after 29.d2d3 was drawn. From move 29 to move 36 the score only moves further in Black's favour (0.49, 0.48, 0.36, 0.31, 0.29).

So the combination is real, but it is the reason White stood worse, not a breakthrough. The author's own feeling at Diagr.18 was the right one.

### 3. 39...f4f3? and the repetition

The book calls 39...f4f3 "a very, very heavy mistake" that is "already losing", and gives 39...h8g8! 40.g2h3 g8h8 with a draw by repetition.

- After 36...h2h1 White is already better: 1.30 at move 36, 1.37 before Black's 39th move.
- 39...h8g8 is the best move (1.36), and 39...f4f3 is 1.49. The difference is 0.13.
- White does not have to repeat. After 39...h8g8 he can go to the corner at once (40.g2h1), or after 40.g2h3 g8h8 41.h3g2 h8g8 play 42.g2h1. The standard play-out went that way and White won; the wide play-out was drawn.
- The plan f4g3 and g3h2 cannot be started at once: 39...f4g3 loses the man to 40.g2:g6 (1.78).

So the idea behind 39...h8g8 is right, but it was a defence of a worse position, not a route to an easy draw.

## Smaller points

- **Diagr.12, the 26...g6f5 line.** After 27.d2e3! the king can also go back with 27...f5h7 (0.93), which is close to the game. The three cases listed (the g-file, e5/d5, and b5) do not include it.
- **31...g5f4!** in that line is a trap, as the book says, and the trap works (32.h3g4?? f7f5! is 0.68). Objectively 31...e6f5 is better (1.25 against 1.41).
- **17.d2c3** (0.96) is a little weaker than 17.f1g2 (1.07), and **18...b5b3!** (1.03) a little weaker than 18...g7g5 (0.94).
- **32.h6f8** (0.36): 32.h6f4 is 0.48.
- **41...f3g2** (1.61) and **42...g8h8** (1.83) each make things worse; 41...g8f8 is 1.44 and 42...g8g4 is 1.62.

## Confirmed by the engine

- 6.e2c4, "extremely strange" to the author, costs nothing (1.06).
- 17...e5f4 would simplify in White's favour (Diagr.6, 1.15).
- At Diagr.8: 20...e6d5?? loses (1.72); 20...f7d5 leaves White solid (1.06); 20...a5c3 is 1.14; and 20...b3b2 21.c5c6! leads to Diagr.10 (1.23; White won the play-out).
- 20...g6f5!! is the best move (1.03), and 25.c5c6! is the only move that saves White (0.91; every other move is 0.55 or worse at 50k).
- At Diagr.12, 26...g6h7 is the best move (0.91); 26...g6e4 27.d2e3! loses (1.79); and 26...g6f5 27.d2e3 f5b5 28.c8a6! leads to a win for White (Diagr.15, 1.57; White won the play-out).
- At Diagr.18, 35.e3f4 (0.32) and 35.a3b4 a4:c4 36.e3f4 f5:f3 37.f8:a6 (0.16) do not save White.
- Diagr.22 is won for White (1.86). 45.a1b2 "loses one tempo, but victory is still within his hands" (1.81).
- At Diagr.23, 46.b3a4 was winning (1.81), and so was 46.b3c4 (1.83).
- 46.b2b4?? (0.23). Both traps work: 46...b5a4? 47.h1g1 (1.90) and 46...h8b8? 47.b4:b6 b8:b2 48.g7g8 (1.95). 46...h8g8! is the only move.
- Diagr.24 is won for Black (0.21; Black won the play-out), and so is the final position against both 54.b4a5 (0.08) and 54.b4c5 h8a8 (0.15).

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 301 | The label reads `Diagr. 20.` with a full stop; no other label has one | `Diagr. 20` |
| 410 | "a 2х1 winning endgame" has a Cyrillic х (U+0445) | Latin x |
| 410 | The result is written "0-2" | "0:2", as elsewhere |
| 264–296 | The verdict on 36.f8h8!! and 36...h2h1 | See finding 1 |
| 243–254 | The verdict on 29.h3g4!! | See finding 2 |

`Game05.agt` carries the main line four plies past the end of the game (54.b4a5 h8h1 55.a1a4 h1a1), as analysis. The chapter rightly stops at 53...g7b2.

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | g2h3 | 1.00 | 1.00 | d8b6 | 1.02 | 1.02 |  |
| 2 | g1h2 | 1.01 | 1.02 | f8h6 | 1.09 | 1.08 |  |
| 3 | b2a3 | 1.07 | 1.08 | c7a5 | 1.10 | 1.10 |  |
| 4 | e1g3 | 1.07 | 1.07 | g8g6 | 1.07 | 1.07 |  |
| 5 | g3h4 | 1.07 | 1.07 | e8h5 | 1.06 | 1.07 |  |
| 6 | e2c4 | 1.07 | 1.06 | b8b5 | 1.05 | 1.06 |  |
| 7 | a3b4 | 1.06 | 1.06 | b5:b3 | 1.06 | 1.06 |  |
| 8 | c3:a3 | 1.06 | 1.06 | b7b5 | 1.06 | 1.04 |  |
| 9 | b1a2 | 1.06 | 1.04 | c6a4 | 1.07 | 1.05 |  |
| 10 | e3d4 | 1.06 | 1.05 | b6b4 | 1.08 | 1.04 |  |
| 11 | a2b3 | 1.09 | 1.05 | b4:b2 | 1.07 | 1.04 |  |
| 12 | c2:a2 | 1.07 | 1.04 | e7c5 | 1.13 | 1.09 |  |
| 13 | c4:c6 | 1.13 | 1.09 | d6:b6 | 1.13 | 1.10 |  |
| 14 | f2e3 | 1.09 | 1.05 | d7c6 | 1.10 | 1.06 |  |
| 15 | f3e4 | 1.06 | 1.04 | h8e5 | 1.01 | 1.05 |  |
| 16 | d1c2 | 1.08 | 1.05 | c8c7 | 1.07 | 1.06 |  |
| 17 | d2c3 | 1.00 | 0.96 | b6b4 | 1.00 | 0.96 | 17.f1g2 1.07 |
| 18 | c1d2 | 1.02 | 0.94 | b5b3! | 1.02 | 1.03 | 18...g7g5 0.94 |
| 19 | d4c5 | 1.10 | 1.13 | c6:c4 | 1.10 | 1.13 |  |
| 20 | c3:c5 | 1.10 | 1.13 | g6f5!! | 1.47 | 1.03 |  |
| 21 | a3:c3 | 1.48 | 1.03 | b4b3 | 1.03 | 0.92 |  |
| 22 | c3:a3 | 1.03 | 0.92 | h5g4 | 0.96 | 0.91 |  |
| 23 | h4:f4 | 0.96 | 0.91 | f5:f3 | 0.96 | 0.91 |  |
| 24 | e3:g3 | 0.96 | 0.91 | e5:c1 | 0.96 | 0.91 |  |
| 25 | c5c6! | 0.98 | 0.91 | c1:g6 | 0.97 | 0.91 |  |
| 26 | c6:c8 | 0.97 | 0.91 | g6h7 | 0.97 | 0.91 |  |
| 27 | c8h8 | 0.99 | 0.91 | g7g6 | 0.96 | 0.90 |  |
| 28 | h2h4 | 0.90 | 0.84 | f7h5 | 0.85 | 0.84 |  |
| 29 | h3g4!! | 0.55 | 0.49 | h5:h3 | 0.55 | 0.49 | 29.d2d3 0.84 |
| 30 | g4h5 | 0.57 | 0.49 | h6:h4 | 0.57 | 0.48 |  |
| 31 | h8:h6 | 0.57 | 0.48 | g6h5 | 0.55 | 0.48 |  |
| 32 | h6f8 | 0.45 | 0.36 | e6f5 | 0.42 | 0.35 | 32.h6f4 0.48 |
| 33 | d2e3 | 0.39 | 0.32 | h3h2 | 0.36 | 0.31 |  |
| 34 | h1:h3 | 0.36 | 0.31 | h4:h2 | 0.36 | 0.31 |  |
| 35 | a3b4 | 0.37 | 0.29 | a4:c4 | 0.37 | 0.29 |  |
| 36 | f8h8!! | 0.39 | 0.29 | h2h1 | 1.30 | 1.30 | 36...f6g5 0.28 |
| 37 | h8:g6 | 1.32 | 1.31 | f5f4 | 1.30 | 1.30 |  |
| 38 | g6g2 | 1.27 | 1.33 | h1h8 | 1.30 | 1.36 |  |
| 39 | e3d4 | 1.33 | 1.37 | f4f3? | 1.45 | 1.49 | 39...h8g8 1.36 |
| 40 | d4e5 | 1.47 | 1.45 | h8g8 | 1.45 | 1.45 |  |
| 41 | g2h1 | 1.46 | 1.45 | f3g2 | 1.53 | 1.61 | 41...g8f8 1.44 |
| 42 | e5f6 | 1.54 | 1.61 | g8h8 | 1.78 | 1.83 | 42...g8g4 1.62 |
| 43 | f6g7 | 1.79 | 1.84 | a8b7 | 1.79 | 1.84 |  |
| 44 | a2b3 | 1.81 | 1.85 | b7a6 | 1.83 | 1.86 |  |
| 45 | a1b2 | 1.74 | 1.81 | a6b5 | 1.73 | 1.82 |  |
| 46 | b2b4?? | 0.33 | 0.23 | h8g8! | 0.29 | 0.22 | 46.b3c4 1.83 |
| 47 | b4:b6 | 0.28 | 0.22 | g8:a6 | 0.27 | 0.21 |  |
| 48 | h1a1 | 0.28 | 0.21 | a6f6 | 0.22 | 0.19 |  |
| 49 | a1h1 | 0.20 | 0.18 | f6a1 | 0.18 | 0.17 |  |
| 50 | h1h8 | 0.19 | 0.17 | g2h1 | 0.18 | 0.17 |  |
| 51 | h8a8 | 0.18 | 0.14 | a1g7 | 0.18 | 0.17 |  |
| 52 | b3b4 | 0.19 | 0.15 | h1h8 | 0.21 | 0.13 |  |
| 53 | a8a1 | 0.21 | 0.13 | g7b2 | 0.19 | 0.12 |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw.
