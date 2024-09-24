<script>
  import './Feature.css';
  export let totalFinishedBooks;
  export let totalReadingTime;

  let isPopupOpen = false;
  let goalType = 'Books'; // Main state
  let actualGoal = 12; // Main state
  let tempGoalType = 'Books'; 
  let tempActualGoal = 12; 
  let labelText = 'Select your goal type and its value!';
  let showError = false;

  $: progress = actualGoal ? parseFloat(Math.min((goalType === 'Books' ? totalFinishedBooks : totalReadingTime) / actualGoal * 100, 100).toFixed(1)) : 0;

  function handleGoalSubmit(event) {
    event.preventDefault();
    // Validate inputs
    if (tempGoalType === null || tempActualGoal === null) {
      showError = true;
      return; // Prevent submission
    }
    
    // Update main state upon successful validation
    goalType = tempGoalType;
    actualGoal = tempActualGoal;
    showError = false;

    console.log('Goal set:', { goalType, actualGoal });

    isPopupOpen = false; // Close the popup
  }
</script>

<div class="feature">
  <div class="item">
    <div class="goalHeader">
      <h4>Your Goal</h4>
      <div class="barDetails">
        {#if actualGoal}
          {goalType === 'Books' ? totalFinishedBooks : totalReadingTime} / {actualGoal} {goalType} 
        {/if}
      </div>
    </div>
    
    <div class="bar">
      <div class="progress" style="width: {progress}%">{progress}%</div>
    </div>
  </div>

  <div class="setGoal">
    <button class="newGoal button-51" on:click={() => isPopupOpen = true}>
      <h2>Want to set a new goal?</h2>
    </button>
  </div>
</div>

{#if isPopupOpen}
  <div class="popup">
    <div class="popup-content">
      <h2>Set Your Goal</h2>

      <form on:submit={handleGoalSubmit}>
        <fieldset>
          <legend>What do you want to track?</legend>
          <label>
            <input type="radio" name="goal" bind:group={tempGoalType} value='Books' required />
            Number of Books
          </label>
          <label>
            <input type="radio" name="goal" bind:group={tempGoalType} value='Hours' required />
            Reading Time (hours)
          </label>
        </fieldset>
      
        <fieldset>
          <legend>Enter your goal value!</legend>
          <input class="goalValue" type="number" bind:value={tempActualGoal} required />
        </fieldset>
        
        <label class="labelText {showError ? 'error' : ''}"> {labelText}</label>
      
        <button type="submit">Submit</button>
        <button type="button" on:click={() => isPopupOpen = false}>Close</button>
      </form>
      
    </div>
  </div>
{/if}

<style>

</style>
