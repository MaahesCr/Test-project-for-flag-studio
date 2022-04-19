<template>
    <div class="tab-content" id="pills-tabContent">
        <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
            <div v-for="cityOfRussia in Object.entries(this.citesAndPointsOfRussia).length">
                <p class="expand-the-city-area">
                    <a class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+cityOfRussia" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+cityOfRussia">
                        {{Object.keys(this.citesAndPointsOfRussia)[cityOfRussia-1]}}
                    </a> 
   
                    <div class="collapse multi-collapse" v-bind:id="'multiCollapseExample'+cityOfRussia"> 
                        <div v-for="pointOfCity in Object.values(this.citesAndPointsOfRussia)[cityOfRussia-1].length" class="card card-body card-of-office">
                            <h5 class="number-of-office">{{Object.values(this.citesAndPointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentHeader}}</h5>
                            <h5 class="head-of-office">{{Object.values(this.citesAndPointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentBody}}</h5>
                            <h5 class="contact-of-office">{{Object.values(this.citesAndPointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentFooter}}</h5>
                        </div> 
                    </div>
                </p>
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
                citesAndPointsOfRussia: {},
                boolVal: false
            }
            
        },

        props: {  
          pointsOfRussia: {
                type: Array,
                required: true,
            }
        },

                methods: {
                    async getPropsValue() {
                        return this.pointsOfRussia 
                    },

                    async setCitesAndPointsOfRussia() {
                        await this.getPropsValue().then((data) => {
                           if (Object.values(this.citesAndPointsOfRussia).length == 0) {
                                this.citesAndPointsOfRussia[Object.values(data)[0].city] = new Array(Object.values(data)[0])
                                for(let i = 1; i < Object.keys(data).length; i++){
                                    for (let j = 0; j < Object.keys(this.citesAndPointsOfRussia).length; j++) {
                                        if (Object.values(data)[i].city == Object.keys(this.citesAndPointsOfRussia)[j]) {  //Object.values(this.citesAndPointsOfRussia)[0][0].city
                                            Object.values(this.citesAndPointsOfRussia)[j].push(Object.values(data)[i]) 
                                            this.boolVal = true
                                        }
                                    }
                                    
                                    if (!this.boolVal)   {
                                        this.citesAndPointsOfRussia[Object.values(data)[i].city] = new Array(Object.values(data)[i])
                                    }
                                    this.boolVal = false
                                }
                            }
                        })
                    }
        },

        async mounted() {
            await this.setCitesAndPointsOfRussia()
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