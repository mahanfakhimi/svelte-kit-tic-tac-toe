<script>
  import Square from "./Square.svelte";

  import CircleIcon from "./Icons/CircleIcon.svelte";
  import XIcon from "./Icons/XIcon.svelte";

  const getWinner = (board) => {
    const winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];

    for (let combination of winningCombinations) {
      const [first, second, third] = combination;

      if (
        board?.[first] &&
        board[first] === board[second] &&
        board[first] === board[third]
      ) {
        return board[first];
      }
    }

    return null;
  };

  const DEFAULT_BOARD_STATE = Array(9).fill(null);
  const DELAY_BEFORE_RESET = 250;

  const MESSAGES = {
    GAME_OVER: (winner) => `بازی تمام شد! ${winner} برنده شد!`,
    DRAW: "بازی مساوی شد!",
    GAME_TITLE: "بازی دوز",
    SCORE_FORMAT: "{o} - {x}",
  };

  let boardState = $state(DEFAULT_BOARD_STATE);
  let currentPlayerIsX = $state(true);
  let playerScores = $state({ x: 0, o: 0 });
  let winner = $derived(getWinner(boardState));

  const handleSquareClick = (index) => {
    if (getWinner(boardState) || boardState[index]) return;

    const updatedBoard = [...boardState];
    updatedBoard[index] = currentPlayerIsX ? "X" : "O";
    boardState = updatedBoard;
    currentPlayerIsX = !currentPlayerIsX;

    const winner = getWinner(boardState);

    if (winner) {
      setTimeout(() => {
        alert(MESSAGES.GAME_OVER(winner));
        boardState = DEFAULT_BOARD_STATE;
        currentPlayerIsX = true;
        winner === "X" ? playerScores.x++ : playerScores.o++;
      }, DELAY_BEFORE_RESET);
    } else if (!winner && boardState.every((square) => square)) {
      alert(MESSAGES.DRAW);
      boardState = DEFAULT_BOARD_STATE;
      currentPlayerIsX = true;
    }
  };
</script>

<div>
  <p class="text-center mb-2">{MESSAGES.GAME_TITLE}</p>

  <p class="text-center mb-2">
    {MESSAGES.SCORE_FORMAT.replace("{x}", playerScores.x).replace(
      "{o}",
      playerScores.o
    )}
  </p>

  <div class="flex items-center gap-x-4">
    <div style:opacity={currentPlayerIsX ? 0.25 : 1}>
      <CircleIcon />
    </div>

    <div class="grid grid-cols-3 gap-2">
      {#each boardState as squareValue, index}
        <Square value={squareValue} onClick={() => handleSquareClick(index)} />
      {/each}
    </div>

    <div style:opacity={!currentPlayerIsX ? 0.25 : 1}>
      <XIcon />
    </div>
  </div>
</div>
