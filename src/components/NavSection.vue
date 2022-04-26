<template>
        <!--В этом компоненте вроде все неплохо, ничего менять бы не стал-->
        <section class="nav-section">
            <nav-sec-nav-pills
                @updateRu="updateCountryArea"
                @updateBl="updateCountryArea"
            />
            <nav-sec-content 
                :pointsOfRussia="pointsOfRussia"
                :pointsOfBelarus="pointsOfBelarus"
                @getIdOfPoint="getIdOfPoint"
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
            },

            getIdOfPoint(id){
                this.$emit('getIdOfPoint', id)
            }
        }
    }
</script>

<style>
    .nav-section{
        background: #FAFAFA;
        width: 25%;
        height: 100vh;
        box-shadow: 0 5px 0 0 #888;
    }
</style>