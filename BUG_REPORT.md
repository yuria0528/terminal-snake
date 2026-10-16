### Situation
- Product: Terminal Snake
- Component: SnakeGame.tick (collision detection)
- Platform / hardware: Windows 11, x64
- Operating system: Windows 11
- Affected version / commit: 8357d14 (initial commit)
- Java version (if relevant): Java 17 (Adoptium 17.0.20)

### Bug description
On a move that does not eat food, moving the head into the cell currently occupied by the tail is incorrectly reported as a self-collision (status = SELF), ending the round, even though the rule says the tail leaves its old cell on such a move so moving into it is legal.

### Severity
Major. It makes a legal move end the round, so the snake is killed on a valid move. This breaks the "moving into the old tail cell is legal" rule and the LOOP classroom case.

### Steps to reproduce
1. Starting position: LOOP (snake = (2,2) head, (2,3), (1,3), (1,2) tail; direction UP; food (6,4))
2. Ordered inputs: A (LEFT), A (LEFT)

Run: `--scenario LOOP --replay AA`

### Expected result
The move into the tail's old cell is legal. Head moves from (2,2) to (1,2), length stays 4, score stays 0, status remains RUNNING.

### Actual result
On the first A move, the head at (2,2) moving LEFT into the tail cell (1,2) is reported as:
`status=SELF head=Cell[x=2, y=2] length=4 score=0 food=Cell[x=6, y=4]`
The head does not move, length stays 4, and the round ends immediately.