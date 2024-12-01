<script>
  import { onMount } from 'svelte';

  const images = [
    "images/alex.jpg",
    "images/dmiitri.jpg",
    "images/ethan.jpg",
    "images/eugene.jpg",
    "images/james.jpg",
    "images/jie.jpg",
    "images/mishal.jpg",
    "images/nan.jpg",
    "images/shawn.jpg",
    "images/shivani.jpg",
    "images/shu.jpg",
    "images/srini.jpg",
    "images/srishti.jpg",
    "images/stefan.jpg",
    "images/sweekar.jpg",
    "images/tao.jpg",
    "images/vicky.jpg",
    "images/yiming.jpg",
  ];

  const employees = images.map((image) => {
    const name = image.replace("images/", "").replace(".jpg", "");
    const capitalizedName = name.charAt(0).toUpperCase() + name.slice(1);
    return { name: capitalizedName, photo: image };
  });

  let selectedEmployee = null;
  let flashingEmployee = null;
  let isSelecting = false;

  // Start the selection process
  const pickRandomEmployee = () => {
    if (isSelecting) return;

    isSelecting = true;
    flashingEmployee = null;
    selectedEmployee = null;

    let iterations = 0;
    const maxIterations = 30;

    // Flash names before stopping on the selected employee
    const interval = setInterval(() => {
      flashingEmployee = employees[Math.floor(Math.random() * employees.length)];
      iterations++;

      // Stop after the max iterations
      if (iterations >= maxIterations) {
        clearInterval(interval);
        selectedEmployee = flashingEmployee;
        flashingEmployee = null;
        isSelecting = false;
      }
    }, 100); // Adjust speed of flashing (100ms per step)
  };
</script>

<style>
  .selector-container {
    text-align: center;
    font-family: Arial, sans-serif;
  }

  .employee-photo {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    margin: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  .employee-name {
    font-size: 24px;
    font-weight: bold;
    margin: 5px;
  }

  .flash {
    animation: flash 0.3s ease-in-out infinite alternate;
  }

  @keyframes flash {
    0% { opacity: 1; }
    100% { opacity: 0.5; }
  }

  button {
    padding: 10px 20px;
    font-size: 18px;
    margin-top: 20px;
    cursor: pointer;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  button:hover {
    background-color: #0056b3;
  }
</style>

<div class="selector-container">
  <h1>Standup Selector</h1>

  <!-- Flashing employee -->
  {#if flashingEmployee}
    <div class="flash">
      <img
        src={flashingEmployee.photo}
        alt={flashingEmployee.name}
        class="employee-photo"
      />
      <p class="employee-name">{flashingEmployee.name}</p>
    </div>
  {/if}

  <!-- Final selected employee -->
  {#if selectedEmployee && !flashingEmployee}
    <div>
      <img
        src={selectedEmployee.photo}
        alt={selectedEmployee.name}
        class="employee-photo"
      />
      <p class="employee-name">{selectedEmployee.name}</p>
    </div>
  {/if}

  <!-- Selection button -->
  <button on:click={pickRandomEmployee} disabled={isSelecting}>
    {isSelecting ? 'Selecting...' : 'Pick Next'}
  </button>
</div>

