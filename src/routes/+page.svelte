<script>
     import { onMount, getContext } from 'svelte';
     import { goto } from '$app/navigation';
     import Workouts from '../stores/workouts.js';
     import WorkoutDetails from '../components/WorkoutDetails.svelte';
     import Loader from '../components/Loader.svelte';
     import WorkoutFrom from '../components/WorkoutFrom.svelte';

     // grab the user store
     const currentUser = getContext('user');

     console.log('first load', $currentUser)
     
     onMount(async () => {
          if ($currentUser === null) {
               // goto('/signup');
               location.href = '/signup';
          }
          if ($currentUser) {
               // Get all the workouts from the server
               const response = await fetch('https://backend-workout.vercel.app/api/workouts', {
                    headers: {
                         'Authorization': `Bearer ${$currentUser.token}`,
                    },
               });
               // Handle the response
               const data = await response.json();
     
               if (response.ok) {
                    // Add the new workout to the store
                    Workouts.set([data]);
               }
          }
     });
</script>

<div class="home">
     <div class="workouts">
          {#if $Workouts.length < 1}
               <Loader />
          {:else}
               {#each $Workouts[0] as workout (workout._id)}
                    <WorkoutDetails {workout} />
               {:else}
                    <p>There are no workouts</p>
               {/each}
          {/if}
     </div>
     <WorkoutFrom />
</div>
