<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';

	import { onMount } from "svelte";

  import SplitType from 'split-type'

  let cursorRef;

  let cursorY;

  let setCursorX;
  let setCursorY;
  let setCursorScale;

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);

    setCursorX = gsap.quickTo(cursorRef, "x", {duration: 0.3, ease: "power3"});
    setCursorY = gsap.quickTo(cursorRef, "y", {duration: 0.3, ease: "power3"}); 
    setCursorScale = (scl, dur) => gsap.to(cursorRef, {duration: dur ? dur : 0.15, scale: scl, ease: "power3"});

    function updateMousePos(e) {
      if (e.type === 'scroll') {
        setCursorY(cursorY + window.scrollY);
        return;
      }
      else if (e.type === 'mousemove') {
        setCursorX(e.clientX);
        setCursorY(e.clientY + window.scrollY);
        cursorY = e.clientY;
      }
    }

    gsap.set(cursorRef, { xPercent: -50, yPercent: -50 });
    document.addEventListener("mousemove", updateMousePos);
    document.addEventListener("scroll", updateMousePos);
    document.addEventListener("mousedown", () => setCursorScale(2));
    document.addEventListener("mouseup", () => setCursorScale(1));

  
    let masterTl = gsap.timeline();
    let boxTl = gsap.timeline();
    
    let navTl = gsap.timeline();
    let footerTl = gsap.timeline();
    let heroTl = gsap.timeline();

    let wobbleTl = gsap.timeline();

    boxTl.from(".box-enter", {
      duration: 1.5,
      width: 'max(100vw, 100vh)',
      height: 'max(100vw, 100vh)',
      rotation: 180,
      ease: "bounce.out",
    })
    boxTl.to(".box-enter", {
      duration: 0.01,
      width: 0,
      height: 0,
    })

    navTl.from("nav", {
      duration: 1,
      opacity: 0,
      y: -100,
      ease: "power4.out",
    });

    footerTl.from("footer", {
      duration: 1,
      opacity: 0,
      y: 100,
      ease: "power4.out",
    });

    const title = new SplitType(".hero-title");
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
    
    // Skip to the end of the animation
    // TODO(calco): REMOVE THIS IN PRODUCTION
    masterTl.seek(masterTl.duration());

    masterTl.add(wobbleTl)
    masterTl.play();

    // Split all nav links into chars
    const navLinks = document.querySelector('nav').querySelectorAll('a');
    navLinks.forEach((link) => {
      new SplitType(link);
    })

    // gsap.from(".binder h1, p", {
    //   duration: 1,
    //   opacity: 0,
    //   x: '-100%',
    //   scrollTrigger: {
    //     trigger: ".binder",
    //     start: "top center",
    //     end: "bottom center",
    //     scrub: true,
    //     markers: true,
    //     pin: true
    //   }
    // });
    // gsap.from(".binder img", {
    //   duration: 1,
    //   opacity: 0,
    //   x: '100%',
    //   scrollTrigger: {
    //     trigger: ".binder",
    //     start: "top center",
    //     end: "center center",
    //     scrub: true,
    //     markers: true,
    //   }
    // });
  });

  function wobbleSelf(e) {
    let chars = e.srcElement.querySelectorAll('.char');

    // Guys this is really bad code I am sorry for any github copilot users who might get this recommendation
    gsap.to(chars, {
      duration: 0.75,
      y: '-25',
      stagger: 0.1,
      ease: "power4.out",
    })
    gsap.delayedCall(0.1, () => {
      gsap.to(chars, {
        duration: 0.75,
        y: '0',
        stagger: 0.1,
        ease: "elastic.out(1, 0.3)",
      })
    })
  }

  function enlargeCursor(scl) {
    setCursorScale(typeof scl === 'number' ? scl : 2);
  }

  function resetCursor() {
    setCursorScale(1);
  }

  function shrinkCursor(scl) {
    setCursorScale(typeof scl === 'number' ? scl : 0.5);
  }

  function navLinkMouseEnter(e) {
    wobbleSelf(e);
    enlargeCursor();
  }

  function navLinkMouseLeave(e) {
    resetCursor();
  }

  let scrollYVal;
