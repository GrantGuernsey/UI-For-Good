<script>
    import { writable } from "svelte/store";
  
    let goals = writable([]);
    let newGoal = "";
  
    function addGoal() {
      if (newGoal.trim()) {
        goals.update((existingGoals) => [...existingGoals, { text: newGoal, completed: false }]);
        newGoal = "";
      }
    }
  
    function toggleGoal(index) {
      goals.update((existingGoals) =>
        existingGoals.map((goal, i) =>
          i === index ? { ...goal, completed: !goal.completed } : goal
        )
      );
    }
  
    function removeGoal(index) {
      goals.update((existingGoals) => existingGoals.filter((_, i) => i !== index));
    }
  </script>
  
  <div class="goals-container">
    <h1>Your Goals</h1>
    <div class="goal-input">
      <input
        type="text"
        placeholder="Enter your goal..."
        bind:value={newGoal}
        on:keypress={(e) => e.key === "Enter" && addGoal()}
      />
      <button on:click={addGoal}>Add Goal</button>
    </div>
    <ul>
      {#each $goals as goal, index}
        <li class="{goal.completed ? 'completed' : ''}">
          <input
            type="checkbox"
            checked={goal.completed}
            on:change={() => toggleGoal(index)}
          />
          <span>{goal.text}</span>
          <button on:click={() => removeGoal(index)}>Remove</button>
        </li>
      {/each}
    </ul>
  </div>
  
  <style>
    .goals-container {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      margin: 20px;
      max-width: 500px;
    }
    .goal-input {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    .goal-input input {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    .goal-input button {
      padding: 10px 20px;
      background-color: #00695c;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
    }
    li.completed span {
      text-decoration: line-through;
      color: #757575;
    }
    li button {
      background-color: #d32f2f;
      color: white;
      border: none;
      border-radius: 5px;
      padding: 5px 10px;
      cursor: pointer;
    }
  </style>
  