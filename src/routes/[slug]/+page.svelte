<script>
     export let data;
     import { onMount, getContext } from 'svelte';
     import { goto } from '$app/navigation';

     // grab the user store
     const currentUser = getContext('user');

     onMount(async () => {
          if (!$currentUser) {
               goto('/signup');
          }
     });

     console.log(data?.slug)
</script>