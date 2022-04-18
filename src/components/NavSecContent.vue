<template>
            <div class="tab-content" id="pills-tabContent">
                <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
                    <div v-for="city in Object.keys(citesOfRussia).length">
                        <p class="expand-the-city-area">
                            <a @click="notiter" class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+city" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+city">
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
                citesAndPointsOfRussia: {},
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
                    async getPropsValue() {
                        return this.pointsOfRussia 
                    },

                    async notiter() {
                        await this.getPropsValue().then((data) => {
                            this.citesAndPointsOfRussia[Object.values(data)[0].city] = new Array(Object.values(data)[0])
                            let boolVal = false
                            for(let i = 1; i < Object.keys(data).length; i++){
                                for (let j = 0; j < Object.keys(this.citesAndPointsOfRussia).length; j++) {
                                    console.log(Object.values(this.citesAndPointsOfRussia)[0][j].city)
                                    if (Object.values(data)[i].city == Object.values(this.citesAndPointsOfRussia)[0][0].city) {  
                                        console.log('ht', Object.values(this.citesAndPointsOfRussia)[0].push(Object.values(data)[i])) // ghjrc. 
                                        Object.values(this.citesAndPointsOfRussia)[0].push(Object.values(data)[i])   
                                        boolVal = true
                                        console.log(boolVal)
                                    }
                                }
                                if (boolVal)   {
                                    //создать новую с прокси
                                    console.log('-')
                                    console.log(Object.values(Object.values(this.citesAndPointsOfRussia)[0]))
                                    //Object.values(this.citesAndPointsOfRussia)[Object.values(data)[i].city] = new Array(Object.values(data)[i])
                                    this.citesAndPointsOfRussia[Object.values(data)[i].city] = new Array(Object.values(data)[i])
                                    console.log('bef',this.citesAndPointsOfRussia)
                                }
                            }
                            console.log(this.citesAndPointsOfRussia)
                            })
                    }
        },

        async mounted() {
            await this.notiter()
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