<template>
            <div class="tab-content" id="pills-tabContent">
                <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
                    <div v-for="city in Object.keys(citesOfRussia).length">
                        <p class="expand-the-city-area">
                            <a class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+city" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+city">
                                {{Object.keys(citesOfRussia)[city-1]}}
                            </a>
                        </p>

                        <div class="collapse multi-collapse" v-bind:id="'multiCollapseExample'+city"> <!--id="multiCollapseExample1"-->
                            <div v-for="varOfCity in Object.values(citesOfRussia)[city-1]" class="card card-body card-of-office">
                                <h5 class="number-of-office">{{Object.values(pointsOfRussia)[varOfCity-1].properties.balloonContentHeader}}</h5>
                                <h5 class="head-of-office">{{Object.values(pointsOfRussia)[varOfCity-1].properties.balloonContentBody}}</h5>
                                <h5 class="contact-of-office">{{Object.values(pointsOfRussia)[varOfCity-1].properties.balloonContentFooter}}</h5>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="tab-pane fade" id="pills-profile" role="tabpanel" aria-labelledby="pills-profile-tab">

                </div>
            </div>
</template>

<script>
    export default {
        data () {
            return {
                iterationVariable: 0
            }
            
        },

        props: {  
          pointsOfRussia: {
                type: Array,
                required: true,
            },
          citesOfRussia: {
                type: Array,
                required: true,
            }
        },

                methods: {
                    iteration() {
                        this.iterationVariable += 1;
                    }
        },

        async mounted() {
            
        }, 
    }
</script>

<style scoped>
    .expand-the-city-area{
        margin: 20px 10px;
        border-bottom: 1px solid rgb(227, 227, 227);
    }

    .expand-the-city{
        background: none;
        color: orange;
        border: none;
        font-weight: bold;
        font-size: 20px;
    }

    .btn-primary:focus {
        box-shadow: none;
    }

    .card-of-office{
        background: none;
        border: none;
        padding: 20px;
    }

    .number-of-office{
        font-size: 18px;
        color: rgb(30, 53, 93);
    }

    .head-of-office{
        font-size: 16px;
        padding: 10px 0;
    }

    .contact-of-office{
    font-size: 16px;
    white-space: normal;
    }
</style>