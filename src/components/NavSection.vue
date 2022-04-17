<template>
        <section class="nav-section">
            <nav-sec-nav-pills/>
            <nav-sec-content 
                :pointsOfRussia="pointsOfRussia"
                :citesOfRussia="citesOfRussia"
            />
        </section>
</template>

<script>
import NavSecContent from './NavSecContent.vue'
import NavSecNavPills from './NavSecNavPills.vue'

    export default {
        components: { NavSecNavPills, NavSecContent },
        props: {  
          pointsOfRussia: {
                type: Array,
                required: true
            },
            citesOfRussia: {
                type: Array,
                required: true
            }
        }
    }
</script>

<style>
    .nav-section{
        background: #FAFAFA;
        width: 25%;
        height: 100vh;
        border-right: 1px solid black;
    }
</style>