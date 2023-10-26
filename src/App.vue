<script setup>
import { ref, onMounted, computed } from 'vue'
import { API_KEY, BASE_URL } from './assets/constants'
import {firstLetter} from './assets/utils'
import WeatherSummary from './components/WeatherSummary.vue'
import Highlights from './components/Highlights.vue'
import Coords from './components/Coords.vue'
import Humidity from './components/Humidity.vue'

const city = ref('Екатеринбург')
const wetherInfo = ref(null)
const isError = computed(() => wetherInfo.value?.cod !== 200)

function getWether() {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => {
      // if (!response.ok) {
      //   throw new Error('Ошибка сети: ' + response.status)
      // }
      return response.json()
    })
    .then((data) => wetherInfo.value = data)
    .catch((error) => console.error('Произошла ошибка:', error))
}

onMounted(getWether)
</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="['section', 'section-left', { 'section-error': isError }]">
              <div class="info">
                <div class="city-inner">
                  <input v-model="city" type="text" class="search"
                    @keyup.enter="getWether">
                </div>
                <WeatherSummary v-if="!isError" :wetherInfo="wetherInfo" />
                <div v-else class="error">
                  <div class="error-title">Ой, что-то пошло не так!</div>
                  <div class="error-message">{{firstLetter(wetherInfo?.message)}}</div>
                </div>
              </div>
            </section>
            <section v-if="!isError" class="section section-right">
              <Highlights :wetherInfo="wetherInfo" />
            </section>
          </div>
          <div v-if="!isError" class="sections">
            <Coords :coord="wetherInfo.coord" />
            <Humidity :humidity="wetherInfo.main.humidity" />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="sass" scoped>
@import './assets/styles/main'
.page
  position: relative
  display: flex
  justify-content: center
  align-items: center
  min-height: 100vh
  padding: 20px 0
  background-color: #59585d

.laptop
  width: 100%
  padding: 20px
  background-color: #0e100f
  border-radius: 25px

.sections
  display: flex
  width: 100%

  @media (max-width: 767px)
    flex-direction: column

.section-left
  width: 30%
  padding-right: 10px

  @media (max-width: 767px)
    width: 100%
    padding-right: 0

.section-right
  width: 100%
  padding-left: 10px

  @media (max-width: 767px)
    width: 100%
    margin-top: 16px
    padding-left: 0
.section-error
  min-width: 235px
  width: auto
  padding-right: 0
.city-inner
  position: relative
  display: inline-block
  width: 100%

  &::after
    content: ''
    position: absolute
    top: 0
    right: 10px
    width: 25px
    height: 25px
    background: url('./assets/img/search.svg') no-repeat 50% 50%
    background-size: contain
    transform: translateY(50%)
    cursor: pointer

.info
  height: 100%
  padding: 16px
  background: url('./assets/img/gradient-1.jpg') no-repeat 50% 50%
  background-size: cover
  border-radius: 25px

.search
  width: 100%
  padding: 16px
  font-family: 'Inter', Arial, sans-serif
  color: $white
  background-color: rgba(0, 0, 0, 0.75)
  border-radius: 16px
  border: none
  outline: none
  cursor: pointer
.section-bottom
  width: 50%
  margin-top: 16px
  
  &:nth-child(2)
    margin-left: 10px

  @media (max-width: 767px)
    width: 100%

    &:nth-child(2)
      margin-left: 0

.error
  padding-top: 20px

  &-title
    font-size: 18px
    font-weight: 700

  &-message
    font-size: 16px
    font-weight: 500
</style>
