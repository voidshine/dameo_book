# Game 3 — engine review of `Game03.md`

Tony vs Beginner_2017, 0:2. Checked with **damleo** (commit `7407e6e`) and `run/model-12.nn`.

## Verdict

The game record is right. The main line matches both `Game03.agt` and the Little Golem record `game2097149.pdn`, every move in the main line and the variations is legal, and all 23 diagrams show the positions they should.

The chapter's central judgments also hold: Diagr.18 is winning for White, 24.h4g5!! is a correct refutation, 24.g3e5? throws the win away, and 28...c5d4! is the only move.

What the chapter does not show is how many times the game changed hands before that. The engine finds a decisive shot that both players missed at four separate points, and it disagrees about which of Black's moves was the real mistake:

| # | Where | The book says | The engine says | Kind |
|--:|:--|:--|:--|:--|
| 1 | 13...b7a6 14.e2c4 | Black patches the light squares; White plays cautiously | 13...b7a6 loses to 14.d3c4!, which puts a man on b7. White missed it | Missed opportunity (White), omission |
| 2 | 16.c1d2, 17.f2g3 | White "closes the d2 square just in case" | 16.c1d2 allows 16...e6f5!, which wins a king. Black missed it twice | Missed opportunity (Black), omission |
| 3 | 24...h5g4 | Black did not know which square to king on | 24...h2g1! wins at once | Missed opportunity (Black) |
| 4 | 18...d6f4 | "This move and all subsequent hits should have led to Black's defeat" | 18...d6f4 is sound. The losing moves are 20...g6f5 and above all 21...b6d4 | Incorrect assessment |
| 5 | Diagr.2: 6...d8b6 | The open path the author would choose now | It loses two men by force | Error (analysis) |
| 6 | Option 3 (Diagr.8, Diagr.10) | 15...b7a6 is safe until 16.f3d5; 20.d3f5!! is the way | White has stronger moves at both points: 16.h3g4! and 20.e4d5! | Omission |
| 7 | 25.e5e7, 27.d6c7 | No comment | 25.e3e4 and 27.e7e8 keep White on top | Missed opportunity (White) |

Details follow, then the smaller points, what the engine confirms, the editorial errata, and a move-by-move table.

## How to read the numbers

- **Scale.** Every evaluation is White's expected score out of 2 (twice the engine's win chance plus its draw chance). 2.00 is a certain 2:0, 1.00 is a level game, 0.00 is a certain 0:2.
- **Won positions do not read 0 or 2.** damleo counts a win that is still many moves away partly as a draw. Anything near 0.2 or 1.8 already means "winning".
- **Depth.** Numbers are from searches of 2,000,000 simulations unless marked "50k". `damleo eval` is fixed at 50,000; the deeper numbers come from the same search code and model run with a larger budget through a throwaway wrapper. The appendix gives both.
- **Play-out.** The engine plays both sides from a position until the game ends. A *standard* play-out takes the engine's most-searched move at 300,000 simulations. A *wide* play-out tries every legal move and takes the one whose resulting position `damleo eval` likes best. One play-out is one sample, not a proof.
- **Forced.** When I call a move forced, it is the only legal move in that position.
- **How far to trust it.** The engine often fails to see a sacrifice until it is on the board: before 14.d3c4 it reads the position as level at 50k. So the alternatives were checked one by one, and every finding below rests on a line I checked move by move. Differences under about 0.08 mean nothing.

## Findings in detail

### 1. 13...b7a6 loses to 14.d3c4!

The book says Black "decided to patch up the holes in the light fields" and expected 14.b3b5 c8b7. White has a combination instead:

```
14.d3c4!  d5:d3    (forced)
15.c4d5   d6:f4    (d6:d4:f4, forced)
16.f3:b7  d3:f3    (f3:f5:d5:b5:b7, four men; both forced)
17.f2:f4           (forced)
```

White has 13 men against 12 and a man on b7 that promotes on b8. Engine: 1.62 after 14.d3c4, against 1.06 for the game's 14.e2c4. White won the play-out.

