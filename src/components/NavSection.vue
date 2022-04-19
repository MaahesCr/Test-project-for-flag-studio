<template>
        <section class="nav-section">
            <nav-sec-nav-pills
                @updateRu="met"
            />
            <nav-sec-content 
                :pointsOfRussia="pointsOfRussia"
                ref="NavSectionContent"
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
            }
        },
        methods: {
            met(data) {
                this.$refs.NavSectionContent.setCitesAndPointsOfRussia()
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