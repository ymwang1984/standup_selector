<script>
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

  let employees = images.map((image) => {
    const name = image.replace("images/", "").replace(".jpg", "");
    const capitalizedName = name.charAt(0).toUpperCase() + name.slice(1);
    return { name: capitalizedName, photo: image };
  });

  let selectedEmployee = null;
  let flashingEmployee = null;
  let isSelecting = false;
  let selectedEmployees = [];

  const removeEmployee = (employee) => {
    employees = employees.filter((e) => e.name !== employee.name);
  };

  const pickRandomEmployee = () => {
    if (isSelecting || employees.length === 0) return;

    isSelecting = true;
    flashingEmployee = null;
    selectedEmployee = null;

    let iterations = 0;
    const maxIterations = 30;

    const interval = setInterval(() => {
      flashingEmployee = employees[Math.floor(Math.random() * employees.length)];
      iterations++;

      if (iterations >= maxIterations) {
        clearInterval(interval);
        selectedEmployee = flashingEmployee;
        removeEmployee(selectedEmployee);
        selectedEmployees = [...selectedEmployees, selectedEmployee];
        flashingEmployee = null;
        isSelecting = false;
      }
    }, 100);
  };
</script>

<style>
  .selector-container {
    text-align: center;
    font-family: Arial, sans-serif;
  }

  .employee-photo {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    margin: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    object-fit: cover;
    object-position: center;
  }

    .employee-photo-picked {
    width: 70px;
    height: 70px;
    border-radius: 50%;
    margin: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    object-fit: cover;
    object-position: center;
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

  .selected-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    padding: 20px;
    margin-top: 20px;
  }

  .selected-item {
    text-align: center;
    flex: 0 1 150px; /* Control item width */
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

  <button on:click={pickRandomEmployee} disabled={isSelecting}>
    {isSelecting ? 'Selecting...' : employees.length === 0 ? 'Winner!!' : 'Pick Next'}
  </button>

  <!-- Display selected employees -->
  <div>
    <h2>Teammates Picked</h2>
    <div class="selected-list">
      {#each selectedEmployees as employee}
        <div class="selected-item">
          <img
            src={employee.photo}
            alt={employee.name}
            class="employee-photo-picked"
          />
          <p class="employee-name">{employee.name}</p>
        </div>
      {/each}
    </div>
  </div>
</div>
