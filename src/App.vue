<template>
  <div>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <div class="container">
      <h4>選取城市</h4>
      <select name="" id="" class="form-control mb-3" v-model="filter" style="width: 45rem">
        <option disabled selected>--- 請選擇城市 ---</option>
        <option v-for="(item, index) in location" :key="index" :value="item">{{ item }}</option>
      </select>
      <h4>關注城市</h4>
      <div class="row gap">
        <card v-for="(item, index) in star" 
          :info="item" 
          :key="index"
          :star="star"
          @favorites="favorites"></card>
      </div>
      <hr>
      <h4>所有區域</h4>
      <div class="row gap">
        <card v-for="(item, index) in filterLocation" 
          :info="item" 
          :key="index"
          :star="star"
          @favorites="favorites"></card>
      </div> 
    </div>
  </div>
</template>

<script>
import $ from 'jquery'
import card from './components/card.vue'

export default {
  name: 'App',
  components: {
    card
  },
  data() {
    return {
      data: [],
      location: [],
      star: JSON.parse(localStorage.getItem('favorites')) || [],
      filter: ''
    }
  },
  mounted() {
    const vm = this;
    const api = 'http://opendata2.epa.gov.tw/AQI.json';

    $.get(api).then(function( response ) {
      response.forEach(item => {
        let obj = {
          county: item.County,
          site: item.SiteName,
          aqi: item.AQI,
          pm: item['PM2.5'],
          status: item.Status,
          time: item.PublishTime
        }
        if (vm.location.indexOf(item.County) == -1) {
          vm.location.push(item.County)
        }
        vm.data.push(obj)   
      });
    });

  },
  computed: {
    filterLocation() {
      if (this.filter == '') return this.data;
      else {
        return this.data.filter(item => {
          return item.county == this.filter
        })
      }
    }
  },
  methods: {
    favorites(info) {
      let index = this.star.indexOf(info);
      if (index == -1) {
        this.star.push(info);
      } else {
        this.star.splice(index, 1);
      }
      localStorage.setItem('favorites', JSON.stringify(this.star))
    }
  }
};
</script>

<style>
.gap {
  grid-gap: 30px;
}
.status-aqi1 {
  background-color: white;
}
.status-aqi2 {
  background-color: #ffff00;
}
.status-aqi3 {
  background-color: #ff7e00;
}
.status-aqi4 {
  background-color: #ff0000;
}
.status-aqi5 {
  background-color: #8f3f97;
}
.status-aqi6 {
  background-color: #7e0023;
}
</style>