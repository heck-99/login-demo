<template lang="pug">
form(@submit.prevent="getData()")
    div(class="form-group row")
        label(for="location" class="col-sm-2") Enter a City:
        div(class="col-sm-6")
            input#location(type="text", v-model="location" class="form-control" required)
    div(class="form-group row")
        label(for="threshold" class="col-sm-2") Desired AQI Threshold:
        div(class="col-sm-6")
            select(v-model="threshold" class="form-control")
                option(v-for="[key, value] in thresholds" :value="key") {{ value }}
    button(type="submit" class="btn btn-primary") Search
    section#result
        h3 {{ message }}
        p(v-if="status === 'ok'") Today's AQI: 
            strong {{ aqi }}
</template>
<script lang="ts">
const BASE_URL = 'https://api.waqi.info/feed/{city}/?token=107e07356961086733ad81e14fb9e6a344aa7562';
const thresholdMap = new Map<number, string>([
    [50, 'Good'],
    [100, 'Moderate'],
]);

export default {
    name: "AirCheck",
    data() {
        return {
            location: '',
            thresholds: thresholdMap,
            threshold: 50,
            aqi: 0, // air quality indicator
            attributions: [],
            status: null,
        }
    },
    computed: {
        message(): string {
            if (this.status === 'ok') {
                return this.aqi < this.threshold ? 'You\'re in the clear!' : 'Air quality does not meet your threshold today!';
            } else if (this.status === 'error') {
                return 'Sorry, that is not a valid city!';
            }
            
            return '';
        }
    },
    methods: {
        async getData() {
            this.status = null;
            const url = BASE_URL.replace('{city}', this.location);
            fetch(url)
            .then(response => response.json())
            .then(json => {
                this.status = json.status;
                if (this.status === 'ok') {
                    // if more of the response were used, would create response class 
                    // would also do more robust response checking
                    this.aqi = json.data.aqi;
                } else {
                    console.log('error msg:' + json.data);
                }             
            })
            .catch((response) => {
                // would check status codes and do external logging to sentry in a real app
                console.log(response.json());
            })
        }
    }
};
</script>
<style scoped>
form {
    margin-top: 32px;
    margin-bottom: 32px;
    padding: 32px;
    background-color: ghostwhite;
}
</style>