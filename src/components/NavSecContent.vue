<template>
    <div class="tab-content" id="pills-tabContent">
        <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
            <div v-for="cityOfRussia in Object.entries(pointsOfRussia).length">
                <p class="expand-the-city-area">
                    <p @click="changeColor(cityOfRussia)" style="display:flex; justify-content: space-between;" class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+cityOfRussia" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+cityOfRussia">
                    <!--<a class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+cityOfRussia" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+cityOfRussia">-->
                        <div :id="'MCE'+cityOfRussia">{{Object.keys(pointsOfRussia)[cityOfRussia-1]}}</div>
                        <lottie-player src="https://assets5.lottiefiles.com/packages/lf20_j5o0zvf5.json" mode="bounce" background="transparent"  speed="1"  style="width: 30px; height: 30px;" hover  loop ></lottie-player>
                    </p> 

   
                    <div class="collapse multi-collapse" v-bind:id="'multiCollapseExample'+cityOfRussia"> 
                        <div @click="getIdOfPoint(Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].id)" v-for="pointOfCity in Object.values(pointsOfRussia)[cityOfRussia-1].length" class="card card-body card-of-office">
                            <h5 class="number-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentHeader}}</h5>
                            <h5 class="head-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentBody}}</h5>
                            <!--<h5 class="contact-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentFooter}}</h5>-->
                            <h5 class="contact-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].phoneNumbs}}</h5>
                            <h5 class="email-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].mail}}</h5>
                        </div> 
                    </div>
                </p>
                
            </div>
        </div>
        <div class="tab-pane fade" id="pills-profile" role="tabpanel" aria-labelledby="pills-profile-tab">
            <div v-for="cityOfBelarus in Object.entries(pointsOfBelarus).length">
                <p class="expand-the-city-area">

                    <p @click="changeColorOfB(cityOfBelarus*100)" style="display:flex; justify-content: space-between;" class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+cityOfBelarus*100" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+cityOfBelarus*100">
                        <div :id="'MCE'+cityOfBelarus*100">{{Object.keys(pointsOfBelarus)[cityOfBelarus-1]}}</div>
                        <lottie-player src="https://assets5.lottiefiles.com/packages/lf20_j5o0zvf5.json" mode="bounce" background="transparent"  speed="1"  style="width: 30px; height: 30px;" hover  loop ></lottie-player>
                    </p> 
   
                    <div class="collapse multi-collapse" v-bind:id="'multiCollapseExample'+cityOfBelarus*100"> 
                        <div @click="getIdOfPoint(Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].id)" v-for="pointOfCity in Object.values(pointsOfBelarus)[cityOfBelarus-1].length" class="card card-body card-of-office">
                            <h5 class="number-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].properties.balloonContentHeader}}</h5>
                            <h5 class="head-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].properties.balloonContentBody}}</h5>
                            <!--<h5 class="contact-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].properties.balloonContentFooter}}</h5>-->
                            <h5 class="contact-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].phoneNumbs}}</h5>
                            <h5 class="email-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].mail}}</h5>
                        </div> 
                    </div>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                citesAndPointsOfRussia: {},
                boolVal: false,
                color: true,
                colorBel: true
            }
            
        },

        props: {  
            pointsOfRussia: {
                type: Array,
                required: true,
            },
            pointsOfBelarus: {
                type: Array,
                required: true,
            }
        },

                methods: {

                    getIdOfPoint(id){
                        console.log(id)
                        this.$emit('getIdOfPoint', id)
                    },

                    async justFunc(){
                        
                        /*if (your_string.indexOf('hello') > -1)
                            {
                            alert("hello found inside your_string");
                            }*/
                    },

                    changeColor(id) {
                        if (this.color) {
                            document.getElementById('MCE'+id).style="color:#FF9e00;"
                        } else {
                            document.getElementById('MCE'+id).style="color:rgb(30, 53, 93);"
                        }
                        this.color = !this.color
                    },

                    changeColorOfB(id){
                        if (this.colorBel) {
                            document.getElementById('MCE'+id).style="color:#FF9e00;"
                        } else {
                            document.getElementById('MCE'+id).style="color:rgb(30, 53, 93);"
                        }
                        this.colorBel = !this.colorBel
                    }
                    


        },

        async mounted() {
            //await this.setCitesAndPointsOfRussia()
            await this.justFunc()
        }, 
    }
</script>

<style scoped>
    .expand-the-city-area{
        margin: 20px 10px;
        border-bottom: 1px solid rgb(227, 227, 227);
    }

    .tab-content{
    margin: 10px 0 0 0;
    border-top: 1px solid rgb(227, 227, 227);
    }

    .expand-the-city{
        background: none;
        color: #FF9e00;
        color: rgb(30, 53, 93);
        border: none;
        font-weight: bold;
        font-size: 18px;
    }

    .btn-primary:focus {
        box-shadow: none;
    }

    .card-of-office{
        cursor: pointer;
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
    .email-of-office{
        font-size: 16px;
        color: #32b3e9;
    }
</style>