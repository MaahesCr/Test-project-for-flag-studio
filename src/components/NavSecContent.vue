<template>
    <div class="tab-content" id="pills-tabContent">
        <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
            <div v-for="cityOfRussia in Object.entries(pointsOfRussia).length">
                <p class="expand-the-city-area">
                    <p @click="changeColor(cityOfRussia)" class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+cityOfRussia" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+cityOfRussia">
                        <div :id="'MCE'+cityOfRussia">{{Object.keys(pointsOfRussia)[cityOfRussia-1]}}</div>
                        <i :id="'I'+cityOfRussia" class="fa fa-angle-right">‹</i>
                   </p> 

                    <div class="collapse multi-collapse" v-bind:id="'multiCollapseExample'+cityOfRussia"> 
                        <div @click="getIdOfPoint(Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].id)" v-for="pointOfCity in Object.values(pointsOfRussia)[cityOfRussia-1].length" class="card card-body card-of-office">
                            <h5 class="number-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentHeader}}</h5>
                            <h5 class="head-of-office">{{Object.values(pointsOfRussia)[cityOfRussia-1][pointOfCity-1].properties.balloonContentBody}}</h5>
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

                    <p @click="changeColor(cityOfBelarus+1000)" class="expand-the-city btn btn-primary" data-bs-toggle="collapse" v-bind:href="'#multiCollapseExample'+cityOfBelarus+1000" role="button" aria-expanded="false" v-bind:aria-controls="'multiCollapseExample'+cityOfBelarus+1000">
                        <div :id="'MCE'+(cityOfBelarus+1000)">{{Object.keys(pointsOfBelarus)[cityOfBelarus-1]}}</div>
                        <i :id="'I'+(cityOfBelarus+1000)" class="fa fa-angle-right">‹</i>  
                    </p> 
   
                    <div class="collapse multi-collapse" v-bind:id="'multiCollapseExample'+cityOfBelarus+1000"> 
                        <div @click="getIdOfPoint(Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].id)" v-for="pointOfCity in Object.values(pointsOfBelarus)[cityOfBelarus-1].length" class="card card-body card-of-office">
                            <h5 class="number-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].properties.balloonContentHeader}}</h5>
                            <h5 class="head-of-office">{{Object.values(pointsOfBelarus)[cityOfBelarus-1][pointOfCity-1].properties.balloonContentBody}}</h5>
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
                        this.$emit('getIdOfPoint', id)
                    },

                    changeColor(id) {
                        let curentHeader = $(`#MCE${id}`)[0];
                        this.toggleClass(curentHeader, 'orangeColor')

                        this.rotate(id)
                    },

                    rotate(id){
                        let curentI = $(`#I${id}`)[0];
                        this.toggleClass(curentI, 'rotate-90')
                    },

                    toggleClass(elem,className){
                        if (elem.className.indexOf(className) !== -1){
                        elem.className = elem.className.replace(className,'');
                        }
                        else{
                        elem.className = elem.className.replace(/\s+/g,' ') + 	' ' + className;
                        }
                    
                        return elem;
                    }

        },

        async mounted() {
            //await this.setCitesAndPointsOfRussia()
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
       
        color: rgb(30, 53, 93);
        border: none;
        font-weight: bold;
        font-size: 18px;
        transition: all 1s ease 0s;
        display:flex; 
        justify-content: space-between;
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

    .orangeColor{
        color: rgb(255, 158, 0);;
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

    .fa-angle-right{
        float: right;
        margin-right: .7em;
        transition: transform .3s;
        font-style: normal;
        font-weight: 900;
        color: rgb(30, 53, 93);
        transform: rotate(90deg);
    }

    .rotate-90{
        transform: rotate(270deg);
        color: rgb(255, 158, 0);
    }
</style>