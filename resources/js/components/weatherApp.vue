<template>
    
    <div class="content">

                <div class="autocomplete">
                    <div v-for="(place) in autocompletePlace" :key="place.id" @click="findFunc()">
                        {{ place }}
                    </div>
                </div>

                <div class="search-container">
                    <input v-model="searchplace" v-on:keyup="keymonitor" type="text" class="search" placeholder="Search City" aria-label="City" aria-describedby="basic-addon2">
                    <div class="search-button">
                        <button type="button" @click="searchFunc()">Find</button>
                    </div>
                </div>

                <div class="results">
                    <div class="results-header">
                        <div class="result-temp">{{ currentTemp.actual }}°C</div>
                        <div class="result-details">
                            <div class="result-weather">{{ currentTemp.summary }}</div>
                            <div class="result-place">{{ currentTemp.city }}</div>
                            <div class="result-date">{{ currentTemp.dt_txt }}</div>
                        </div>
                        <div class="result-icon">
                            <img :src="currentTemp.icon" alt="" srcset="">
                        </div>
                    </div>
                    <div class="results-week">
                        <table>
                            <tr v-for="(item) in tempList" :key="item.id">
                                <td>{{ item.dt_txt }}</td>
                                <td>
                                    <img :src="`http://openweathermap.org/img/w/${ item.weather[0].icon }.png`">
                                </td>
                                <td>{{ item.weather[0].description }}</td>
                                <td>{{ item.main.temp }}°C</td>
                            </tr>
                        </table>
                        
                    </div>
                </div>
               
    </div>

</template>

<script>
    export default {
        mounted() {
            this.fetchData()
        },
        data() {
            return {
                currentTemp: {
                    actual: '',
                    summary: '',
                    icon: 'http://openweathermap.org/img/w/10d.png',
                    city: '',
                    dt_txt: ''
                },
                tempList: [],
                city: 'tokyo',
                searchplace: '',
                autocompletePlace: [],
                clientKey: "SW5VQG3KT1SPSDXFAAE1ZP50B5LYZHDT34NVXBFGC5OD4MBO",
                secretKey: "FO2VF5KA2ZHE21SYCZHTQCJ1DYRFOJESITIQA0HVMCMWQV2N",
                v: "20180323",
            }
        },
        methods: {
            fetchData() {
                
                fetch(`api/weather?city=${this.city}`)
                .then(response => response.json())
                .then(data => {
                    console.log(data)
                    this.currentTemp.actual = data.list[0].main.temp;
                    this.currentTemp.summary = data.list[0].weather[0].description;
                    this.currentTemp.icon = 'http://openweathermap.org/img/w/' + data.list[0].weather[0].icon + '.png';
                    this.currentTemp.city = data.city.name;
                    this.currentTemp.dt_txt = data.list[0].dt_txt;
                    this.tempList = data.list;
                    
                });
            },
            keymonitor: function(event) {
                console.log(this.searchplace);
                //this.autocompletePlace = ['test1','test2','test3'];

                fetch(`https://api.foursquare.com/v2/venues/explore?client_id=${this.clientKey}&client_secret=${this.secretKey}&v=${this.v}&limit=1&ll=40.7243,-74.0018&query=${this.searchplace}`)
                .then(response => response.json())
                .then(data => {
                    console.log(data)
                });


                if(event.key == "Enter") {
                    console.log("enter key was pressed!");
                    this.searchFunc();
                }
            },
            searchFunc () {
                console.log(this.searchplace);
                this.autocompletePlace = [];
            },
            findFunc () {
                console.log('find');
                this.autocompletePlace = [];
            },

            
        }
    }
</script>
