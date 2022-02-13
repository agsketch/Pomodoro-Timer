<script>
  /* Edit Lock - Variables */
  let editState = false;
  $: editIcon = editState ? "unlock" : "lock";

  /* Edit Lock - Functions */
  const toggleEdit = () => {
    if (!running) {
      editState = !editState;
      duration = [minutes, seconds];
    }
  };

  /* Timer - Variables */
  let running = false;
  let duration = [5, 0];
  let minutes = "05";
  let seconds = "00";

  /* Timer - Functions */
  const toggleTimer = () => {
    running = !running;
    running && (editState = false);
  };

  setInterval(() => {
    if (running) {
      try {
        minutes = parseInt(minutes);
        seconds = parseInt(seconds);

        if (seconds > 59 || Number.isNaN(minutes) || Number.isNaN(seconds)) {
          throw "Please Enter A Valid Input";
        }
      } catch (error) {
        minutes = "05";
        seconds = "00";
        running = false;
        alert(error);
      }

      minutes = parseInt(minutes);
      seconds = parseInt(seconds);

      if (--seconds < 0) {
        if (--minutes < 0) {
          minutes = duration[0];
          seconds = duration[1];
        } else {
          seconds = 59;
        }
      }

      minutes = minutes.toString().padStart(2, "0");
      seconds = seconds.toString().padStart(2, "0");
    }
  }, 1000);
</script>

<div class="container">
  <div class="card">
    <div class="time">
      <input
        type="text"
        disabled={!editState}
        bind:value={minutes}
        maxlength="2"
        name="minutes"
      />
      <p>:</p>
      <input
        type="text"
        disabled={!editState}
        bind:value={seconds}
        maxlength="2"
        name="seconds"
      />
    </div>

    <button disabled={editState} on:click={toggleTimer}>
      {running ? "pause" : "start"}
    </button>

    <img
      on:click={toggleEdit}
      class:disabled={running}
      src={`assets/${editIcon}.svg`}
      alt="Timer Edit Lock"
    />
  </div>
</div>

<style>
  .container {
    box-sizing: border-box;
    height: 100%;
    width: 100%;
    display: grid;
    place-items: center;
  }

  .card {
    width: 90%;
    aspect-ratio: 1;
    max-width: 500px;
    max-height: 500px;
    border-radius: 100%;

    background-color: var(--gray-300);
    border: 5px solid var(--gray);

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .time {
    display: flex;
    align-items: center;
  }

  input {
    box-sizing: border-box;
    display: inline;
    text-align: center;
    font-size: 3rem;
    padding: 20px 10px;
    width: 6rem;
    color: var(--gray);
  }

  p {
    font-size: 3rem;
    margin-bottom: 10px;
  }

  button {
    padding: 15px 25px;
    text-align: center;
    background-color: var(--gray);
    color: var(--gray-400);
    font-size: 1.2rem;
    text-transform: uppercase;
    letter-spacing: 2px;
  }

  button:hover,
  button:active,
  img:hover {
    transform: scale(1.04);
  }
  
  button:disabled {
    background-color: var(--gray-200);
    transform: scale(1.04);
    cursor: not-allowed;
  }

  .disabled {
    cursor: not-allowed;
    transform: scale(1.04);
  }

  button,
  img {
    transition: all 75ms ease-in-out;
    cursor: pointer;
  }

  img {
    margin-top: 20px;
    padding: 5px;
  }
</style>
