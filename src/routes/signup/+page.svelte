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

     let inputValues = {
          email: '',
          password: '',
     };
     let isLoading = null;
     let error = null;

     const handleSubmit = async (e) => {
          isLoading = true;
          error = null;

          // // Send the data to the server
          const response = await fetch('https://backend-workout.vercel.app/api/user/signup', {
               method: 'POST',
               headers: {
                    'Content-Type': 'application/json',
               },
               body: JSON.stringify(inputValues),
          });
          // Handle the response
          const data = await response.json();

          if (!response.ok) {
               isLoading = false;
               error = data.error;
          } else {
               // Add the new user to the store
               localStorage.setItem('user', JSON.stringify(data));

               // Updating user store
               currentUser.set(data);
               console.log($currentUser)
               console.log('New user added!', data);

               // Reset the form
               inputValues = {
                    email: '',
                    password: '',
               };

               goto('/');
          }
     };
</script>

<form on:submit|preventDefault={handleSubmit} class="signup">
     <h3>Sign up</h3>

     <label for="email">Email:</label>
     <input type="email" onChange={(e) => (inputValues.email = e.target.value)} name="email" id="email" bind:value={inputValues.email} />
     
     <label for="password">Password:</label>
     <input type="password" onChange={(e) => (inputValues.password = e.target.value)} name="password" id="password" bind:value={inputValues.password} />

     <button disabled={isLoading} type="submit">Sign up</button>
     {#if error}
          <div class="error">{error}</div>
     {/if}
</form>