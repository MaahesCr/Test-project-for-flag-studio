<template>
            <div class="tab-content" id="pills-tabContent">
                <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">
                    <div v-for="nums in pointsOfRussua.length">
                        <p>
                            <a class="btn btn-primary" data-bs-toggle="collapse" href="#multiCollapseExample1" role="button" aria-expanded="false" aria-controls="multiCollapseExample1">Переключить первый элемент</a>
                        </p>
                        <div class="collapse multi-collapse" id="multiCollapseExample1">
                            <div class="card card-body">
                                Некоторый заполнитель для первого компонента сворачивания в этом примере множественного сворачивания. Эта панель по умолчанию скрыта, но открывается, когда пользователь активирует соответствующий триггер.
                            </div>
                        </div>
                        Раскрывающийся список по городу с количеством городов
                        {{redactoredValues.length}}
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
                redactoredValues: []
            }
            
        },

        methods: {
            countNumberOfCities() {
                
            }
        },

        props: {  
          pointsOfRussua: {
                type: Array,
                required: true
            }
        },

        async mounted() {
            this.redactoredValues = this.pointsOfRussua;
        }, 
    }
</script>

<style scoped>

</style>