</script>

<svelte:window bind:scrollY={scrollYVal} />

<div class="cursor" bind:this={cursorRef}></div>
<div class="box-enter"></div>
<nav
  style={
    `background-color: ${scrollYVal > 0 ? 'var(--color-pastel-2)' : 'var(--color-pastel-1)'};` +
    `border-color: ${scrollYVal > 0 ? 'var(--color-pastel-3)' : 'var(--color-pastel-1)'};`
  }>
  <ul>
    <li><a href="/" on:mouseenter={navLinkMouseEnter} on:mouseleave={navLinkMouseLeave}>Home</a></li>
    <li><a href="/about" on:mouseenter={navLinkMouseEnter} on:mouseleave={navLinkMouseLeave}>About</a></li>
    <li><a href="/contact" on:mouseenter={navLinkMouseEnter} on:mouseleave={navLinkMouseLeave}>Contact</a></li>
  </ul>
</nav>

<main>
  <section class="section hero">
    <h1 class="hero-title" on:mouseenter={enlargeCursor} on:mouseleave={resetCursor}>CALCO.DEV</h1>
  </section>

  <section class="section flex screen-height-2 binder">
    <div class="flex-1">
      <h1 class="section-heading">Pro Dev Man</h1>
      <p class="section-desc">
        See that? That is me. Why? Because. <br/>
        I am very a pro developer man. In the words of dm: <br/> <br/> 
        "Give me barometru cuz they call me Pascal." <br/> 
        - DM, 2022
      </p>
    </div>
    <div class="flex-1 center">
      <img 
        class="section-image"
        src="https://images.unsplash.com/photo-1551434678-e076c223a692?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
        alt="Me"
        on:mouseenter={enlargeCursor}
        on:mouseleave={resetCursor}
      />
    </div>
  </section>
</main>

<footer>
  <span on:mouseenter={enlargeCursor} on:mouseleave={resetCursor}>made by <strong>Calcopod</strong></span>
</footer>

<style>
  :root {
    --color-pastel-3: #d5bdaf; /* 213 189 175 */
    --color-pastel-2: #e3d5ca; /* 227 213 202 */
    --color-pastel-1: #f5ebe0; /* 245 235 224 */
    --color-pastel-0: #fffefc; /* 255 254 252 */

    --color-primary-3: #22223b; /* 34 34 59 */
    --color-primary-2: #4a4e69; /* 74 78 105 */
    --color-primary-1: #9a8c98; /* 154 140 152 */
    
    --color-secondary-3: #344e41; /* 52 78 65 */
    --color-secondary-2: #3a5a40; /* 58 90 64 */
    --color-secondary-1: #588157; /* 88 129 87 */
  }

  :global(body) {
    user-select: none;
    overflow-x: hidden;
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
    background-color: var(--color-primary-3); 
    position: absolute;

    left: 50%;
    top: 50%;

    transform-origin: center;
    transform: translate(-50%, -50%);

    /* For proper scaling. Later set to 0. */
    width: 0.1px;
    height: 0.1px;
  }

  nav {
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vw;

    position: sticky;
    top: 0;

    transition: background-color 1s ease-out, border-color 1s ease-out;

    border-bottom: 10px solid;
    z-index: 999;

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
    color: var(--color-secondary-1);
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
    padding-right: 10vw;

    background-color: var(--color-pastel-1);
  }

  .section {
    height: 80vh;
    width: 100%;
  }

  .section.hero {
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
    padding-right: 10vw;
    height: 10vh;

    display: flex;
    justify-content: center;
    align-items: center;

    background-color: var(--color-pastel-1);
  }

  .flex {
    display: flex;
  }

  .flex-1 {
    flex: 1;
  }

  .center {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .section-heading {
    color: var(--color-primary-3);
    font-size: 3rem;
    font-weight: 700;
  }

  .section-desc {
    color: var(--color-primary-1);
    font-size: 0.8rem;
    font-weight: 400;
  }

  .screen-height-2 {
    height: 160vh;
  }

  .section-image {
    width: 100%;
    aspect-ratio: 16/9;
    object-fit: cover;
  }
</style>