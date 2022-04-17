<template>
    <div class="all-content">
        <NavSection
            :pointsOfRussua="pointsOfRussua"
        />
        <section class="map-section" id="map">

        </section>
    </div>
</template>

<script>
    import NavSection from '@/components/NavSection.vue'
    import BranchesPoints from '@/json/data.json';

    export default {
        components: {
            NavSection
        },

        data () {
            return {
                pointsOfRussua: []
            }
            
        },

        methods: {
            getRussianPoints() {
                for (let i = 0; i < BranchesPoints.features.length; i++) {
                    //console.log(JSON.parse(BranchesPoints))
                    //currentFeature = BranchesPoints.features[i] 
                    (BranchesPoints.features[i].country === 'Россия') ? this.pointsOfRussua.push(BranchesPoints.features[i]) : null
                }
                console.log(this.pointsOfRussua)
            },

            async createMap() {
                ymaps.ready(init);
                function init () {
                    var myMap = new ymaps.Map('map', {
                        center: [55.76, 37.64],
                        zoom: 3
                    }, {
                        searchControlProvider: 'yandex#search'
                    }),
                    objectManager = new ymaps.ObjectManager({
                        // Чтобы метки начали кластеризоваться, выставляем опцию.
                        clusterize: true,
                        // ObjectManager принимает те же опции, что и кластеризатор.
                        gridSize: 32,
                        clusterDisableClickZoom: true
                    });

                    // Чтобы задать опции одиночным объектам и кластерам,
                    // обратимся к дочерним коллекциям ObjectManager.
                    objectManager.objects.options.set('preset', 'islands#greenDotIcon');
                    objectManager.clusters.options.set('preset', 'islands#greenClusterIcons');
                    myMap.geoObjects.add(objectManager);

                    /*
                    $.ajax({
                        url: "data.json"
                    }).done(function(data) {
                        objectManager.add(data);
                    });
                    */

                    //objectManager.add(BranchesPoints)   //тут цикл
                    for (let i = 0; i < BranchesPoints.features.length; i++) {
                            objectManager.add(BranchesPoints.features[i])
                        }
                }
            }
        },

        async mounted() {
            await this.createMap()
            this.getRussianPoints()
        },         
    }

</script>

<style scoped>
    .all-content{
        max-width: 100%;
        max-height: 100%;
        display: flex;
        overflow: hidden;
    }

    .map-section{
        height: 100vh;
        width: 100%;
    }
    
</style>