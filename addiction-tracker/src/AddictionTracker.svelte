<script>
    import { onMount } from 'svelte';
    import { writable } from 'svelte/store';
  
    let lastUsageDate = writable('2023-10-01');
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
      lastUsageDate.subscribe(value => {
        daysSinceLastUsage.set(calculateDaysSinceLastUsage(value));
      });
    });
  </script>
  
  <style>
    .container {
      display: flex;
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    .tracker {
      flex: 3;
      margin-right: 20px;
    }
    .sidebar {
      flex: 1;
      background-color: #f4f4f4;
      padding: 10px;
      border-radius: 5px;
    }
    .tracker input {
      margin-top: 10px;
    }
    .sidebar h2 {
      margin-top: 0;
    }
    .sidebar ul {
      list-style-type: none;
      padding: 0;
    }
    .sidebar li {
      margin-bottom: 10px;
    }
    .sidebar a {
      text-decoration: none;
      color: #007BFF;
    }
    .sidebar a:hover {
      text-decoration: underline;
    }
  </style>
  
  <div class="container">
    <div class="tracker">
      <h1>Addiction Tracker</h1>
      {#if $daysSinceLastUsage < 7}
        <p>It's been less than a week since your last usage. Stay strong!</p>
      {:else if $daysSinceLastUsage < 30}
        <p>Great job! It's been {$daysSinceLastUsage} days since your last usage.</p>
      {:else}
        <p>Incredible! You've been clean for over a month!</p>
      {/if}
      <p>Days since last usage: {$daysSinceLastUsage}</p>
      <input type="date" bind:value={$lastUsageDate} on:change={updateLastUsage} />
    </div>
    <div class="sidebar">
      <h2>Resources</h2>
      <ul>
        <li><a href="https://www.samhsa.gov/find-help/national-helpline" target="_blank">SAMHSA National Helpline</a></li>
        <li><a href="https://www.aa.org/" target="_blank">Alcoholics Anonymous</a></li>
        <li><a href="https://www.na.org/" target="_blank">Narcotics Anonymous</a></li>
        <li><a href="https://www.smartrecovery.org/" target="_blank">SMART Recovery</a></li>
        <li><a href="https://www.recovery.org/" target="_blank">Recovery.org</a></li>
      </ul>
    </div>
  </div>