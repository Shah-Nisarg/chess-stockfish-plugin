# Chess Explainer: a plugin for Claude

Ask Claude why the engine says +3. This plugin connects Claude to Stockfish 17.1 running at
`https://chess.tinyapps.link/mcp` (free, no sign-in) and adds a skill that teaches Claude to explain
positions from engine lines instead of guessing.

Paste a screenshot, a FEN or a game, and ask:
- "Why is this +3 for White?"
- "Why can't Black just take the bishop?"
- "What about 17...g6?"

Claude analyses the position, checks every "why not this move?" with the engine, explains the reason
in plain words, and publishes a page where you can step through the lines on a board.

## Install

**Claude Code:**
```
/plugin install chess-stockfish --marketplace Shah-Nisarg/chess-stockfish-plugin
```

**claude.ai and the Claude apps:** add the connector on its own. Go to Settings → Connectors → Add custom
connector, name it "Chess", and use the URL `https://chess.tinyapps.link/mcp`. No sign-in is needed.

## What's inside
- `.mcp.json`: the remote connector (tools `analyse_position`, `check_move`, `show_line`).
- `skills/explain-chess-position`: how to read a board from a screenshot, when to call each tool, and
  how to explain without inventing tactics.

## Contact
chess@tinyapps.link, or open an issue in this repository.

## Privacy
Only positions, moves and page notes are sent to the server. See https://chess.tinyapps.link/privacy.html.

## Licence
The plugin is MIT licensed. Stockfish and python-chess are GPL-3.0 and run on the server.

Chess Explainer uses the Stockfish engine and is not affiliated with the Stockfish project.
