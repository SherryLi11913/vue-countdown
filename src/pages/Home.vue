<template>
    <CounterComponent
        :year="2022"
        :month="8"
        :day="6"
        :hour="11"
        :minute="37"
    />
</template>

<script>
import CounterComponent from '../components/Counter.vue';

export default {
    name: 'HomePage',
    components: {
        CounterComponent,
    },
}
</script>

<style>
</style>