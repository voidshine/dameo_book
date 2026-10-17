# Game 1 — engine review of `Game01.md`

Beginner_2017 vs Busybee, 0:2. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn` (one hidden layer of 512, 400,000 self-play games).

## Verdict

The chapter's main story stands. The game record, the move numbering and all 26 diagrams are correct, every move in the main line and the variations is legal (with one notation slip), and the engine agrees that 20.g3g5? is the mistake that costs the game. Black's key moves — 20...d5c4!!, 25...e7f6!, 27...c6b5!!, 29...h2g1, 37...b8a8 — are the engine's choices too.

What needs attention is mostly in the side notes. In order of importance:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | Diagr.19 notes: 30.c1b2, 30.c1c2, 30.c1d2 | 30...g1f1! wins, 0:2 | 30...g1f1 loses the king in all three lines. White ends up better (c1c2), level (c1b2) or only slightly worse (c1d2). Black wins with 30...g1h2! or 30...c5b4! instead | Error (analysis) |
| 2 | Diagr.8: 7.h4h5 g6g4 8.g3h4 g5f4!? | "Puzzling complications", too risky for White | 9.f2g3! wins three men by force | Incorrect assessment |
| 3 | 9.f2c5! | An "unexpected refutation" of 8...b6b5 | Sound, but it trades White's edge (1.20) for a level game (1.05). 9.g3g4 keeps the edge (1.21) | Incorrect assessment |
| 4 | 20.g3g5? | No improvement is given | 20.f1f2! (1.13) keeps a small edge, and 20...d5c4 no longer works | Omission |
| 5 | 30.b3c4! | "White finds the best move" | It is the weakest of the serious tries (0.21). 30.c1b2 scores best (0.49), though everything loses | Incorrect assessment |
| 6 | 34...c7a7? | A mistake that lets White back in | It is as good as 34...c7b8!. The slip in the line is 35...a7b8, and Black still wins after it | Incorrect assessment |
| 7 | 28...f8g7!!, 31...e4f3!! | Brilliant, implied necessary | Both win, but 28...h2h1 and 31...b7h7 are simpler and score at least as well | Missed opportunity (Black) |
| 8 | 9.h4h6? line, move 11 | "Insignificant permutations of moves" | The order matters: 11.f2:h2 saves a man compared with 11.h6:f6 | Incorrect assessment |
| 9 | Diagr.16 | "Black's position is won" | Probably, but not yet clear (0.81). 25.c3a5 made it easier; 25.h4h6 or 25.b3b5 demand only-moves from Black | Missed opportunity (White) |
| 10 | 16.h1g2 e8e7 | No comment | Both are inaccurate: 16.f3g4 (1.10) and 16...c7c5 (0.98) | Omission |
| 11 | Diagr.21, line A | 36.d8:d4 | The king is on e8: 36.e8:d4 | Error (notation) |

Details follow, then the smaller points, the list of what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. The final position, where White resigned, reads 0.10. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000, so the deeper numbers come from the same search code and the same model run with a larger budget through a throwaway wrapper; nothing in the engine was changed. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends, at 1,000,000 simulations a move (300,000 where noted). One play-out is one sample, not a proof.
- **Noise.** Differences under about 0.08 mean nothing.
- **How far to trust it.** The engine is a self-taught network, not a tablebase. In this game it was slow to see Black's win: its score slid from 0.81 at move 23 to 0.31 at move 28 along a line it mostly approved of for both sides. I rely on it for concrete tactics, where I give the line and have checked each move for legality and for whether it is forced. Its positional verdicts are a second opinion.

## Findings in detail

### 1. Diagr.19 notes: the king raid 30...g1f1 does not win

The book gives `30.c1b2 (c1c2) g1f1! 0:2` ("then destroys everything in its path") and `30.c1d2 g1f1! 31.d2e3 f1:c3 32.d3:b3 e4:e2 0:2`.

In each line White has a reply after which the black king is obliged to capture and must stop on a square where a man takes it.

**30.c1c2 g1f1? 31.c2a4!** (the c2 man vaults b3)

```
    a b c d e f g h
  8 b . . . . . b b 8
  7 . . . . . . . . 7
  6 . w . . . . . w 6
  5 w . b . . w . . 5
  4 w . . . b . . . 4
  3 . w . w . . . . 3
  2 . . . . . w . . 2
  1 w . . . . B . . 1
    a b c d e f g h
