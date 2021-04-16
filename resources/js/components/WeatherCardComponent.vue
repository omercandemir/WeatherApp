<template>
<div>
  <p v-if="!isLoading">Veriable Loading...</p>
  <div class="text-white mb-8" v-if="isLoading">
      <div class="places-input text-gray-800" id="search">
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
            <div><img :src="currentVeriable.icon" :alt="currentVeriable.timezone"></div>
        </div>
        <div class="future-weather text-sm bg-gray-800 px-6 py-8 overflow-hidden">
        <div v-for="(day, index) in daily" :key="day.time" class="flex items-center" :class="{ 'mt-8' : index > 0 }">
          <div class="w-1/6 text-lg text-gray-200">{{weekDays(day.dt)}}</div>
          <div class="w-4/6 px-4 flex items-center">
            <div><img :src="icon + day.weather[0].icon + '.png'" alt=""></div>
            <div class="ml-3">{{day.weather[0].description}}</div>
          </div>
          <div class="w-1/6 text-right">
            <div>{{Math.round(day.temp.max - 273.15)}} °C</div>
            <div>{{Math.round(day.temp.min - 273.15)}} °C</div>
          </div>
        </div>
      </div> <!-- end future-weather -->
      </div>
  </div>
</div>
</template>

<script>
import places from 'places.js';
export default {
    props: {
        type: {
        type: String,
        required: false,
        },
   },
    mounted(){
        this.fetchData()
        setTimeout(() => {
            this.input = document.createElement('input');
            document.querySelector('#search').appendChild(this.input);
            var placesAutocomplete = places({
                container: this.input,
            }).configure({
                type: 'city',
                aroundLatLngViaIP: false,
            });
                var $address = document.querySelector('#address-value')
                placesAutocomplete.on('change', (e) => {
                    $address.textContent = e.suggestion.value
                    this.location.name = `${e.suggestion.name}, ${e.suggestion.country}`
                    this.location.lat = e.suggestion.latlng.lat
                    this.location.lng = e.suggestion.latlng.lng
                });
                placesAutocomplete.on('clear', function () {
                    $address.textContent = 'none';
                });
        }, 1000)
        
    },
    data() {
        return {
            instance: null,
            isLoading: false,
            icon: 'http://openweathermap.org/img/wn/',
            currentVeriable: {
                timezone: '',
                degree: '',
                felt: '',
                summary: '',
                icon: 'http://openweathermap.org/img/wn/',

            },
            daily: [],
            location: {
                name: 'Europe/Istanbul',
                lat: 41.0351,
                lon: 28.9833,
            },
        }
    },
    beforeDestroy() {
        // if you had any "this.instance.on", also call "off" here
        this.instance.destroy();
    },
    methods: {
        fetchData(){
            fetch(`api/weather?lat=${this.location.lat}&lon=${this.location.lon}`)
                .then(response => response.json())
                .then(data => {
                    console.log(data)
                    
                    this.currentVeriable.timezone = data.timezone;
                    
                    this.currentVeriable.degree = Math.round(data.current.temp - 273.15);
                    this.currentVeriable.summary = data.current.weather.description;
                    this.currentVeriable.felt = Math.round(data.current.feels_like - 273.15);
                    this.currentVeriable.icon = this.currentVeriable.icon + data.current.weather[0].icon + '.png';

                    this.daily = data.daily
                    this.isLoading = true;
                })
        },
        weekDays(timestamp){
            const newDate = new Date(timestamp*1000)
            const days = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT']
            return days[newDate.getDay()]
        }
    }
}
</script>

<style>

</style>