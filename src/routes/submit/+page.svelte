<script lang="ts">
  import { PUBLIC_SUPABASE_ANON_KEY } from '$lib/types';
  
  let team = "";
  let model = "";
  let file: File | null = null;
  function onFile(e) { file = e.target.files?.[0] ?? null; }

  let loading = false;
  let message = "";

  // async function submitForm(event: Event) {
  //   event.preventDefault();
  //   loading = true;

  //   try {
  //     const payload = {
  //       team,
  //       model
  //     };

  //     const res = await fetch(
  //       'https://ctwfyjhvheylawtxrvdq.supabase.co/functions/v1/submit-leaderboard',
  //       {
  //         method: 'POST',
  //         headers: {
  //           Authorization: `Bearer ${PUBLIC_SUPABASE_ANON_KEY}`,
  //           'Content-Type': 'application/json'
  //         },
  //         body: JSON.stringify(payload)
  //       }
  //     );

  //     if (!res.ok) throw new Error(`HTTP error ${res.status}`);
  //     const json = await res.json();
  //     console.log('Submit response:', json);
  //     message = "Submission successful!";
  //   } catch (err) {
  //     console.error('Failed to submit:', err);
  //     message = "Submission failed. Try again.";
  //   } finally {
  //     loading = false;
  //   }
  // }
</script>

{#if message}
  <p class="feedback">{message}</p>
{/if}

<main class="subcontainer">
  <div class="leaderboard-preview card">
    <iframe 
      src="https://docs.google.com/forms/d/e/1FAIpQLScwTIR43liXvuN0oF0OB0muLLjBa8t04UwuEJHj1XN8fykmtw/viewform?embedded=true">Loading…
    </iframe>
  </div>
</main>