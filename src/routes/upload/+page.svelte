<script>
     import { onMount, getContext } from 'svelte';
     import { goto } from '$app/navigation';

     // grab the user store
     const currentUser = getContext('user');
     // let url = '';

     onMount(() => {
          if (!$currentUser) {
               goto('/login');
          }
     })

     const handleSubmit =  async (e) => {
          const response = await fetch('https://backend-workout.vercel.app/api/upload', {
               method: 'POST',
               body: new FormData(e.target),
          });
          const data = await response.json();
          console.log(data)
          // url = data.image;
     }
</script>

<h1>image upload</h1>

<form on:submit|preventDefault={handleSubmit} enctype="multipart/form-data">
     <input type="file" name='image'>
     <input type="submit">
</form>

<!-- <img src="https://backend-workout.vercel.app/api/images/{url}" alt=""> -->

<style>
     img {
          width: 400px;
          height: 400px;
          border: 2px solid blue;
     }
</style>
