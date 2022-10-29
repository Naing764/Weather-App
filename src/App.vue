<template>
  <div>
    <div class="col-8 offset-2 mt-5">
      <div class="row">
        <div class="col">
          <h1>Weather App</h1>
          <p>Seacrh with city name</p>
          <input
            v-on:keyup.enter="getWeatherData()"
            type="text"
            v-model="city"
            class="form-control"
          />
          <button
            class="btn btn-warning mt-2 w-100"
            type="button"
            @click="getWeatherData()"
          >
            Search
          </button>
          <div>
            <h3 v-if="error" class="text-danger">Something was wrong!</h3>
          </div>
        </div>
        <div class="col">
          <div
            class="card shadow"
            style="background-color: #e4eaed; border-radius: 35px"
          >
            <div v-if="showData" class="card-body">
              <div class="d-flex">
                <h3 class="flex-grow-1">{{ location.name }}</h3>
                <h6>{{ location.localtime }}</h6>
              </div>
              {{ currentLocation }}
              <div class="d-flex flex-column text-center mt-5 mb-4">
                <h6
                  class="display-4 mb-0 font-weight-bold"
                  style="color: #1c2331"
                >
                  {{ Math.round(Number(current.temp_c)) }}°C
                </h6>
                <span class="small">{{ current.condition.text }}</span>
              </div>

              <div class="d-flex align-items-center">
                <div class="flex-grow-1" style="font-size: 1rem">
                  <div>
                    <font-awesome-icon icon="fa-solid fa-wind" />
                    <span class="ms-1"> {{ current.wind_kph }} km/h </span>
                  </div>
                  <div>
                    <font-awesome-icon icon="fa-solid fa-tint" />
                    <span class="ms-1"> {{ current.humidity }}% </span>
                  </div>
                  <div>
                    <font-awesome-icon icon="fa-solid fa-sun" />
                    <span class="ms-1"> 0.2h </span>
                  </div>
                </div>
                <div>
                  <img
                    src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-weather/ilu1.webp"
                    width="100px"
                  />
                </div>
              </div>
            </div>
            <img
              v-else
              src="https://i.pinimg.com/564x/b2/f1/a4/b2f1a4aff39c6e3f999981b11acbd658.jpg"
              alt="no-data-image"
              class="img-thumbnail"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      city: "",
      current: "",
      location: "",
      error: false,
      showData: false,
    };
  },
  methods: {
    async getWeatherData() {
      const options = {
        method: "GET",
        headers: {
          "X-RapidAPI-Key":
            "7d28ed42bcmsh96c0e0262141df5p1aa65djsn5d90090f1243",
          "X-RapidAPI-Host": "weatherapi-com.p.rapidapi.com",
        },
      };
      const url = `https://weatherapi-com.p.rapidapi.com/current.json?q=${this.city}`;
      let res = await fetch(url, options);
      if (res.status == 200) {
        let data = await res.json();
        console.log(data);
        this.showData = true;
        this.error = false;
        this.current = data.current;
        this.location = data.location;
      } else {
        this.error = true;
      }
    },
  },
  computed: {
    currentLocation() {
      return `${this.location.region},${this.location.country}`;
    },
  },
};
</script>
