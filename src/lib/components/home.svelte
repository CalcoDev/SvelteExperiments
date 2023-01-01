<script>
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import { TextPlugin } from "gsap/TextPlugin";

	import { onMount } from "svelte";

  import SplitType from 'split-type'

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger, TextPlugin);

    let tl = gsap.timeline();

    tl.delay(0.5);
    tl.from(".box", {
      duration: 1.5,
      width: 'max(100vw, 100vh)',
      height: 'max(100vw, 100vh)',
      ease: "bounce.out",
      rotate: 180,
    })

    const title = new SplitType(".title");
    tl.from(title.chars, {
      duration: 2,
      opacity: 0,
      y: 100,
      stagger: 0.1,
      ease: "power4.out",
    }, "+=1");

    tl.play();
  });
</script>

<section class="content">
  <div class="box"></div>

  <h1 class="title">CALCO.DEV</h1>
</section>

<style>
  .box {
    background-color: #fff; 
    position: absolute;
  }

  .title {
    color: #fff;
    font-size: 5rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5rem;
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