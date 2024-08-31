<template>
  <div id="app" class="container">
    <h1>ISS Tracker</h1>
    <!-- Display each position with alternating colors -->
    <ul class="position-list">
      <li v-for="(position, index) in positions" :key="index"
        :class="{ 'even': index % 2 === 0, 'odd': index % 2 !== 0 }">
        <span class="icon">🌍</span> Latitude: {{ position.latitude }}, Longitude: {{ position.longitude }}
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import axios from "axios";

export default {
  setup() {
    const positions = ref([]); // Reactive reference to hold ISS positions

    const fetchISSPosition = () => {
      
      axios
        .get("http://api.open-notify.org/iss-now.json")
        .then((response) => {
          positions.value.push(response.data.iss_position); // Push new position to the array
        })
        .catch((error) => {
          console.error("Error fetching ISS position:", error);
        });
    };

    onMounted(() => {
      fetchISSPosition(); // Fetch initial data
      setInterval(fetchISSPosition, 1000); // Fetch data every second
    });

    return {
      positions,
    };
  },
};
</script>

<style>
body {
  font-family: 'Roboto', sans-serif;
  text-align: center;
  background-color: #282c34;
  color: #ffffff;
  margin: 0;
  padding: 20px;
}

h1 {
  margin-bottom: 20px;
  color: #61dafb;
}

.position-list {
  list-style-type: none;
  padding: 0;
  margin: 0 auto;
  max-width: 500px;
  border-radius: 8px;
  overflow: hidden;
}

li {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 10px 15px;
  border-bottom: 1px solid #444;
  font-size: 1.2em;
  transition: background-color 0.3s;
}

.even {
  background-color: #3a3f47;
}

.odd {
  background-color: #2e333b;
}

li:hover {
  background-color: #4a90e2;
  color: #ffffff;
}

.icon {
  margin-right: 10px;
  font-size: 1.5em;
}
</style>