The move for Black is **13...d7b5** (1.04), which the game reaches a move later. None of the three options the chapter examines at Diagr.5 is as good: 13...e6c4 is 1.24, 13...c6c4 14.b3a4 is 1.33 and 13...d5c4 is 1.16, all as the book says.

### 2. 16.c1d2 allows 16...e6f5!

After 15...a6a4 the book says Tony "closes the d2 square just in case". That move loses:

```
16...e6f5!  17.e4:e6  d6:f6    (all forced)
18.d4:d6    c6:e6              (both forced)
19.c4:a6    a4:g2              (c4:c6:a6 forced; a4:c4:c2:e2:g2, four men)
20.a6:a4    g2g1               (forced; Black kings)
```

Black has 12 pieces against 11 and a king. Engine: 0.39. Black won the play-out.

Black played 16...f8g7 (0.96), and after 17.f2g3 the shot was there again: 17...e6f5! 18.e4:e6 d6:f6 19.d4:d6 c6:e6 20.c4:a6 a4:e4 21.a6:a4 is 0.65, and Black won that play-out too. Black played 17...a8a7 (0.98).

White's safe moves were 16.f3f4 (1.02) and then 17.d2f4 (0.97).

### 3. 24...h2g1! wins at once

After 24.g3e5? the book explains that Black could king on a1 or b1, did not know which, and so played 24...h5g4. The winning move kings the other man:

```
24...h2g1!  25.h1:f1           (forced)
25...g7f6   26.e6:g6  h6:f6    (both forced)
27.h4:h8                       (h4:h6:h8, forced)
27...a2a1   28.h8:a4  a1:b4    (a1:a6:e6:e4:e1:h1:h4:b4, seven men)
```

White is left with four men against a king and two men. Engine: 0.20, against 1.10 for the game's 24...h5g4. Black won both play-outs (standard and wide).

24...a2a1 (1.06) and 24...h5g4 are about equal; only 24...h2g1 wins.

### 4. Which move lost the game for Black

The book says of 18...d6f4 that "this move and all subsequent hits should have led to Black's defeat". The engine separates the idea from its execution.

- **18...d6f4** is fine: 0.95, level, and among the best three moves.
- **20...g6f5** costs a little (1.16). The book says it could not allow f6e7, but **20...b6d4!** at once holds the balance (0.96): 21.b4:d6 d4:b2 22.a2:c2 a4:a2, and Black has reached the Diagr.18 structure with the g6 man still on the board and without giving White the capture f6:f4.
- **21...b6d4?** is the losing move (1.69). **21...a7d4!** keeps the game going (1.15): 22.c4:e4 a4:e2 (a4:c4:c2:e2) 23.e3:e1.

So Diagr.18 is lost (1.70), as the book says, but the explosion on move 18 was not the reason.

### 5. Diagr.2: 6...d8b6 loses two men

The author writes that he would now choose "another path, connected with the move d8b6", to open the game. In this position it fails tactically. White's e4 man is already attacking e5, and d8b6 leaves d8 empty:

```
6...d8b6?  7.e4:e6  d6:f6    (both forced)
8.d4:d8                      (d4:d6:d8, forced; White kings)
```

After 8...b8a7 9.d8:b8 a8:c8 White has 15 men against 13. Engine: 1.40.

6...d7e6, as played, is the engine's first choice (1.02). The open plan would need different preparation.

### 6. Option 3: stronger moves for White

The analysis after 13...d5c4 14.b3a4 c4:c2 15.c1:c3! is sound as far as it goes, and 18.h4g5!! is a real find (1.77; the engine does not see it until it is played). But White has two better moves on the way.

- **Diagr.8.** After 15...b7a6 the book "did not see any dangerous continuation" until 16.f3d5 (1.13). The engine plays **16.h3g4!** h5:h3 (forced) 17.e2h5 h6:f4 18.f3:b7 — 1.61. So 15...b7a6 is not the simple answer it looks.
- **Diagr.10.** The book rejects 20.d3d5 because of 21...c5:g1 and finds 20.d3f5!! (1.41). The original idea works if the c5 man is decoyed first: **20.e4d5!** c5:e5 (forced) 21.d3d5 b3:f3 22.f2:h8 e5:c5 23.h8:f2 — 1.84. White won the play-out.

