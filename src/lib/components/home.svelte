<script>
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
	import { onMount } from "svelte";

  let one, two, three;

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);

    let xOne = one.getBoundingClientRect().x;
    let xTwo = two.getBoundingClientRect().x;
    let xThree = three.getBoundingClientRect().x;

    let width = 100;
    let height = 100;

    let dAB = xTwo - xOne;
    let dBC = xTwo - xThree;

    let tl = gsap.timeline();
    tl.from(".one", { rotate: 360, scale: 0, duration: 1}, 0 );
    tl.from(".two", { rotate: 180, scale: 0, duration: 1}, 0 );
    tl.from(".three", { rotate: 470, scale: 0, duration: 1}, 0 );

    tl.to(".one", { duration: 1, x: dAB - width});
    tl.to(".three", { duration: 1, x: dBC + width});
    
    tl.to(".box", { duration: 1, y: -window.innerHeight * 0.5 + height, opacity: 0 });
  });
</script>

<section class="content">
  <div class="box one" bind:this={one}></div>
  <div class="box two" bind:this={two}></div>
  <div class="box three" bind:this={three}></div>
</section>

<style>
  .box {
    width: 100px;
    height: 100px;

    background-color: #fff; 
  }

  .content {
    height: 100vh;
    width: 100vw;
    background-color: #2e2e2e;

    display: flex;
    justify-content: space-evenly;
    align-items: center;

    overflow: hidden;
  }
</style>