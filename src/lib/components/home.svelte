<script>
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import { TextPlugin } from "gsap/TextPlugin";

	import { onMount } from "svelte";

  import SplitType from 'split-type'

  let boxEnter;
  let heroTitle;
  
  let nav;
  let footer;

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger, TextPlugin);

    let masterTl = gsap.timeline();
    let boxTl = gsap.timeline();
    
    let navTl = gsap.timeline();
    let footerTl = gsap.timeline();
    let heroTl = gsap.timeline();

    boxTl.from(boxEnter, {
      duration: 1.5,
      width: 'max(100vw, 100vh)',
      height: 'max(100vw, 100vh)',
      rotation: 180,
      ease: "bounce.out",
    })
    boxTl.to(boxEnter, {
      duration: 0.01,
      width: 0,
      height: 0,
    })

    navTl.from(nav, {
      duration: 1,
      opacity: 0,
      y: -100,
      ease: "power4.out",
    });

    footerTl.from(footer, {
      duration: 1,
      opacity: 0,
      y: 100,
      ease: "power4.out",
    });

    const title = new SplitType(heroTitle);
    heroTl.from(title.chars, {
      duration: 2,
      opacity: 0,
      y: 100,
      stagger: 0.1,
      ease: "power4.out",
    });

    masterTl.delay(0.5);
    masterTl.add(boxTl);
    masterTl.add(heroTl);
    masterTl.add([navTl, footerTl]);
    masterTl.play();
  });
</script>

<div class="box-enter" bind:this={boxEnter}></div>
<div class=""></div>
<section class="content">
  <nav bind:this={nav}>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>

  <main>
    <section class="section-hero">
      <h1 class="hero-title" bind:this={heroTitle}>CALCO.DEV</h1>
    </section>
  </main>

  <footer bind:this={footer}>
    <span>made by <strong>Calcopod</strong></span>
  </footer>
</section>

<style>
  :root {
    --color-paster-3: #d5bdaf;
    --color-pastel-2: #e3d5ca;
    --color-pastel-1: #f5ebe0;
    --color-pastel-0: #fffefc;

    --color-primary-3: #22223b;
    --color-primary-2: #4a4e69;
    --color-primary-1: #9a8c98;
    
    --color-secondary-3: #344e41;
    --color-secondary-2: #3a5a40;
    --color-secondary-1: #588157;
  }

  .box-enter {
    background-color: var(--color-pastel-0); 
    position: absolute;

    left: 50%;
    top: 50%;

    transform-origin: center;
    transform: translate(-50%, -50%);

    /* For proper scaling. Later set to 0. */
    width: 0.1px;
    height: 0.1px;
  }

  .content {
    height: 100vh;
    width: 100vw;
    background-color: var(--color-pastel-1);

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;

    overflow: hidden;
  }

  nav {
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vh;

    height: 10vh;
  }

  nav ul {
    width: 80vw;
    height: 100%;

    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  nav ul li {
    flex: 1;
    list-style: none;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  nav ul li a {
    color: var(--color-primary-3);
    font-size: 1rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5rem;
    text-decoration: none;
    text-align: center;

    transition: all 0.3s ease-in-out;
  }

  nav ul li a:hover {
    color: var(--color-primary-2);
  }

  main {
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vh;
  }

  .section-hero {
    height: 80vh;
    width: 100%;
    
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .hero-title {
    color: var(--color-primary-3);
    font-size: 5rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5rem;

    clip-path: inset(0% 0 0 0);
  }

  footer {
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vh;
    height: 10vh;

    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>