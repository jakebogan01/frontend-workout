<script>
     import "../app.css";
     import { setContext } from 'svelte';
     import { writable } from 'svelte/store';
     import { browser } from '$app/environment';
     import Navbar from "../components/Navbar.svelte";

     // check if this is client side not server side
     if (browser) {
          // create user store
          const user = writable(JSON.parse(localStorage.getItem('user')));
          // set user store
          setContext('user', user);
     }
</script>

<div class="app">
     <Navbar />
     <div class="pages">
          <slot />
     </div>
</div>