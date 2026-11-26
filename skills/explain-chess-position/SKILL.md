---
name: explain-chess-position
description: Explain a chess position or move with the Stockfish tools - why the engine prefers a move, why a tempting move fails, what a score like +3 means - and give the user a step-through board page. Use when the user shares a chess screenshot, FEN, PGN or game moves, or asks about an engine evaluation, a blunder, or "what about this move?".
---

# Explaining a chess position with Stockfish

The `chess` connector gives you three tools: `analyse_position`, `check_move` and `show_line`.
Your job is to turn engine lines into a reason a club player understands, without inventing anything.

## 1. Get the position exactly right
- From a screenshot: list every piece by square, decide whose move it is, and write the FEN. Check the
  board orientation (coordinates, or which side's pieces are at the bottom). If any square is unclear,
  or the side to move isn't shown, say what you read and ask the user to confirm before analysing.
- From a game: pass `moves` (SAN) instead of building a FEN by hand.
- The tools reject illegal positions. If that happens, re-read the screenshot; don't guess.

## 2. Ask the engine before you explain
- Call `analyse_position` as soon as you have the FEN. Don't work out variations yourself first: the
  engine is faster and more reliable, and your own lines are what lead to invented tactics.
- `analyse_position` gives the best lines (3 by default). Read all of them: the gap between line 1 and line 2
  tells you whether the best move is the only good move.
- Read the **threat check**: what the opponent would do if the side to move passed. Often the best move
  is good because it creates a threat (making the opponent's natural move impossible) or stops one.
- For every "why not X?" (the user's or one you want to rule out), call `check_move`. Never say a move
  fails, wins material or allows mate unless a line you received shows it.
- If the key idea comes several moves into a line, check the position at that point with
  `analyse_position` and the `moves` list, so you can explain what happens there.

## 3. Explain
- Lead with the answer in one or two sentences ("Qh3 threatens mate on h7, so Black has no time to take
  the bishop").
- Then the lines that prove it, each with a short reason. Name squares and pieces ("the d3 bishop").
- Scores are from White's side, in pawns. +3 means White is about three pawns better; "decisive" means
  winning. Material at the end of a line tells you who came out ahead.
- Say what you are unsure of. Positional judgements that no line shows are your opinion; label them.
- Don't describe tactics that aren't in the lines: pins, forks, mates, trapped pieces and "only moves"
  must all be visible in the engine output. A pin needs the pinned piece and the piece behind it on the
  same line as the attacker; check the squares before calling something pinned.
- Use the material counts the tools give ("White is up 2") rather than counting yourself.

## 4. Show it on a board
Most people can't follow long notation. After explaining, call `show_line` with:
- a title (e.g. "Why Qh3 wins") and 2-4 summary sentences;
- the main line and the one or two refuted alternatives the user cares about, each as a line of moves
  from the page's start position, with a short note per move that says why (notes[0] is the start);
- arrows for the key idea (`good`), threats (`threat`) and blocked lines (`blocked`);
- `flip: true` when the user is playing Black.
Give the user the link. Pages are public, so keep personal information out of them.

## Limits
Depth is capped at 24 and each search at about 20 seconds. For very sharp positions, say that the engine
assessment may change with more depth.
