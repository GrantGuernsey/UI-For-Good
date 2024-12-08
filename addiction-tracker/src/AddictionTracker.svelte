
<script>
  import { onMount } from "svelte";
  import { writable } from "svelte/store";
  import Resource from "./Resource.svelte";
  import Goals from "./Goals.svelte";

  let lastUsageDate = writable("2024-12-08");
  let daysSinceLastUsage = writable(0);
  $: daysSinceColor = 
    $daysSinceLastUsage < 7 
      ? "#d32f2f" // Red for less than a week
      : $daysSinceLastUsage < 30 
      ? "#fbc02d" // Yellow for less than a month
      : "#00695c"; // Green for over a month

  function calculateDaysSinceLastUsage(date) {
    const lastUsage = new Date(date);
    const today = new Date();
    const diffTime = Math.abs(today - lastUsage);
    return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
  }

  function updateLastUsage(event) {
    lastUsageDate.set(event.target.value);
  }

  onMount(() => {
    lastUsageDate.subscribe((value) => {
      daysSinceLastUsage.set(calculateDaysSinceLastUsage(value));
    });
  });
</script>

<div class="container">
  <div class="tracker">
    <h1>Addiction Tracker</h1>
    {#if $daysSinceLastUsage < 7}
      <p class = "update-message">It's been less than a week since your last usage. Stay strong!</p>
    {:else if $daysSinceLastUsage < 30}
      <p class = "update-message">
        Great job! It's been {$daysSinceLastUsage} days since your last usage.
      </p>
    {:else}
      <p class = "update-message"> Incredible! You've been clean for over a month!</p>
    {/if}
    <p class="days-since" style="color: {daysSinceColor};">Days since last usage: {$daysSinceLastUsage}</p>
    <input
      type="date"
      bind:value={$lastUsageDate}
      on:change={updateLastUsage}
    />
  </div>
  <div class="goals-container">
    <Goals/>
  </div>
  <div class="sidebar">
    <h2>Resources</h2>
    <Resource
      title="SAMHSA National Helpline"
      url="https://www.samhsa.gov/"
      blurb="Free, confidential, 24/7, 365-day-a-year treatment referral and information service."
    />
    <Resource
      title="Alcoholics Anonymous"
      url="https://www.aa.org/"
      blurb="A.A. is more than a set of principles; it is a society of alcoholics in action. We must carry the message, else we ourselves can wither and those who haven't been given the truth may die."
    />
    <Resource
      title="Narcotics Anonymous"
      url="https://www.na.org/"
      blurb="The message is that an addict, any addict, can stop using drugs, lose the desire to use, and find a new way to live. Our message is hope and the promise is freedom."
    />
    <Resource
      title="SMART Recovery"
      url="https://www.smartrecovery.org/"
      blurb="SMART Recovery is a global community of people and families working together to resolve addictive problems. In our free group discussion meetings, participants learn from one another using a self-empowering approach based on the most current science of recovery."
    />
    <Resource
      title="Recovery.org"
      url="https://www.recovery.org/"
      blurb="Recovery.org is a private and confidential resource for individuals and families seeking addiction treatment information."
    />
  </div>
  <div class="emergency">
    <h2>Emergency Contacts</h2>
    <p>If you feel like you might relapse or are in crisis, call for help:</p>
    <button onclick="window.location.href='tel:+18006624357'">Call SAMHSA Helpline</button>
  </div>

</div>

<style>
  .container {
    display: flex;
    font-family: 'Helvetica Neue', Arial, sans-serif;
    justify-content: center;
    margin: 20px;
    background-color: #f5f5f5; /* Light grey background */
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  .tracker {
    flex: 2;
    margin-right: 20px;
    background-color: #ffffff; /* White background */
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    font-size: 1.2em;
  }
  .sidebar, .emergency, .goals-container {
    flex: 1;
    background-color: #ffffff; /* White background */
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  .sidebar{
    background-color: #f9f9f9; /* Light grey background */
  }
  .emergency {
    background-color: #da9292; /* Light grey background */
    max-width: 300px;
  }
  .goals-container{
    background-color: #a6b5b8; /* Light grey background */
    max-width: 400px;
  }
  h2 {
    color: #333333; /* Dark grey text */
  }
  p {
    color: #666666; /* Medium grey text */
  }
  button {
    background-color: #007bff; /* Blue background */
    color: #ffffff; /* White text */
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  button:hover {
    background-color: #0056b3; /* Darker blue on hover */
  }
  a {
    color: #007bff; /* Blue text */
    text-decoration: none;
  }
  a:hover {
    text-decoration: underline;
  }
  .update-message {
    font-weight: bold;
    font-size: 2.0em;
    margin-bottom: 20px;
  }
  .days-since {
    font-size: 1.5em;
    margin-bottom: 20px;
  }
</style>
