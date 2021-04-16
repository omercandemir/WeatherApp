<template>
<div>
  <p v-if="!isLoading">Veriable Loading...</p>
  <div class="text-white mb-8" v-if="isLoading">
      <div class="places-input text-gray-800">
          <input type="search" id="address-input" placeholder="Where are we going?" />
          <p>Selected: <strong id="address-value">none</strong></p>
      </div>
      <div class="weather-container font-sans md:w-128 max-w-lg rounded-lg overflow-hidden bg-gray-900 shadow-lg mt-8">
        <div class="current-weather flex items-center justify-between px-6 py-8">
            <div class="flex flex-col md:flex-row items-center">
                <div>
                  <div class="text-6xl font-semibold">{{currentVeriable.degree}}°C</div>
                  <div>Feels Like {{currentVeriable.felt}}°C</div>
                </div>
                <div class="mx-5">
                    <div class="font-semibold">{{currentVeriable.summary}}</div>
                    <div>{{currentVeriable.timezone}}</div>
                </div>
            </div>
            <div><img :src="currentVeriable.icon" :alt="currentVeriable.timezone" width="96px" height="96px"></div>
        </div>
      </div>
  </div>
</div>
</template>

<script>
export default {
    mounted(){
        this.fetchData()
    },
    data() {
        return {
            isLoading: false,
            currentVeriable: {
                timezone: '',
                degree: '',
                felt: '',
                summary: '',
                icon: 'http://openweathermap.org/img/wn/',

            },
            location: {
                name: 'Europe/Istanbul',
                lat: 41.0351,
                lon: 28.9833,
            },
        }
    },
    methods: {
        fetchData(){
            fetch(`api/weather?lat=${this.location.lat}&lon=${this.location.lon}`)
                .then(response => response.json())
                .then(data => {
                    console.log(data)
                    
                    this.currentVeriable.timezone = data.timezone;
                    
                    this.currentVeriable.degree = data.current.temp;
                    this.currentVeriable.summary = data.current.weather.description;
                    this.currentVeriable.felt = data.current.feels_like;
                    this.currentVeriable.icon = this.currentVeriable.icon + data.current.weather[0].icon + '.png';
                    this.isLoading = true;
                })
        }
    },
}
</script>

<style>

</style>