<template>
        <section class="nav-section">
            <nav-sec-nav-pills/>
            <nav-sec-content 
                :pointsOfRussua="pointsOfRussua"
            />
        </section>
</template>

<script>
import NavSecContent from './NavSecContent.vue'
import NavSecNavPills from './NavSecNavPills.vue'

    export default {
        components: { NavSecNavPills, NavSecContent },
        props: {  
          pointsOfRussua: {
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