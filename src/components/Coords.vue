<script setup>
const props = defineProps({
  coord: {
    type: Object,
    required: true,
  }
})
</script>

<template>
    <section class="section-bottom">
        <div class="block-bottom">
            <div class="block-bottom-inner">
                <div class="block-bottom-pic pic-coords"></div>
                <div class="block-bottom-texts">
                    <div class="block-bottom-text-block">
                        <div class="block-bottom-text-block-title">Долгота: {{coord?.lon}}</div>
                        <div class="block-bottom-text-block-desc">Долгота измеряет расстояние к востоку или западу от нулевого меридиана.</div>
                    </div>
                    <div class="block-bottom-text-block">
                        <div class="block-bottom-text-block-title">Широта: {{coord?.lat}}</div>
                        <div class="block-bottom-text-block-desc">Широтные линии начинаются на экваторе (0 градусов широты) и проходят на восток и запад параллельно экватору.</div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style lang="sass" scoped>
</style>