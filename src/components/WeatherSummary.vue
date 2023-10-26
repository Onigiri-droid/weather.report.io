<script setup>
import {firstLetter} from '../assets/utils'
const props = defineProps({
  wetherInfo: {
    type: [Object, null],
    required: true,
  }
})

const today = new Date().toLocaleString('ru-RU', {weekday: 'short', year: 'numeric', month: 'long', day: 'numeric'})
</script>

<template>
    <div class="summary">
      <div :style="`background-image: url('/weather-main/${wetherInfo?.weather[0].description}.png');`" class="pic-main"></div>
        <div class="weather">
            <div class="temp">{{Math.round(wetherInfo?.main?.temp)}} °C</div>
            <div class="weather-desc text-block">{{firstLetter(wetherInfo?.weather[0].description)}}</div>
        </div>
        <div class="city text-block">{{wetherInfo?.name}}, {{wetherInfo?.sys?.country}}</div>
        <div class="date text-block">{{today}}</div>
    </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main.sass'

.pic-main
  width: 60px
  height: 60px
  margin: 20px 0 12px
  background-repeat: no-repeat
  background-position: 50% 50%
  background-size: contain

.city
  font-size: 24px

.weather
  margin: 0 0 20px
  padding: 20px 0
  border-bottom: 1px solid rgba(255, 255, 255, 0.4)

.temp
  padding-bottom: 8px
  font-size: 32px

.text-block
  position: relative
  padding-left: 24px
  padding-bottom: 8px
  font-size: 14px

  &::before
    content: ''
    position: absolute
    top: 0
    left: 0
    width: 20px
    height: 20px
    margin-right: 6px
    background-repeat: no-repeat
    background-position: 50% 50%
    background-size: contain

.weather-desc

  &::before
    background-image: url('/src/assets/img/weather.svg')

.city

  &::before
    background-image: url('/src/assets/img/location.svg')

.date

  &::before
    left: 2px
    width: 15px
    height: 15px
    background-image: url('/src/assets/img/calendar.svg')
</style>