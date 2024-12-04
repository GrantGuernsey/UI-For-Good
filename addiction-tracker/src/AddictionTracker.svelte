<script>
  import { onMount } from "svelte";
  import { writable } from "svelte/store";
  import Resource from "./Resource.svelte";

  let lastUsageDate = writable("2023-10-01");
  let daysSinceLastUsage = writable(0);

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
      <p>It's been less than a week since your last usage. Stay strong!</p>
    {:else if $daysSinceLastUsage < 30}
      <p>
        Great job! It's been {$daysSinceLastUsage} days since your last usage.
      </p>
    {:else}
      <p>Incredible! You've been clean for over a month!</p>
    {/if}
    <p class="days-since">Days since last usage: {$daysSinceLastUsage}</p>
    <input
      type="date"
      bind:value={$lastUsageDate}
      on:change={updateLastUsage}
    />
  </div>
  <div class="sidebar">
    <h2>Resources</h2>
    <Resource
      title="SAMHSA National Helpline"
      url="https://www.samhsa.gov/find-help/national-helpline"
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
</div>

<style>
  .container {
    display: flex;
    font-family: 'Helvetica Neue', Arial, sans-serif;
    margin: 20px;
  }
  .tracker {
    flex: 2;
    margin-right: 20px;
  }
  .sidebar {
    flex: 3;
    background-color: #e0f7fa;
    padding: 30px;
    border-radius: 10px;
  }
  .tracker input {
    margin-top: 10px;
  }
  .sidebar h2 {
    margin-top: 0;
    color: #00695c;
  }
  .days-since {
    font-size: 2em;
    font-weight: bold;
    color: #d32f2f;
  }
</style>