```

Black has exactly two legal moves, both captures of three men, and both lose the king:

- 31...f1:a6 (f1:f3:f6:a6) 32.a5:a7 a8:a6. Five men each, no king. Engine: 1.33. White won both play-outs (300k and 1M).
- 31...f1:a3 (f1:f3:c3:a3) 32.a4:a2. White is a man up. Engine: 1.61.

So after 30.c1c2 the book's move turns a won game into a worse one.

**30.c1b2 g1f1? 31.b3a4!** Black has four legal captures:

- 31...f1:a6 32.a5:a7, or 31...f1:b1 (f1:f3:b3:b1) 32.a1:c1, and the king is gone at once.
- 31...f1:b8 or 31...f1:b7 (f1:f3:b3:b8). The king now stands on the open b-file and must take the b2 man next move, landing on b1: 32.a4a6 b8:b1 (Black's only legal move) 33.a1:c1. Five men each. Engine: 1.06. The play-out was drawn.

This is the a1–c1 trap the author himself mentions under Diagr.16.

**30.c1d2 g1f1 31.d2e3??** is where that line goes wrong, not at move 30. After 31.b6a7! Black's only legal move is 31...f1:a6 (f1:f3:c3:a3:a6, four men), and 32.a7:a5 takes the king. Five men each. Engine: 0.74. The play-out was drawn.

**What Black should play:**

- 30.c1b2 g1h2! 31.b3a4 h2:h7 (h2:d2:d5:h5:h7, four men, and the king is safe). Engine: 0.48. Black won both play-outs.
- 30.c1d2 g1h2! Engine: 0.47. Black won the play-out.
- 30.c1c2 c5b4! 31.b3:b5 g1f1. Engine: 0.39. Black won the play-out.

So the conclusion that White is lost at Diagr.19 holds, but the refutations given need replacing.

### 2. Diagr.8: 8...g5f4!? loses three men

After 7.h4h5 g6g4 8.g3h4 g5f4 White plays **9.f2g3!**. Black must capture two men, by 9...f4:h2 or 9...f4:d2 (both through f2), and then 10.g3:g7 (g3:g5:e5:e7:g7) takes four. For example 9...f4:h2 10.g3:g7 f8f7 11.g7:e7 e8:e6 12.h1:h3, and White has 15 men against 12. Engine: 1.60 at the diagram. White won the play-out (300k).

At 50k, 8...g5f4 is the worst of Black's 48 legal moves. The position before it is already good for White (7...g6g4: 1.20).

The real test of 7.h4h5 is **7...b8e5!** (1.05), the binding move the author says he feared. It is not analysed. So avoiding 7.h4h5 was reasonable (7.h4h5 1.06, 7.g2h3 1.11, 7.d1g4 1.17), but line B is not the reason.

Line A is fine: Diagr.7 is 1.16, "favourable for White".

### 3. 9.f2c5! is not a refutation of 8...b6b5

Before White's 9th move the engine has 1.20. After 9.f2c5 b5:d5 10.e3c5 e5:e1 11.c5:g7 g6:g8 12.d1:f1 it has 1.05, with everything from move 9 on forced or best for both sides. The combination works, but it cashes a small edge for a level game.

The engine prefers to keep the tension:

- 9.g3g4 g5:g3 10.f3:h3 — 1.21 (its first choice, 86% of the search)
- 9.e2g4 — 1.18
- 9.d1d2 — 1.17
- 9.d1g4 — 1.16

8...b6b5 costs Black only about 0.08 against 8...f7f6 (1.13), so "?!" is fair, but "refutation" and the note at Diagr.12 that "White has an advantage" (1.05) overstate it. Black's 12...f5f4 is the engine's first choice.

### 4. 20.g3g5? — the missing improvement

The engine agrees completely with the mark: 1.11 before the move, 0.78 after it, the largest single loss in the game. It also confirms the author's calculations: 20...c7f4 1.62, 20...e5f4 1.28 and 20...d6f4 1.72 are all good for White, and 20...d5c4!! is the only move (98% of the search).

The chapter does not say what White should have played. The engine's answer is **20.f1f2!** (1.13, 91% of the search). The f1 man steps up before the g-file advance, and then 20...d5c4? fails (1.30). 20.h4h6 (1.08) is the alternative.

### 5. 30.b3c4! is not the best move

| 30th move | c1b2 | c1d2 | b6a7 | c1c2 | b3a4 | **b3c4** | f5e6 |
|:--|--:|--:|--:|--:|--:|--:|--:|
| Engine | 0.49 | 0.47 | 0.42 | 0.41 | 0.30 | **0.21** | 0.19 |

Every move that was played out lost (c1b2, c1d2, c1c2 and b3c4), so the mark does not change the result. But "the best move" is not supported. 30.b3c4 sets one trap, 30...c5:e3??, in a position where Black has only three legal moves and the other two win. The c1 moves set the 30...g1f1 trap of finding 1, which is the more natural move to fall into.

### 6. 34...c7a7? is not a mistake

The book's line is 34.a1b2! c7a7? 35.a5b6 a7b8 36.b6c7 b8:b1 37.c7d8, "White's chances increase".

- 34...c7a7 (0.19) is as good as 34...c7b8 (0.19), and is the engine's first choice.
- After 35.a5b6 Black just plays 35...g8h7 (0.18). The b6 man cannot move: 36.b6b7 and 36.b6c7 are both captured by the king on a7. Black won the play-out.
- The slip is 35...a7b8?, which allows 36.b6c7!. Even so 37.c7d8 is 0.32, and Black won that play-out too.

The "?" belongs on 35...a7b8, and the gain for White is a longer game, not a different result.

### 7. 28...f8g7!! and 31...e4f3!! are not the only way

**28...h2h1** scores 0.30 against 0.51 for the game's 28...f8g7. The lines are 29.b3a4 h1:b8 (h1:b1:b8, winning the c1 and b6 men) and 29.c1b2 e4f3! (0.16). Black won the play-outs of both 28th moves. Busybee's way is the prettier one, and it is precise: after 28...f8g7 29.h7:f5 only 29...h2g1 works (29...h2h1 is 1.02).

**31...b7h7** at once is 0.19, and so is 31...b7a7; the game's 31...e4f3 is 0.26. The play-out went 31...b7h7 32.f2e3 h7:c7 (h7:h5:c5:c7) 33.e3:e5 c7a7 and 0:2. The sacrifice is elegant but not needed.

### 8. 9.h4h6?: the move order at move 11 matters

The verdict on 9.h4h6 is right: 9...f5g4!! 10.h5:d5 g4:g2 is 0.83, against 1.21 available. But after 10...g4:g2 the book says "there are possible insignificant permutations of moves".

- Book: 11.h6:f6 f7:f5 12.f2:h2 b5c4! — 0.51. After 13.d4:b4 d6:d4 (d6:d4:f4:f2:d2:d4) White has 11 men against 13.
- **11.f2:h2!** g6h5 12.h6:h4 b5c4 13.c3:c5 c6:c4 14.d4:b4 d6:d4 — 0.83. The same five-man shot lands, but White has 11 against 12.

Black won the play-outs of both (300k), so "won position" at Diagr.11 may well be right. The engine's own number there is 0.51.

### 9. Diagr.16: "Black's position is won"

The engine has 0.81 here, which is an advantage, not a win. Its play-outs split:

- Game: 25.c3a5 e7f6! — 0.58, and Black won the play-out without difficulty.
- 25.b3b5 — 0.83. Black won after 25...e7f6 and 25...d6d5; 25...d6e5 was drawn.
- 25.h4h6 — 0.83. Black won after 25...h8g7; 25...e7f6 and 25...e7c5 were drawn; after 25...d6e5 White won.

So the verdict is plausible, but 25.c3a5 was the most convenient move for Black. 25.b3b5 and 25.h4h6 were better practical tries. 24.f1f2! is confirmed as the only move (0.82; next best 0.66).

### 10. 16.h1g2 and 16...e8e7

Neither move has a comment, and the engine thinks both are inaccurate:

- 16.h1g2 — 0.98. 16.f3g4 — 1.10 (93% of the search).
- 16...e8e7 — 1.14. 16...c7c5 — 0.98.

17.c2d3?! (1.03) is confirmed, and so is the suggested 17.b2b3 (1.13). The engine's first choice is 17.f1h3 (1.15).

### 11. Diagr.21, line A: notation

Line 518 has `36. d8:d4`. The king moved to e8 on move 35, so this is `36. e8:d4` (in full e8:h8:h4:d4, three men). The line itself is sound.

There is also a third and quicker win at Diagr.21: **34.f5e6!** (1.95) e1:f1 (e1:a1:a6:f6:f1, four men, forced) 35.h6h7 h8:h6 36.d8:e5 (d8:h8:h1:e1:e5), which takes g8, h6, the king and e4 and leaves Black one man. The book's 34.h6h7 (1.77) and 34.f5f6 (1.81) also won their play-outs.

## Smaller points

- **Opening.** 4...e8h5 (0.92) is the engine's move in place of both 4...d8b6 and the variation 4...g5h4 (1.06 each); likewise 3...e8h5 (0.93) for 3...g7g5 (1.03). For White, 4.g2h3 (1.03) for 4.e1e3 (0.92), and 8.d1g4 (1.21) for 8.h3h5 (1.13). All are worth about 0.1.
- **6.h4h5?** The mark is fair but the wording is strong. The move costs about 0.15 (0.97 against 1.12 for 6.g2h3), and Diagr.5 is 0.96, which is level, not "worse".
- **7...f6e5** costs about 0.1 against 7...b8e5 (1.10). "Binds White's centre in an unusual way" could say so.
- **21...c7f4.** The same shot with the other man, 21...d6f4, scores slightly better (0.73 against 0.82).
- **27.h5h7!** is one of several equal moves (0.33; 27.a5a6 0.39, 27.f2f3 0.38). The mark is fine as a practical one.
- **34.a1b2!** is level with the other moves (0.20). Nothing is stronger, and nothing saves the game.

## Confirmed by the engine

These need no change:

- Every main-line move and every variation move is legal under compulsory, maximum capture, apart from the slip in finding 11.
- The material counts in the text (Diagr.3, 5, 12, 13, 16, 19 and 23) are right.
- Diagr.3: 1.07, "a small advantage". Diagr.7: 1.16.
- 17.c2d3?!, and 17.b2b3 as the improvement. 18...b7d5! and 19...b8e5 are first choices.
- 20.g3g5? and 20...d5c4!!. 21.d4:b4 is at least as good as the other capture (0.73 against 0.70).
- 24.f1f2!, 25...e7f6! (and 25...d6e5 is indeed worse, 0.86), 26.h4h6!, 26...d6c5, 27...c6b5!!.
- The threat behind 27.h5h7: after h6g7 either capture lets White promote on g8 or h8.
- 30.f5e6 and 30.b3a4 lose as stated (0.19 and 0.28).
- 30...c5:e3?? loses (1.70), and 31.b6c7 is the only move that punishes it.
- 32...b7h7!, 33.d3e4 ("nothing better"), 35...a8b7!.
- 37...b8a8 is the only winning move, and the final position is lost (0.10).

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 35 | Diagr.2 shows the position after 4...d8b6, but the variation under it starts with 4...g5h4, and Diagr.3 correctly shows d8 still at home. The file is `SmallDiagram-015-8.png`; every other diagram is a `LargeDiagram`, and `Game01.agt` flags the position after 4.e1e3 (`LargeDiagram-002-7.png`). | Use the diagram after 4.e1e3 and give 4...d8b6 after the variation, or say "instead of 4...d8b6" |
| 50 | The alt text of the Diagr.3 image is "Diagram 2" | "Diagram 3" |
| 102 | The label `А)` is a Cyrillic А (U+0410). Its partner `B)` and the later `A)` on line 510 are Latin | Latin A |
| 175 | "What? B6b5??" has a capital B and "??", but the move is marked "?!" in the table and in the next sentence. `Game01.agt` has "b6b5?!" | "What? b6b5?!" |
| 426–474 | The 30...g1f1! lines | See finding 1 |
| 518 | `36. d8:d4` | `36. e8:d4` |

All 26 diagram images were compared square by square with the engine's positions, and all match. The main-line tables match `Game01.agt` move for move.

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | e3e4 | 0.96 | 0.97 | g8h7 | 0.99 | 0.99 |  |
| 2 | g1g3 | 0.94 | 0.94 | h7g6 | 0.94 | 0.96 |  |
| 3 | d2d4 | 0.90 | 0.93 | g7g5 | 1.02 | 1.03 | 3...e8h5 0.93 |
| 4 | e1e3 | 0.90 | 0.92 | d8b6 | 1.08 | 1.06 | 4.g2h3 1.03; 4...e8h5 0.92 |
| 5 | f2h4 | 1.10 | 1.06 | e6f5 | 1.14 | 1.13 |  |
| 6 | f1f2 | 1.10 | 1.07 | e7e6 | 1.14 | 1.15 | 6...b8b5 1.07 |
| 7 | g2h3 | 1.15 | 1.11 | f6e5 | 1.21 | 1.20 | 7...b8e5 1.10 |
| 8 | h3h5 | 1.12 | 1.13 | b6b5?! | 1.19 | 1.20 | 8.d1g4 1.21; 8...f7f6 1.13 |
| 9 | f2c5! | 0.95 | 1.04 | b5:d5 | 0.95 | 1.04 | 9.g3g4 1.21 |
| 10 | e3c5! | 1.02 | 1.05 | e5:e1 | 1.02 | 1.05 |  |
| 11 | c5:g7 | 1.02 | 1.05 | g6:g8 | 1.02 | 1.05 |  |
| 12 | d1:f1 | 1.02 | 1.05 | f5f4 | 0.99 | 1.04 |  |
| 13 | f3:f5 | 0.99 | 1.04 | g5:e5 | 0.99 | 1.04 |  |
| 14 | b1e4 | 0.96 | 1.00 | e5:e3 | 0.96 | 1.00 |  |
| 15 | d3:f3 | 0.96 | 1.00 | c8e6 | 1.02 | 1.09 | 15...g8h7 1.01 |
| 16 | h1g2 | 0.95 | 0.98 | e8e7 | 1.11 | 1.14 | 16.f3g4 1.10; 16...c7c5 0.98 |
| 17 | c2d3?! | 1.07 | 1.03 | d7f5! | 1.13 | 1.09 | 17.f1h3 1.15 |
| 18 | g2g4 | 1.04 | 1.06 | b7d5! | 1.06 | 1.05 |  |
| 19 | b2b3 | 1.10 | 1.10 | b8e5 | 1.05 | 1.11 |  |
| 20 | g3g5? | 0.86 | 0.78 | d5c4!! | 0.83 | 0.77 | 20.f1f2 1.13 |
| 21 | d4:b4 | 0.84 | 0.73 | c7f4 | 0.84 | 0.82 | 21...d6f4 0.73 |
| 22 | g4:e4 | 0.83 | 0.82 | e5:g3 | 0.83 | 0.82 |  |
| 23 | g5:e5 | 0.83 | 0.82 | e6:e4 | 0.83 | 0.81 |  |
| 24 | f1f2! | 0.85 | 0.82 | g3h2 | 0.83 | 0.81 |  |
| 25 | c3a5 | 0.78 | 0.71 | e7f6! | 0.84 | 0.58 | 25.h4h6 or 25.b3b5 0.83 (finding 9) |
| 26 | h4h6! | 0.86 | 0.44 | d6c5 | 0.65 | 0.42 |  |
| 27 | h5h7! | 0.62 | 0.33 | c6b5!! | 0.58 | 0.32 |  |
| 28 | b4:b6 | 0.58 | 0.31 | f8g7!! | 0.59 | 0.51 | 28...h2h1 0.30 |
| 29 | h7:f5 | 0.59 | 0.50 | h2g1 | 0.55 | 0.49 |  |
| 30 | b3c4! | 0.27 | 0.21 | g1:b7! | 0.25 | 0.20 | 30.c1b2 0.49 |
| 31 | c4:c6 | 0.24 | 0.20 | e4f3!! | 0.28 | 0.26 |  |
| 32 | f2:f4 | 0.27 | 0.25 | b7h7! | 0.25 | 0.24 |  |
| 33 | d3e4 | 0.26 | 0.24 | h7:c7 | 0.24 | 0.22 |  |
| 34 | a1b2! | 0.21 | 0.20 | c7b8! | 0.22 | 0.19 |  |
| 35 | b2a3 | 0.24 | 0.16 | a8b7! | 0.17 | 0.15 |  |
| 36 | a3a4 | 0.17 | 0.13 | b7a6 | 0.14 | 0.11 |  |
| 37 | a5:a7 | 0.13 | 0.11 | b8a8 | 0.12 | 0.10 |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw. Moves 25 to 28 show why the deeper search was needed: at 50k the engine had not yet seen how bad White's position was.
