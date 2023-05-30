<script>
  let jsonData = [];
  let tableVisible = false;
  let selectedCandidate = null;

  onMount(async () => {
    await fetchData();
    tableVisible = true;
  });

  async function fetchData() {
    const response = await fetch("https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E");
    const responseData = await response.json();
    jsonData = responseData.data;
  }

  function handleTitleClick(candidate) {
    selectedCandidate = candidate;
  }

  function handleFieldChange(event, field) {
    selectedCandidate[field] = event.target.value;
  }

  function handleSaveClick() {
    const index = jsonData.findIndex(candidate => candidate.id === selectedCandidate.id);

    if (index !== -1) {
      jsonData[index] = selectedCandidate;
    }

    selectedCandidate = null;
  }
</script>

<style>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-content {
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    max-width: 400px;
    text-align: center;
  }

  .popup-content h2 {
    margin-bottom: 10px;
  }

  .popup-content p {
    margin-bottom: 5px;
  }

  .popup-content input {
    width: 100%;
    padding: 5px;
    margin-bottom: 10px;
  }

  .popup-content button {
    margin-top: 20px;
  }
</style>

<main>
  {#if tableVisible}
    <table class="table">
      <thead class="thead-light">
        <tr>
          <th>First Name</th>
          <th>Surname</th>
          <th>Email</th>
          <th>Mobile</th>
        </tr>
      </thead>
      <tbody>
        {#each jsonData as candidate}
          <tr>
            <td>
              <a href="#" on:click|preventDefault={() => handleTitleClick(candidate)}>{candidate.firstName}</a>
            </td>
            <td>{candidate.surname}</td>
            <td>{candidate.email}</td>
            <td>{candidate.mobile}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  {/if}
</main>

{#if selectedCandidate}
  <div class="popup">
    <div class="popup-content">
      <h2>Edit Candidate</h2>
      <label for="firstName">First Name:</label>
      <input type="text" id="firstName" bind:value={selectedCandidate.firstName} on:input={(e) => handleFieldChange(e, 'firstName')} />
      <label for="surname">Surname:</label>
      <input type="text" id="surname" bind:value={selectedCandidate.surname} on:input={(e) => handleFieldChange(e, 'surname')} />
      <label for="email">Email:</label>
      <input type="text" id="email" bind:value={selectedCandidate.email} on:input={(e) => handleFieldChange(e, 'email')} />
      <label for="mobile">Mobile
