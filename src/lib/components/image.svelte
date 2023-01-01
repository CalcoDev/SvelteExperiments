<script>
	import { fly } from "svelte/transition";

  let className = "";
  export {className as class};
  
  export let first = "😺";
  export let second = "🐶";

  export let left = false;
  export let top = false;

  let yeet = 200;

  let showFirst = true;

  function toggle() {
    showFirst = !showFirst;
  }

  function getHiddenX(isIn, isFirst) {
    if (isFirst)
      return 0;

    if (!isIn) {
      if (left && top)
        return -1;
      if (!left && !top)
        return 1;
      if (!left && top)
        return 1;
      if (left && !top)
        return -1;
    }

    return 0;
  }

  function getHiddenY(isIn, isFirst) {
    if (!isFirst)
      return 0;

    if (!isIn) {
      if (left && top)
        return -1;
      if (!left && !top)
        return 1;
      if (!left && top)
        return -1;
      if (left && !top)
        return 1;
    }

    return 0;
  }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class={className} on:click={toggle} >
  <img 
    src={first}
    alt="first"
    class={`first ${showFirst ? "shown" : ""}`}
    style={
      `transform: translate(${getHiddenX(showFirst, true) * 100}%, ${getHiddenY(showFirst, true) * 100}%)`
    }
  />
  <img 
    src={second}
    alt="second"
    class={`second ${showFirst ? "" : "shown"}`}
    style={
      `transform: translate(${getHiddenX(!showFirst, false) * 100}%, ${getHiddenY(!showFirst, false) * 100}%)`
    }
  />
</div>

<style>
  /* The div should always display at most 1 child */
  div {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    position: relative;
    overflow: hidden;
  }

  div img {
    font-size: 5rem;
    /* z-index: -1; */

    position: static;
    transition: transform 0.5s ease-in-out;
    position: absolute;
    background-clip: padding-box;

    width: 100%;
    height: 100%;
  }

  .shown {
  }
</style>