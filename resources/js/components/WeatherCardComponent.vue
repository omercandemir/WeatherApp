<template>
  <div class="text-white mb-8">
      <div class="places-input text-gray-800">
          <input type="text" class="w-full">
      </div>
      <div class="weather-container font-sans w-128 max-w-lg rounded-lg overflow-hidden bg-gray-900 shadow-lg mt-4">
        <div class="current-weather flex items-center justify-between px-6 py-8">
            <div class="flex items-center">
                <div>
                  <div class="text-6xl font-semibold">8°C</div>
                  <div>Feels Like 2°C</div>
                </div>
                <div class="mx-5">
                    <div class="font-semibold">Cloudy</div>
                    <div>TR, Istanbul</div>
                </div>
            </div>
            <div>icon</div>
        </div>
        <div class="future-weather text-sm bg-gray-800 px-6 py-6 overflow-hidden">
            <div class="flex items-center mt-8"> <!-- Day Item -->
                <div class="w-1/6 text-lg text-gray-200">MON</div>
                <div class="w-4/6 px-4 flex items-center">
                    <div>icon</div>
                    <div class="ml-3">Cloudy with a chance of showers</div>
                </div>
                <div class="w-1/6 text-right">
                    <div>5°C</div>
                    <div>-2°C</div>
                </div>
            </div> <!-- End Day Item -->
            <div class="flex items-center mt-8"> <!-- Day Item -->
                <div class="w-1/6 text-lg text-gray-200">TUE</div>
                <div class="w-4/6 px-4 flex items-center">
                    <div>icon</div>
                    <div class="ml-3">Cloudy with a chance of showers</div>
                </div>
                <div class="w-1/6 text-right">
                    <div>5°C</div>
                    <div>-2°C</div>
                </div>
            </div> <!-- End Day Item -->
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
            location: {
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
                })
        }
    },
}
</script>

<style>

</style>