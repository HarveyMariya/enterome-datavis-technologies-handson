<script>
    let clicked = 0;

    function handleClick(_event) {
        clicked += 1;
    }

    function handleReset(_event) {
        clicked = 0;
    }
</script>

<button class = "btn btn-primary" on:click= {handleClick}> Clicked {clicked} times</button>
<button class = "btn btn-danger" on:click= {handleReset}>Reset </button>