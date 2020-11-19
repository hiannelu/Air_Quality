<template>
  <div class="card">
    <div :class="bgColor">
      <div class="card-header">{{ info.county }} - {{ info.site }}
        <a href="#" class="float-right" @click="favorites(info)">
          <FontAwesomeIcon v-if="toggle" icon="star" />
          <FontAwesomeIcon v-else :icon="['far', 'star']" />
          </a>
      </div>
      <div class="card-body">
        <ul class="list-unstyled">
          <li>AQI 指數: {{ info.aqi }}</li>
          <li>PM2.5: {{ info.pm }}</li>
          <li>說明: {{ info.status }}</li>
        </ul>
      {{ info.time }}
      </div>
    </div>
  </div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core'
import { faStar } from '@fortawesome/free-solid-svg-icons'
import { faStar as faStarRegular } from '@fortawesome/free-regular-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faStar, faStarRegular)

export default {
  props: [
    'info',
    'star'
  ],
  components: {
    FontAwesomeIcon
  },
  computed: {
    bgColor() {
      switch(this.info.status) {
        case '普通':
          return 'status-aqi2';
        case '對敏感族群不健康':
          return 'status-aqi3';
        case '對所有族群不健康':
          return 'status-aqi4';
        case '非常不健康':
          return 'status-aqi5';
        case '危害':
          return 'status-aqi6';
        default:
          return 'status-aqi1';
      }
    },
    toggle() {
      return this.star.includes(this.info)
    }
  },
  methods: {
    favorites(info) {
      let local = JSON.parse(localStorage.getItem('favorites')) || []
      if (local.indexOf(this.info.site) == -1) {
        this.toggle = true
      }
      this.$emit('favorites', info)
    }
  }
}
</script>

<style>
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
