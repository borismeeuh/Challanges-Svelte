<script>
  let city;
  let inputPlace;
  let weather = [];
  let data;

  // Function that gets input from the input field
  function inputHandler(e) {
    inputPlace = e.target.value;
    city =
      "https://weerlive.nl/api/json-data-10min.php?key=089d190d32&locatie=" +
      inputPlace;
    console.log(city);
  }

  //Function that fetches the data from the place given in the input
  async function fetchWeatherData() {
    const res = await fetch(city);
    data = await res.json();
    weather = await data.liveweer;
  }
</script>

<svelte:head>
  <link
    href="https://fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<body>
  <!-- Input a place that you want to check the weather -->

  <div id="inputAndResult">
    <h1>Check the weather for any place in the Netherlands</h1>
    <div id="placeInputDiv">
      <form on:submit|preventDefault>
        <input type="text" id="placeInputField" on:input={inputHandler} />
        <button type="submit" id="submitPlace" on:click={fetchWeatherData}
          >Check</button
        >
      </form>
    </div>

    <!-- Display the information about the given place - today -->
    <div id="weatherResults">
      <h1>
        {#each weather as place}
          Weather in {place.plaats}
        {/each}
      </h1>
      {#each weather as localWeather}
        <li><strong>Place:</strong> {localWeather.plaats}</li>
        <li><strong>Summary:</strong> {localWeather.samenv}</li>
        <li><strong>Temperature:</strong> {localWeather.temp}</li>
        <li><strong>Wind chill:</strong> {localWeather.gtemp}</li>
        <li><strong>Wind direction:</strong> {localWeather.windr}</li>
        <li><strong>Wind force:</strong> {localWeather.winds}</li>
        <li><strong>Sun rise:</strong> {localWeather.sup}</li>
        <li><strong>Sun set:</strong> {localWeather.sunder}</li>
        <li><strong>Maximum temperature:</strong> {localWeather.d0tmax}</li>
        <li><strong>Minimum temperature:</strong> {localWeather.d0tmin}</li>

        <!-- Display the information about the given place - tomorrow -->
        <h1>
          {#each weather as place}
            Predictions for tomorrow in {place.plaats}
          {/each}
        </h1>
        <li><strong>Maximum temperature:</strong> {localWeather.d1tmax}</li>
        <li><strong>Minimum temperature:</strong> {localWeather.d1tmin}</li>
        <li><strong>Wind force:</strong> {localWeather.windk}</li>
        <li><strong>Wind direction:</strong> {localWeather.windr}</li>
        <li>
          <strong>Chance at precipitation:</strong>
          {localWeather.d1neerslag}
        </li>
        <li><strong>Chance at sunshine:</strong> {localWeather.d1zon}</li>
      {:else}
        <!-- this block renders when weather.length === 0 -->
        <p>Enter a place in the Netherlands</p>
      {/each}
    </div>
  </div>
  <div id="footer">
    <p id="footerText">Boris Kamstra | HZ University of Applied Sciences</p>
  </div>
</body>

<style>
  :global(body) {
    background-image: url("/images/wallpaper.jpg");
    margin: 0;
  }

  #inputAndResult {
    color: white;
    position: absolute;
    left: 35%;
    top: 5%;
    width: 500px;
  }

  #placeInputField {
    position: relative;
    left: 15%;
    width: 300px;
    height: 40px;
    border-radius: 5px;
    background-color: aliceblue;
    opacity: 0.75;
    border: none;
  }

  #placeInputField:focus {
    outline: none;
  }

  #submitPlace {
    position: relative;
    left: 15%;
    width: 50px;
    height: 40px;
    border-radius: 5px;
    border: none;
    background-color: aliceblue;
  }

  #submitPlace:hover {
    background-color: aquamarine;
  }

  #weatherResults {
    text-align: center;
    list-style: none;
  }

  #footer {
    position: absolute;
    width: 100%;
    height: 35px;
    bottom: 0%;
    background-color: aliceblue;
  }

  #footerText {
    text-align: center;
    color: black;
    top: 10%;
  }
</style>
