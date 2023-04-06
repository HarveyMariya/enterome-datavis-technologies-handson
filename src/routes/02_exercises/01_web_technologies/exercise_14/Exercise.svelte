<script>
  let current = "";
</script>

<!-- <style>
  .selected {
    background-color: blue;
    color: white;
  }
</style> -->

<button class="btn btn-secondary" class:btn-success={current === "messi"} on:click={() => (current = 'messi')}>
  Messi
</button>

<button class="btn btn-secondary" class:btn-success={current === "ronaldo"} on:click={() => (current = 'ronaldo')}>
  Ronaldo
</button>

<button class="btn btn-secondary" class:btn-success={current === "neymar"} on:click={() => (current = 'neymar')}>
  Neymar
</button>


<!-- <p class= "this_paragraph">fake </p>
<p class:this_paragraph={true}>fake</p> -->


<!-- <script>
  let current = "";
  let lastSelected = null;

  function handleSelection(event) {
    if (lastSelected) {
      lastSelected.classList.remove("selected");
    }
    event.target.classList.add("selected");
    lastSelected = event.target;
    current = event.target.innerText.toLowerCase();
  }
</script>

<style>
  .btn-secondary.selected {
    background-color: red;
    color: white;
  }
</style>

<button class="btn btn-secondary" class:selected={current === "messi"} on:click={handleSelection}>
  Messi
</button>

<button class="btn btn-secondary" class:selected={current === "ronaldo"} on:click={handleSelection}>
  Ronaldo
</button>

<button class="btn btn-secondary" class:selected={current === "neymar"} on:click={handleSelection}>
  Neymar
</button> -->
