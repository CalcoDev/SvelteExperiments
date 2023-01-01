<script>
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import { TextPlugin } from "gsap/TextPlugin";

	import { onMount } from "svelte";

  import SplitType from 'split-type'

  let boxEnterRef;
  let heroTitleRef;
  
  let navRef;
  let footerRef;
  
  let cursorRef;

  onMount(() => {
    const setCursorX = gsap.quickTo(cursorRef, "x", {duration: 0.3, ease: "power3"});
    const setCursorY = gsap.quickTo(cursorRef, "y", {duration: 0.3, ease: "power3"}); 
    
    function handleMouseMove(e) {
      setCursorX(e.clientX);
      setCursorY(e.clientY);
    }

    function handleMouseDown(e) {
      gsap.to(cursorRef, { scale: 2, duration: 0.1});
    }

    function handleMouseUp(e) {
      gsap.to(cursorRef, { scale: 1, duration: 0.1});
    }

    gsap.set(cursorRef, { xPercent: -50, yPercent: -50 });
    document.addEventListener("mousemove", handleMouseMove);
    document.addEventListener("mousedown", handleMouseDown);
    document.addEventListener("mouseup", handleMouseUp);

    gsap.registerPlugin(ScrollTrigger, TextPlugin);

    let masterTl = gsap.timeline();
    let boxTl = gsap.timeline();
    
    let navTl = gsap.timeline();
    let footerTl = gsap.timeline();
    let heroTl = gsap.timeline();

    let wobbleTl = gsap.timeline();

    boxTl.from(boxEnterRef, {
      duration: 1.5,
      width: 'max(100vw, 100vh)',
      height: 'max(100vw, 100vh)',
      rotation: 180,
      ease: "bounce.out",
    })
    boxTl.to(boxEnterRef, {
      duration: 0.01,
      width: 0,
      height: 0,
    })

    navTl.from(navRef, {
      duration: 1,
      opacity: 0,
      y: -100,
      ease: "power4.out",
    });

    footerTl.from(footerRef, {
      duration: 1,
      opacity: 0,
      y: 100,
      ease: "power4.out",
    });

    const title = new SplitType(heroTitleRef);
    heroTl.from(title.chars, {
      duration: 2,
      opacity: 0,
      y: 100,
      stagger: 0.1,
      ease: "power4.out",
    });

    title.chars.forEach((char, i) => {
      const tl = gsap.timeline({ repeat: -1, yoyo: true });
      tl.to(char, {
        duration: 0.75,
        y: '+=25',
        repeat: -1,
        yoyo: true,
      })
      tl.to(char, {
        duration: 0.75,
        y: '-=25',
        repeat: -1,
        yoyo: true,
      })
      wobbleTl.add(tl, i * 0.1);
    })

    masterTl.delay(0.5);
    masterTl.add(boxTl);
    masterTl.add(heroTl);
    masterTl.add([navTl, footerTl]);
    masterTl.add(wobbleTl)
    masterTl.play();
    
    // Skip to the end of the animation
    masterTl.seek(masterTl.duration());

    // Split all nav links into chars
    const navLinks = navRef.querySelectorAll('a');
    navLinks.forEach((link) => {
      const split = new SplitType(link);
    })
  });

  function handleMouseEnter(e) {
    let chars = e.path[0].querySelectorAll('.char');
    gsap.to(chars, {
      duration: 0.5,
      y: '+=25',
      stagger: 0.1,
      ease: "power4.out",
    })
  }

  // function handleMouseLeave(e) {
  //   let chars = e.path[0].querySelectorAll('.char');
  //   gsap.to(chars, {
  //     duration: 0.5,
  //     y: '0',
  //     stagger: 0.1,
  //     ease: "power4.out",
  //   })
  // }
</script>

<div class="cursor" bind:this={cursorRef}></div>

<div class="box-enter" bind:this={boxEnterRef}></div>
<section class="content">
  <nav bind:this={navRef}>
    <ul>
      <li><a href="/" on:mouseenter={handleMouseEnter}>Home</a></li>
      <li><a href="/about">About</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>

  <main>
    <section class="section-hero">
      <h1 class="hero-title" bind:this={heroTitleRef}>CALCO.DEV</h1>
    </section>
  </main>

  <footer bind:this={footerRef}>
    <span>made by <strong>Calcopod</strong></span>
  </footer>
</section>

<style>
  :root {
    --color-pastel-3: #d5bdaf; /* 213 189 175 */
    --color-pastel-2: #e3d5ca; /* 227 213 202 */
    --color-pastel-1: #f5ebe0; /* 245 235 224 */
    --color-pastel-0: #fffefc; /* 255 254 252 */

    /*
    pastel 1 - primary 3
    */

    --color-primary-3: #22223b; /* 34 34 59 */
    --color-primary-2: #4a4e69; /* 74 78 105 */
    --color-primary-1: #9a8c98; /* 154 140 152 */
    
    --color-secondary-3: #344e41; /* 52 78 65 */
    --color-secondary-2: #3a5a40; /* 58 90 64 */
    --color-secondary-1: #588157; /* 88 129 87 */
  }

  :global(body) {
    user-select: none;
    overflow: hidden;
  }

  .cursor {
    position: absolute;
    top: 0px;
    left: 0px;

    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-color: #fff;

    z-index: 1000;
    pointer-events: none;

    mix-blend-mode: difference;
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
    color: var(--color-secondary-3);
    font-size: 1rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5rem;
    text-decoration: none;
    text-align: center;

    transition: all 0.3s ease-in-out;
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