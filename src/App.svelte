<script>
  import Router, { link } from 'svelte-spa-router'; 
  import MainSolver from './lib/Main.svelte';
  import VigenereEncoder from './lib/vigenere_encoder/VigenereEncoder.svelte';
  import SubstitutionEncoder from './lib/substution_encoder/SubstitutionEncoder.svelte';
  import PlayfairEncoder from './lib/playfair_encoder/PlayfairEncoder.svelte';
  import AffineEncoder from './lib/affine_encoder/affine.svelte';

  const routes = {
    '/': MainSolver,
    '/substitution_encoder': SubstitutionEncoder,
    '/vigenere_encoder': VigenereEncoder,
    '/playfair_encoder' : PlayfairEncoder,
    '/affine_encoder' : AffineEncoder
  };

  let openMenu = null;

  function toggleMenu(menuName, event) {
    event.stopPropagation();
    if (openMenu === menuName) {
      openMenu = null;
    } else {
      openMenu = menuName;
    }
  }

  function closeAll() {
    openMenu = null;
  }
</script>

<svelte:window on:click={closeAll} />

<nav>
  <div class="dropdown">
    <button 
      class="main-link" 
      class:active={openMenu === 'encoder'} 
      on:click={(e) => toggleMenu('encoder', e)}
    >
      Encoder
    </button>
    
    {#if openMenu === 'encoder'}
      <div class="dropdown-content">
        <a href="/vigenere_encoder" use:link>Vigenere Encoder</a>
        <a href="/substitution_encoder" use:link>Substution Encoder</a>
        <a href="/playfair_encoder" use:link>Playfair Encoder</a>
        <a href="/affine_encoder" use:link>Affine Encoder</a>
      </div>
    {/if}
  </div>

  <a 
    href="/" 
    use:link 
    class="main-link"
    on:click={closeAll}
  >
    Decoder
  </a>
</nav>

<hr />

<main>
  <Router {routes} />
</main>

<style>
  :root {
    --color-bg: rgba(15, 7, 7, 0.9);
    --color-fg: #B5ADAD;
    --color-active: #6E5F35;
    --color-hover: rgba(181, 173, 173, 0.2);
  }

nav {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000; 
    
    background-color: var(--color-bg);
    display: flex;
    justify-content: center;
    gap: 15px;
    padding: 10px;
    font-family: "GeistMono Nerd Font", sans-serif;
    
    
    backdrop-filter: blur(5px); 
  }
  main {
    
    padding: 20px;
    margin-top: 20px; 
  }
  .dropdown {
    position: relative;
  }

  .main-link {
    background: transparent;
    border: none;
    cursor: pointer;
    color: var(--color-fg);
    padding: 6px 15px;
    border-radius: 12px;
    font-size: 13px;
    text-decoration: none; 
    transition: all 0.3s ease;
    outline: none;
    display: inline-block;
  }

  .main-link:hover {
    background: var(--color-hover);
  }

  .main-link.active, .main-link:active {
    background: var(--color-active);
    color: #ffffff;
    border-radius: 10px;
  }

  .dropdown-content {
    position: absolute;
    background-color: var(--color-bg);
    min-width: 180px;
    border-radius: 10px;
    margin-top: 8px;
    z-index: 10;
    border: 1px solid var(--color-hover);
    box-shadow: 0px 8px 16px rgba(0,0,0,0.5);
    overflow: hidden;
  }

  .dropdown-content a {
    color: var(--color-fg);
    padding: 10px 15px;
    text-decoration: none;
    display: block;
    font-size: 12px;
  }

  .dropdown-content a:hover {
    color: white;
  }

  hr {
    border: none;
    border-top: 1px solid rgba(137, 180, 250, 0.1);
    margin: 0;
  }

  main {
    padding: 20px;
  }
</style>