The rest is confirmed. 20.d3d5 b3:f3 21.f2:h8 c5:g1 is unclear (1.26; the play-out was drawn). Diagr.12 is good for White (1.44; one play-out was drawn and one won by White). Both side lines after 24...c8f5 25.h3g4 win (1.80 and 1.86), and the main line to 33.h5:a8 wins (1.93).

### 7. White's later chances

After 24...h5g4 White was still not worse, and passed two better moves:

- **25.e3e4** (1.13; White won the play-out) in place of 25.e5e7 (0.94).
- **27.e7e8** (1.16), kinging with the other man, in place of 27.d6c7 (0.91). The book's point that 27.e6e8 loses the king to 27...b1a1 is right (0.39), but it does not apply to e7e8.

After 27.d6c7 c8:c6 28.e6e8 the position is lost to 28...c5d4! (0.45), as the book shows.

## Smaller points

- **15.a1a2** (0.73) is weaker than 15.f3f4 (1.06), and **15...a6a4** (1.02) lets it pass; the engine prefers 15...c6a4 (0.72), though White won that play-out, so this one is only the engine's opinion.
- **18.f1g2** (0.99). The engine prefers 18.h4g5 g6:g2 (forced) 19.f1f2 (1.35).
- **26...a2b1** (1.14) against 26...a2a1 (0.93). The book says b1 was chosen "not by chance", and the game bears that out, but the engine rates a1 slightly higher because of 27.e7e8.
- **24.c3c4** (1.81) is a second winning move at Diagr.18, next to 24.h4g5!! (1.72). White won all four play-outs of the two moves.
- **24.d6e7?** The mark is right, since it gives up the win (1.08). But the line shown needs 25.e6e8? as well; 25.g3e5 keeps the game level (1.06).

## Confirmed by the engine

- Option 1 (13...e6c4, 1.24) and Option 2 (13...c6c4 14.b3a4, 1.33) are bad for Black.
- 15...b7b5? loses to 17.d3d5 (1.69), and 15...d7b5 is "slightly better" but still bad (1.28).
- At Diagr.9, 18.d3d5 fails to 18...c5:g5 (0.50); 18.h4g5!! and 19.f5g6! work (1.80 at Diagr.10).
- 15...d6f4? at Diagr.15 loses to 17.b3d5 (1.62).
- Diagr.18 is winning for White (1.70), and 24.h4g5!! h5:f3 25.d6e7 wins (Diagr.20 is 1.78).
- 24.d6e7? a2a1 25.e6e8 h2g1! 26.e8:a4 g1:e2! wins for Black (0.26).
- 24.g3e5? (0.64).
- 28...c5d4! is the only move: every other 28th move is 0.93 or worse for Black.
- Diagr.23 is won for Black (0.39), and 32.d8h8 prolongs the game without saving it (0.38; Black won the play-out).

## Editorial errata

| Line | Issue | Suggested fix |
|--:|:--|:--|
| 210–211 | Two side lines are written on one line each, three moves to a line, unlike every other variation in the chapter | Set them out as the others are, or leave as they are |
| 44–49 | The alternative 6...d8b6 | See finding 5 |
| 287 | "This move and all subsequent hits should have led to Black's defeat" | See finding 4 |

## Appendix: the main line move by move

Each number is the engine's evaluation after the move. The last column lists an alternative only where the 2M search rates it at least 0.08 better for the side to move. The move numbers are the record's, so 28 to 33 differ from the chapter's.

