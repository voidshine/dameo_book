# Game 2 — engine review of `Game02.md`

Ray Garrison vs Beginner_2017, 0:2. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

The game record is right: the main line matches `Game02.agt` and the Little Golem record `game1946344.pdn` move for move, every move in the main line and in the variations is legal, and all 33 diagrams show the positions they should. The end of the story also holds: 24.c1b2?! and 26.g2a2? are mistakes, and 28.a8a2 loses at once.

The analysis is a different matter. This is the most tactical chapter so far, and the engine overturns a good deal of it. In order of importance:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | Moves 9, 10 and 12 (Diagr.3, Diagr.4) | Black has the advantage; White is "constrained and clamped" | White has a forced combination, f4f5!, that makes a king on f8. It was there three times | Missed opportunity (White), omission |
| 2 | 18...g7h6! | "An unexpected answer" | 18...g7g4! wins the white king by force | Missed opportunity (Black) |
| 3 | Diagr.21: 24.g2a2 | "With high probability led to White's victory" | 24.g2a2 loses the king to 25...a7b6! | Error (analysis) |
| 4 | Diagr.26 | 26.g2a2? loses, 0:2 | 28.a4b4! wins for White, so 26.g2a2 is not a mistake there | Error (analysis) |
| 5 | Diagr.17: 22.e4e5 | "Nothing gives White"; Black is "at least no worse" | With 24.c2:b8! it is White's strongest line, better than the game's 22.f2g3 | Incorrect assessment |
| 6 | Diagr.29–30 | A demonstrated draw | The demonstration loses to 35...h4g3!, and Diagr.29 itself is not clearly a draw | Error (analysis) |
| 7 | 23...f7f5? 24.e4e5! | Wins for White in both lines | The verdict is right, but the winning move is 24.g3h4!. After 24.e4e5 Black holds | Error (analysis) |
| 8 | Continuation h8h5 | 18...f6g5 is the "stronger move" and leads to a draw | 18...f6g5 loses material to 19.g2g3!. Black's move is 18...g7e5! | Error (analysis) |
| 9 | 12...c7c5? and 13.d3b5!! | The wrong piece; a brilliant punishment | No better 12th move is found, and Diagr.5 is not clearly good for White (low confidence) | Incorrect assessment |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations. A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best; it is slower but does not overlook sacrifices. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it here.** Less than in Game 1. With a king against a mass of men, the engine repeatedly misjudged positions until the tactics were on the board. Diagr.6 reads 0.79, for example, yet the book's own 19.g4h5!! line is 1.45 three moves later. So every finding below rests on a concrete line that I checked move by move, and I mark the ones that are only the engine's opinion.

## Findings in detail

### 1. White's missed combination: f4f5!

After 8...e8g6 the book calls Diagr.3 a "picture position" for Black, and at Diagr.4 it says White is constrained and resorts to "a desperate measure". In all three positions where White had a free choice (moves 9, 10 and 12), he could play **f4f5!**, and every black reply from there is forced.

At move 12, in place of 12.c3b4:

```
12.f4f5!  f6:h4    (f6:f4:h4, forced)
13.c3c5   d5:b5    (forced)
14.g2g4   h4:f4    (forced)
15.f3:d7  c7:e7    (f3:f5:d5:d7; both forced)
16.f2c5   b5:d5    (forced)
17.d4:f8           (d4:d6:f6:f8)
```

Eleven pieces each, and White has a king on f8. Engine: 1.48 after 12.f4f5, against 0.79 for the game's 12.c3b4. White won the play-out.

The same idea works earlier with a different move order:

