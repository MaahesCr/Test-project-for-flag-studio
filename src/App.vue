<template>
    <div class="all-content">
        <NavSection style="overflow:auto"
            :pointsOfRussia="pointsOfRussia" 
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
                pointsOfRussia: [],
                pointsOfBelarus: [],
                propertiesValues: ["balloonContentHeader", "balloonContentBody", "balloonContentFooter", "clusterCaption"],
            }
            
        },

        methods: {
            getRussianPoints() {
                // формирую пропс только с российскими точками
                for (let i = 0; i < BranchesPoints.features.length; i++) {
                    (BranchesPoints.features[i].country === 'Россия') ? this.pointsOfRussia.push(BranchesPoints.features[i]) : null
                }   
                // удаляю html разметку из пропса
                for (let i = 0; i < Object.keys(this.pointsOfRussia).length; i++){
                    for (let j = 0; j < this.propertiesValues.length; j++){
                        let currentVarOfProp = this.propertiesValues[j]
                        this.pointsOfRussia[i].properties[currentVarOfProp] = this.pointsOfRussia[i].properties[currentVarOfProp].replace(/<(.|\n)*?>/g, '')
                    }  
                }
            },

            getBelarusPoints() {
                for (let i = 0; i < BranchesPoints.features.length; i++) {
                    (BranchesPoints.features[i].country === 'Белоруссия') ? this.pointsOfBelarus.push(BranchesPoints.features[i]) : null
                }   

                for (let i = 0; i < Object.keys(this.pointsOfBelarus).length; i++){
                    for (let j = 0; j < this.propertiesValues.length; j++){
                        let currentVarOfProp = this.propertiesValues[j]
                        this.pointsOfBelarus[i].properties[currentVarOfProp] = this.pointsOfBelarus[i].properties[currentVarOfProp].replace(/<(.|\n)*?>/g, '')
                    }  
                }

                // изменить чтоб сразу был по городам 
                console.log('BP', this.pointsOfBelarus)
            },

            async createMap() {
                ymaps.ready(this.init);
            }, 

            init(){
                var myMap = new ymaps.Map('map', {
                    center: [55.76, 37.64],
                    zoom: 3
                }, {
                    searchControlProvider: 'yandex#search'
                }),
                    objectManager = new ymaps.ObjectManager({
                    clusterize: true,
                    gridSize: 32,
                    clusterDisableClickZoom: true
                });
                    this.initPartTwo(myMap, objectManager)
                    return [myMap, objectManager]
            },
                
            initPartTwo(myMap, objectManager) {
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
                this.getCurrentArea(myMap, objectManager)
                },
                
            getCurrentArea(myMap, objectManager) {
                    myMap.setBounds(myMap.geoObjects.getBounds(), {checkZoomRange:true}).then(function(){
                        if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                    });
            },
                
        },

        async mounted() {
            await this.createMap()
            this.getRussianPoints()
            this.getBelarusPoints()
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