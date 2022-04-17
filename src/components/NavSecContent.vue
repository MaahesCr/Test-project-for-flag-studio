<template>
            <div class="tab-content" id="pills-tabContent">
                <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
                    <div v-for="cites in Object.keys(citesOfRussia).length">
                        <h1>{{Object.keys(citesOfRussia)[cites-1]}}</h1>
                        <div v-for="point in citesOfRussia[Object.keys(citesOfRussia)[cites-1]]">
                            <h1>ujdyj</h1>
                            
                        </div>
                    <!--
                        <p>
                            <a class="btn btn-primary" data-bs-toggle="collapse" href="#multiCollapseExample1" role="button" aria-expanded="false" aria-controls="multiCollapseExample1">Переключить первый элемент</a>
                        </p>
                        <div class="collapse multi-collapse" id="multiCollapseExample1">
                            <div class="card card-body">
                                Некоторый заполнитель для первого компонента сворачивания в этом примере множественного сворачивания. Эта панель по умолчанию скрыта, но открывается, когда пользователь активирует соответствующий триггер.
                            </div>
                        </div>
                        Раскрывающийся список по городу с количеством городов
                        {{pointsOfRussia.length}}
                        {{citesOfRussia}}-->
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
                redactoredValues: [],
                uniqueCities: [['kek', 2],['lol', 1]]
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
                    async countNumberOfCities() {
                        console.log('here: ', this.pointsOfRussia)
                        //console.log('ooo', Object.keys(this.citesOfRussia).length)
                        console.log('ooo', this.citesOfRussia)
                        //console.log('sss', pointsOfRussia)
                        //this.pointsOfRussia.then((data) => this.redactoredValues = data)
                        /*
                        console.log('redactoredValues: ', this.redactoredValues)
                        console.log('L: ', this.redactoredValues.length)
                        for (let i = 0; i < this.redactoredValues.length; i++){      
                            if (this.redactoredValues[0].city == this.uniqueCities[i][0]) {
                                this.uniqueCities[i][1] +=1
                            }   else {
                                this.uniqueCities.push([this.redactoredValues[0].city ,0])
                            }   
                        } 
                        console.log('sd', this.uniqueCities)
                        */
                        //this.uniqueCities.push(this.redactoredValues[0].city)
                    }
        },

        async mounted() {
            this.redactoredValues = this.pointsOfRussia
            await this.countNumberOfCities()   
        }, 
    }
</script>

<style scoped>

</style>