- 9.f4f5! f6:h4 10.e3c5 d5:b5 11.e2g4 h4:f4 12.f3:d7 c7:e7 13.c3c5 b5:d5 14.d4:f8 — 1.36 (the game's 9.e2e4 is 1.00).
- 10.f4f5! f6:h4 11.c3c5 d5:b5 12.g2g4 h4:f4 13.f3:d7 c7:e7 14.f2c5 b5:d5 15.d4:f8 — 1.44 (the game's 10.b2a3 is 0.92).

So the move that needs a mark is **8...e8g6**, which allows it. 8...c7c5 keeps the game level (0.99, 50k). Black's 9th and 10th moves allow it again; 9...a7c5 (1.01, 50k) and 10...a7a4 (0.92) do not.

The book notes that 8.h2f4 was played "with the idea of f3f5". The idea that works is f4f5.

### 2. 18...g7g4! wins the king

After 17...h6g5! 18.g4h5 the book gives 18...g7h6! (0.96). The engine's move is **18...g7g4!**:

- 19.f4:h2 (f4:h4:h2, forced)
- 19...b7c6, and the king must take: 20.c2:g7 (c2:c7:g7, forced)
- 20...h7:f7, capturing the king.

Black has given four men for the king. Nine men each, and the engine has 0.43. Black won both play-outs (standard and wide). The idea needs the g7 square empty, which is why it only appears now.

### 3. Diagr.21: 24.g2a2 loses the king

The book's line is 24.g2a2 b7a6 25.a2:a4 b3b2 26.a4d1! and so on to Diagr.24, concluding that "the direct attack 24.g2a2 with high probability led to White's victory".

Instead of 25...b3b2 Black plays **25...a7b6!**. The king's only legal move is 26.a4:g7 (a4:a7:g7), and 26...h7:f7 takes it. Six men each with Black's b3 man far advanced. Engine: 0.42. Black won both play-outs.

So 24.g2a2 is a mistake (0.44), and Ray was right to avoid it. The closing summary (line 578, "he could punish me a second time") repeats the claim. The book's own line also has a second slip for Black: 27...b2b1 (1.53) where 27...a7b6 holds (1.03).

### 4. Diagr.26: the "0:2" is a win for White

The line is 24.f4e5 h8g7 25.e5:e7 f7:d7 26.g2a2? b7a6 27.a2:a4 b3b2, and the book says 28.a4d1 fails to 28...b2b1, "0:2".

That part is true (28.a4d1? b2b1 29.d1:d8 forced, 0.15). But White has **28.a4b4!** b2b1 29.b4e1 b1:d1 (forced) 30.e1:b1. Engine: 1.60. White won both play-outs.

So in this line 26.g2a2 does not deserve the "?" (1.16), and it is 26...b7a6 that goes wrong (1.47); 26...a3b2 keeps 1.16. The broader point survives: 24.f4e5 was White's best 24th move (1.16), and White won the play-outs after it.

### 5. Diagr.17: 22.e4e5 was the strongest move

The book gives 22.e4e5 e6:g4 23.f2e3 h2:f2 24.c2:a2 b4b3! 25.a2:a8 b3b2 (Diagr.19) and concludes "Black's position is at least no worse".

White has six ways to make the four-man capture at move 24. The best is **24.c2:b8!** (c2:g2:g6:b6:b8), which takes the b7 man instead of leaving the king on the a-file. Engine: 1.51. White won both play-outs. The book's 24.c2:a2 is 1.15, and even Diagr.19 is 1.26 (White won that play-out too).

The note that the king "would be caught after b7b6" if it took a different route is true of 24.c2:b1 (1.05), but not of the route to b8.

So 22.e4e5 (1.48) was better than the game's 22.f2g3 (1.17).

### 6. Diagr.29 and 30: the draw is not demonstrated

After 26.f4g5! the book reaches Diagr.29 ("It seems to be a draw!") and shows 30.c1b2 h8h6 31.b2a3 h7h4 32.a3a4 h6h3 33.a4a5 a6:a4 34.a1:a8 h5h2 (Diagr.30) 35.a8a2 h4h1 36.a2a1 h1:e1 37.a1:h1 h3g2, "Draw!".

- At move 35 Black has **35...h4g3!** (0.26) in place of 35...h4h1 (0.99). After 36.a2a8 g3f2 37.f1:f3 h2h1 Black has a king and a man against a king and a man, and won the play-out.
- Diagr.30 is 0.33 wherever the king goes; the reply is ...h4g3 each time.
- Earlier in the line 31.b2a3 (0.36) is weaker than 31.b2c3 (0.54).
- Diagr.29 itself is 0.57. The engine's first move is 30.f1g2, keeping the f-man. The standard play-out from there was drawn; the wide play-out was won by Black.

So the line as printed shows a loss, and "draw" at Diagr.29 is plausible but open.

It also matters for the game. 26.g2a2? is rightly marked (0.65), but the engine's improvement is **26.g3e5** (0.99), not 26.f4g5 (0.60).

### 7. 23...f7f5?: the refutation is 24.g3h4!

The verdict is right (1.37 after 23...f7f5, against 1.16 for 23...b4b3!). But the engine's reply is **24.g3h4!** f5:f3 (forced) 25.e4e5 e6:e4 (forced) 26.g2a2 — 1.57.

After the book's 24.e4e5! (1.02) Black holds in both branches:

- 24...f5:h3! 25.e5:e7, and now 25...b4b3 (1.07). The book's 25...h3h2? loses to 26.e7f8! as shown (1.71).
- 24...e6:g4 25.g3:e5, and now 25...b4b3 (1.29). The book's 25...b7c6? loses (1.76).

### 8. Continuation h8h5

The book's conclusion is that 17...h8h5 "allows Black to keep the position under control", by 18.e4d5 f6g5 and a long line to a draw at Diagr.14–15.

- **18...f6g5** runs into **19.g2g3!** (1.47) in place of 19.d5d6! (0.50). After 19...h3:f5 (forced) 20.c2a2 g5:g3 (forced) the king takes five men: 21.a2:b8 (a2:a4:f4:f6:b6:b8).
- The engine's 18th move is **18...g7e5!** (0.52): 19.d5:f5 (forced) e6e5 20.f5:d5 (forced) b7c6 21.c2:g5 (forced) h5:f3 22.f2:f4, and the king is gone. The wide play-out from here was drawn.
- 18...h6g5? is rightly marked, but 19.c2c6! (1.60) is stronger than 19.h1h2! (1.15).
- 18...g7g5 19.c2d1! is confirmed (1.71).
- Diagr.14 is 1.13, and White won both play-outs from it, so "draw" there is not secure either. In the line to Diagr.15 the engine wants 28.h2g1 (1.44) in place of 28.h2c2 (0.46).

The other two continuations are sound: 17...e6e5 loses to 19.g4h5!! as shown (Diagr.8 is 1.47), and 17...g7g5 loses to 18.g4f5!! (Diagr.16 is 1.46). White won both play-outs.

### 9. 12...c7c5?, 13.d3b5!! and Diagr.5 (low confidence)

This is the engine's opinion, not a checked line, and it is the kind of position where the engine did worst.

- It rates 12...c7c5 (0.77), 12...a7c5 (0.79, the move that keeps a man on c7) and 12...b6c5 (0.82) the same, and answers all three with 13.c4b5. 12...c6c5?? loses to 13.c4:a8 (1.72).
- It rates 13.d3b5!! (0.68) a little below 13.c4b5 (0.78).
- It has Diagr.5 at 0.67, that is, fine for Black. Its own preferred 17th move is 17...b4c3 (0.59; Black won the wide play-out). But its second choice, 17...h3h2 (0.63), lost the wide play-out, so these numbers are not stable.

I would not change the text on this evidence. If anything is softened, it is the claim that White had "a great chance of winning" after the combination: the game's own 18...g7g4! (finding 2) shows how quickly it turns.

## Smaller points

- **19...h3h2** (1.20) gives White more than playing 19...g6g5 at once (0.96), which keeps the h3 man.
- **25...f7f5!** is fine, but only level after 26.g3e5 (0.98). The engine prefers 25...b7a6 (0.84).
- **7.a2b3?** is confirmed (0.96; 7.b2a3 is 1.09). The suggested 7.e2b5! b6:b4 8.c4:a4 is sound (1.02).
- **10...a5a4** (1.11) is weaker than 10...a7a4 (0.92), quite apart from finding 1.
- **28.a8a2.** The alternatives 28.f1f2 (0.65), 28.f1e2 (0.58) and 28.a8a1 (0.47) all lost their play-outs, so "prolonged the struggle, but did not save the game" is right.

## Confirmed by the engine

- 6...f8h6?! (1.09; 6...c7c5 is 0.98).
- The whole of *Continuation e6e5*: 19.c2a2? h6g5! wins for Black (0.37), and 19.g4h5!! h6:h4 20.h1h2! h3:h1 21.c2d1 h1:e1 22.d1:b1! wins for White (1.47), including 22...b7a6 23.f4e5 (1.48) and 22...b7c6 to Diagr.9 (1.72).
- The whole of *Continuation g7g5* (Diagr.16, 1.46).
- Diagr.12 (1.76), and 24...f7h5 25.a8a7 (1.85).
- "You cannot move along the second horizontal" before 30.h2g1: after ...h3g2 the king's only move is to take, and it is lost (0.18).
- 23...b4b3! is the best move (1.16), and 24.g3e5 e6d5! is good for Black (Diagr.25, 0.40; Black won the play-out).
- 24.c1b2?! (0.87) and the suggested 24.f4e5 (1.16).
- 26.e4e5? and Diagr.28 (0.45; Black won the play-out).
- 26.g2a2? (0.65), and everything from 28...h2h1 to the end.

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 392–461 | The 24.g2a2 verdict and the Diagr.26 line | See findings 3 and 4 |

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move. The annotations are the chapter's.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | g1h2 | 1.01 | 1.00 | e7e5 | 1.03 | 1.03 |  |
| 2 | e1g3 | 1.01 | 1.02 | d7d5 | 1.03 | 1.03 |  |
| 3 | b1a2 | 1.03 | 1.02 | g8h7 | 1.02 | 1.02 |  |
| 4 | d2d4 | 1.00 | 0.99 | d8b6 | 1.02 | 1.01 |  |
| 5 | c2c4 | 1.00 | 1.00 | c8a6?! | 1.03 | 1.02 |  |
| 6 | d1g4 | 0.99 | 0.99 | f8h6?! | 1.10 | 1.09 | 6...c7c5 0.98 |
| 7 | a2b3? | 0.95 | 0.96 | b8a7 | 0.95 | 0.99 | 7.b2a3 1.09 |
| 8 | h2f4 | 0.96 | 0.98 | e8g6 | 1.02 | 1.00 | 8...c7c5 (finding 1) |
| 9 | e2e4 | 1.01 | 1.00 | a8a5 | 0.99 | 1.02 | 9.f4f5 1.36 |
| 10 | b2a3 | 0.91 | 0.92 | a5a4 | 1.01 | 1.11 | 10.f4f5 1.44; 10...a7a4 0.92 |
| 11 | a3:a5 | 1.01 | 1.11 | a6:a4 | 1.01 | 1.11 |  |
| 12 | c3b4!? | 0.80 | 0.79 | c7c5? | 0.79 | 0.77 | 12.f4f5 1.48 |
| 13 | d3b5!! | 0.66 | 0.68 | c5:a3 | 0.66 | 0.68 | 13.c4b5 0.78 (finding 9) |
| 14 | f3f5! | 0.67 | 0.68 | d5:h3 | 0.67 | 0.68 |  |
| 15 | f5:d7 | 0.67 | 0.68 | a4:c4 | 0.67 | 0.67 |  |
| 16 | d7c8 | 0.67 | 0.68 | b6:b4 | 0.67 | 0.68 |  |
| 17 | c8:c2 | 0.67 | 0.67 | h6g5! | 0.93 | 0.89 | 17...b4c3 0.59 (finding 9) |
| 18 | g4h5 | 0.95 | 0.89 | g7h6! | 0.96 | 0.96 | 18...g7g4 0.43 |
| 19 | h5:f5 | 0.96 | 0.96 | h3h2 | 1.07 | 1.20 | 19...g6g5 0.96 |
| 20 | h1:h3 | 1.07 | 1.20 | g6g5 | 1.19 | 1.18 |  |
| 21 | f5:h5 | 1.19 | 1.18 | h6:h2 | 1.20 | 1.18 |  |
| 22 | f2g3 | 1.28 | 1.17 | h2:f2 | 1.28 | 1.17 | 22.e4e5 1.48 |
| 23 | c2:g2 | 1.12 | 1.17 | b4b3 | 1.10 | 1.16 |  |
| 24 | c1b2?! | 0.83 | 0.87 | b3:b1 | 0.83 | 0.87 | 24.f4e5 1.16 |
| 25 | a1:c1 | 0.82 | 0.87 | f7f5! | 0.78 | 0.98 | 25...b7a6 0.84 |
| 26 | g2a2? | 0.76 | 0.65 | f5:h3 | 0.76 | 0.65 | 26.g3e5 0.99 |
| 27 | a2:a8 | 0.76 | 0.65 | h3h2 | 0.73 | 0.64 |  |
| 28 | a8a2 | 0.20 | 0.18 | h2h1 | 0.18 | 0.17 | 28.f1f2 0.65 |
| 29 | a2b1 | 0.17 | 0.16 | h1:e5 | 0.17 | 0.16 |  |
| 30 | b1:b8 | 0.16 | 0.15 | e6f5 | 0.16 | 0.15 |  |
| 31 | b8e8 | 0.15 | 0.11 | e5e1 | 0.11 | 0.10 |  |
| 32 | e8h5 | 0.09 | 0.05 | e1:a1 | 0.08 | 0.05 |  |
| 33 | h5:d5 | 0.07 | 0.03 | f6g5 | 0.04 | 0.02 |  |
| 34 | d5:h5 | 0.03 | 0.02 | h7h6 | 0.01 | 0.01 |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw. Note that neither column shows the f4f5 combination in the rows for moves 8 to 11: the engine only finds it once the move is on the board, which is why the alternatives were checked one by one.
