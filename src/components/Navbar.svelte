<script>
     import { onMount, getContext } from 'svelte';
     import { goto } from '$app/navigation';
     // grab the user store
     const currentUser = getContext('user');

     onMount(() => {
          if ($currentUser) {
               goto('/');
          }
     })

     const handleLogout = () => {
          // Update user in local storage
          localStorage.setItem('user', null);
          currentUser.set(null);

          // Redirect to the login page
          goto('/login');
     };
</script>

<header>
     <div class="container">
          <a href="/">Workout Buddy</a>
          <nav>
               {#if $currentUser}
                    <div>
                         <span>{$currentUser.email}</span>
                         <button on:click={handleLogout} type="button">Log out</button>
                    </div>
               {:else if $currentUser === null}
                    <div>
                         <a href="/login">Login</a>
                         <a href="/signup">Signup</a>
                    </div>
               {/if}
          </nav>
     </div>
</header>