| No. | White | 50k | 2M | Black | 50k | 2M | Engine prefers |
|---:|:---|---:|---:|:---|---:|---:|:---|
| 1 | e3e4 | 0.96 | 0.97 | e6d5 | 1.02 | 1.02 |  |
| 2 | e2e3 | 1.01 | 1.00 | f6e5 | 1.03 | 1.02 |  |
| 3 | e4:e6 | 1.03 | 1.02 | e7:e5 | 1.05 | 1.03 |  |
| 4 | d2d4 | 1.02 | 1.00 | g8g6 | 1.07 | 1.06 |  |
| 5 | d1d2 | 1.05 | 1.04 | f7h5 | 1.07 | 1.05 |  |
| 6 | b1e4 | 1.05 | 1.02 | d7e6 | 1.03 | 1.02 |  |
| 7 | e1e2 | 1.02 | 1.01 | g7h6 | 1.01 | 1.01 |  |
| 8 | g2h3 | 1.02 | 1.01 | h8h7 | 1.07 | 1.07 |  |
| 9 | g1h2 | 1.07 | 1.06 | b8b6 | 1.08 | 1.07 |  |
| 10 | h2h4 | 1.05 | 1.06 | d8a5 | 1.05 | 1.07 |  |
| 11 | b2a3 | 1.06 | 1.08 | c7c5 | 1.08 | 1.10 |  |
| 12 | c2b3 | 1.10 | 1.09 | e8d7 | 1.09 | 1.10 |  |
| 13 | d2b4!? | 1.01 | 1.04 | b7a6 | 1.10 | 1.60 | 13...d7b5 1.04 |
| 14 | e2c4 | 0.96 | 1.06 | d7b5 | 0.95 | 1.05 | 14.d3c4 1.62 |
| 15 | a1a2 | 0.89 | 0.73 | a6a4 | 0.96 | 1.02 | 15.f3f4 1.06; 15...c6a4 0.72 |
| 16 | c1d2 | 0.51 | 0.40 | f8g7 | 0.92 | 0.96 | 16.f3f4 1.02; 16...e6f5 0.39 |
| 17 | f2g3 | 0.89 | 0.68 | a8a7 | 1.00 | 0.98 | 17.d2f4 0.97; 17...e6f5 0.65 |
| 18 | f1g2 | 0.93 | 0.99 | d6f4 | 0.97 | 0.95 | 18.h4g5 1.35 |
| 19 | d4:f6 | 0.97 | 0.95 | f4:h2 | 0.97 | 0.95 |  |
| 20 | e4:e6 | 0.97 | 0.95 | g6f5 | 1.21 | 1.16 | 20...b6d4 0.96 |
| 21 | f6:f4 | 1.21 | 1.16 | b6d4 | 1.46 | 1.69 | 21...a7d4 1.15 |
| 22 | b4:d6 | 1.46 | 1.69 | d4:b2 | 1.46 | 1.70 |  |
| 23 | a2:c2 | 1.47 | 1.70 | a4:a2 | 1.47 | 1.70 |  |
| 24 | g3e5? | 1.10 | 0.64 | h5g4 | 1.02 | 1.10 | 24.c3c4 1.81; 24...h2g1 0.20 |
| 25 | e5e7 | 0.96 | 0.94 | g4:e2 | 0.96 | 0.94 | 25.e3e4 1.13 |
| 26 | d2:f2 | 0.96 | 0.94 | a2b1 | 1.05 | 1.14 | 26...a2a1 0.93 |
| 27 | d6c7 | 1.07 | 0.91 | c8:c6 | 1.07 | 0.91 | 27.e7e8 1.16 |
| 28 | e6e8 | 1.08 | 0.81 | c5d4! | 0.53 | 0.45 |  |
| 29 | d3:d5 | 0.53 | 0.45 | c6c5 | 0.49 | 0.44 |  |
| 30 | d5:b5 | 0.49 | 0.43 | b1:f1 | 0.47 | 0.43 |  |
| 31 | e7d8 | 0.44 | 0.41 | h2g1 | 0.38 | 0.39 |  |
| 32 | c2c4 | 0.21 | 0.16 | a5b4 | 0.17 | 0.14 | 32.d8h8 0.38 |
| 33 | c4:a4 | 0.17 | 0.14 | g1g3 | 0.13 | 0.12 |  |

The 50k column is what `damleo eval run/model-12.nn <record>` prints for the moves up to that point, converted as 2·win + draw. The row for move 13 shows the engine's blind spot: at 50k the position after 13...b7a6 reads 1.10, and only the deeper search finds 14.d3c4.
