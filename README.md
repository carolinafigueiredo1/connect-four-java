# Connect Four — Java Game Engine

A full-featured Connect Four game, built for the Advanced Programming course (ISEC, 
2020/2021). Features both a text-based and a JavaFX graphical interface, backed by 
a state machine architecture with an Adapter layer, and the Memento pattern for 
save/undo/replay functionality.

## Features

- **Game modes** — Human vs Human, Human vs Computer, Computer vs Computer
- **Special pieces** — earned by winning a minigame, grants an extra move
- **Undo system** — rewind previous moves using a credit-based system
- **Save & load** — persist game state to file, resume later (up to 5 saved games)
- **Replay** — step through the move history of a saved game
- **Two minigames** — unlockable side-challenges: a math quiz and a word-typing challenge
- **Game logs** — full move history, viewable in-session and across saved games

## Architecture

### Logic layer (`logica.dados`, `logica.estados`, `logica.memento`)

- **dados** — core game data: `Jogo` (main game logic), `Jogador` (player info), 
  `Dicionario` (word bank for Minigame 2), and the `MiniJogo` hierarchy 
  (`MiniJogo1`: math quiz, `MiniJogo2`: word-typing — sharing a common base class 
  via inheritance)
- **estados** — implements the **State design pattern**, with `EstadoAdapter` (abstract 
  class) as an Adapter layer providing shared state elements, and one concrete state 
  per game moment: `Inicio`, `AguardaJogada`, `AguardaDecisaoMJ`, `AguardaMiniJogo`, 
  `AguardaDecisaoPeca`, `AguardaDecisaoFinal`, `Fim`. `MaqEstados` delegates behavior 
  to the current state object rather than branching manually
- **memento** — implements the **Memento design pattern** with its three classic roles: 
  `MaqEstados` as Originator, `Memento` as the state snapshot, `CareTaker` managing 
  when to save/restore — powering the undo-credits system, save/load, and replay

### Text UI (`ui.texto`)

Entry point: `Main` → `UItexto`. A loop driven entirely by the current state — each 
game moment has its own UI method, keeping presentation logic cleanly separated per phase.

### Graphical UI (`ui.gui`)

Entry point: `JogoGalojfx`. Creates the state machine, wraps it in `JogoObservavel`, and 
uses `PropertyChangeSupport` to notify views of state changes. `PaneOrganizer` connects 
the observable game to `PrincipalPane`, which manages state-specific panes 
(`InicioPane`, `AguardaJogadaPane`, `AguardaMiniJogoPane`, `AguardaDecisaoFinalPane`) 
for rendering.

## Tech

Java · JavaFX · Java Serialization (save/load persistence) · 
design patterns: State, Adapter, Memento
