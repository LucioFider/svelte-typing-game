<script lang="ts">
  type Game = "waiting for input" | "in progress" | "game over";
  type Word = string;

  let game: Game = "waiting for input";
  let typedLetter = "";

  let words: Word[] =
    "The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog The quick brown fox jumps over the lazy dog".split(
      " "
    );

  let wordIndex = 0;
  let letterIndex = 0;
  let correctLetters = 0;

  let wordsEl: HTMLDivElement;
  let letterEl: HTMLSpanElement;
  let inputEl: HTMLInputElement;

  function updateGameState() {
    setLetter();
    checkLetter();
    nextLetter();
    resetLetter();
  }

  function setLetter() {
    const isWordCompleted = letterIndex > words[wordIndex].length - 1;

    if (!isWordCompleted) {
      letterEl = wordsEl.children[wordIndex].children[
        letterIndex
      ] as HTMLSpanElement;
    }
  }

  function checkLetter() {
    const currentLetter = words[wordIndex][letterIndex];

    if (typedLetter === currentLetter) {
      letterEl.dataset.letter = "correct";
      increaseScore();
    }
    if (typedLetter === currentLetter) {
      letterEl.dataset.letter = "incorrect";
    }
  }

  function increaseScore() {
    correctLetters += 1;
  }

  function nextLetter() {
    letterIndex += 1;
  }

  function resetLetter() {
    typedLetter = "";
  }

  function startGame() {
    setGameState("in progress");
  }

  function setGameState(state: Game) {
    game = state;
  }

  function handleKeydown(event: KeyboardEvent) {
    if (event.code === "Space") {
      event.preventDefault();
    }
    if (game === "waiting for input") {
      startGame();
    }
  }
</script>

<div class="game" data-game={game}>
  <input
    bind:this={inputEl}
    bind:value={typedLetter}
    on:input={updateGameState}
    on:keydown={handleKeydown}
    class="input"
    type="text"
  />
</div>

<div bind:this={wordsEl} class="words">
  {#each words as word}
    <span class="word">
      {#each word as letter}
        <span class="letter">{letter}</span>
      {/each}
    </span>
  {/each}
</div>

<style lang="scss">
  .words {
    --line-height: 1em;
    --lines: 3;

    width: 100%;
    max-height: calc(var(--line-height) * var(--lines) * 1.42);
    display: flex;
    flex-wrap: wrap;
    gap: 0.6em;
    position: relative;
    font-size: 1.5rem;
    line-height: var(--line-height);
    overflow: hidden;
    user-select: none;

    .letter {
      opacity: 0.4;
      transition: all 0.3s ease;

      &:global([data-letter="correct"]) {
        opacity: 0.8;
      }
      &:global([data-letter="incorrect"]) {
        color: var(--primary);
        opacity: 1;
      }
    }
  }
</style>
