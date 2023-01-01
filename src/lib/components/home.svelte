<script>
	import Image from "./image.svelte";
</script>

<section>
  <div class="scr">
    <nav>
      <span>CALCO</span>
      <ul>
        <li>Home</li>
        <li>Projects</li>
        <li>Blog</li>
        <li>About</li>
      </ul>
    </nav>

    <main>
      <Image 
        class="img one" 
        first="https://images.unsplash.com/photo-1672508644320-c39f93a9904f?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=627&q=80" 
        second="https://images.unsplash.com/photo-1672411164027-f1c3fb8dbd5f?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=687&q=80" 
        left top
      />
      <Image 
        class="img two" 
        first="https://images.unsplash.com/photo-1672427045052-1dff001fb0b4?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1471&q=80" 
        second="https://images.unsplash.com/photo-1544038286-2b1fe3556062?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80" 
        top
      />
      <Image 
        class="img three"
        first="https://images.unsplash.com/photo-1672478865829-a0860d97919e?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=686&q=80" 
        second="https://images.unsplash.com/photo-1672420877594-99a77c4dff5c?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=687&q=80" 
        left
      />
      <Image 
        class="img four" 
        first="https://images.unsplash.com/photo-1672449413726-780b5b3d67e4?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80" 
        second="https://plus.unsplash.com/premium_photo-1669867674346-710d75215b51?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80" 
      />
    </main>

    <footer>
      <span>© 2020 CALCO</span>
    </footer>
  </div>
</section>

<style>
  :root {
    --img-width-small: 150px;
    --img-width-large: 250px;
    --img-height-small: 150px;
    --img-height-large: 250px;
  }

  main {
    height: 100%;
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vw;

    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, 1fr);
    grid-gap: 1rem;
  }

  /* left top */
  main :global(.img.one) {
    align-self: end;
    justify-self: end;

    width: var(--img-width-small);
    height: var(--img-height-large);
  }

  /* right top */
  main :global(.img.two) {
    align-self: end;
    justify-self: start;

    width: var(--img-width-large);
    height: var(--img-height-small);
  }

  /* left bottom */
  main :global(.img.three) {
    align-self: start;
    justify-self: end;

    width: var(--img-width-large);
    height: var(--img-height-small);
  }

  /* right bottom */
  main :global(.img.four) {
    align-self: start;
    justify-self: start;

    width: var(--img-width-small);
    height: var(--img-height-large);
  }

  .scr {
    height: 100vh;
    width: 100vw;
    background-color: #f0f0f0;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
  }

  nav {
    height: 10vh;
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vw;
    background-color: #fbfbfb;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  nav span {
    font-size: 2rem;
    font-weight: 600;
  }

  nav ul {
    display: flex;
    list-style: none;
  }

  nav li {
    margin-left: 2rem;
    font-size: 1.5rem;
    font-weight: 600;
  }

  footer {
    height: 10vh;
    width: 80vw;
    padding-left: 10vw;
    padding-right: 10vw;
    background-color: #d9d9d9;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  footer span {
    font-size: 1.5rem;
    font-weight: 600;
  }
</style>