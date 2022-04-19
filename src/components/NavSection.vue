<template>
        <section class="nav-section">
            <nav-sec-nav-pills
                @updateRu="updateCountryArea"
                @updateBl="updateCountryArea"
            />
            <nav-sec-content 
                :pointsOfRussia="pointsOfRussia"
                :pointsOfBelarus="pointsOfBelarus"
                ref="NavSectionContent"
            />
        </section>
</template>

<script>
import NavSecContent from './NavSecContent.vue'
import NavSecNavPills from './NavSecNavPills.vue'

    export default {
        components: { NavSecNavPills, NavSecContent },

        data () {
            return {
               
            }
        },

        props: {  
            pointsOfRussia: {
                type: Array,
                required: true
            },
            pointsOfBelarus: {
                type: Array,
                required: true
            }
        },
        methods: {
            updateCountryArea(data) {
                this.$emit('updateCountryArea', data)
                //this.$refs.NavSectionContent.setCitesAndPointsOfRussia()
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