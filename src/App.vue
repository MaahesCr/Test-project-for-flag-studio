<template>
    <div class="all-content">
        <NavSection style="overflow:auto"
            :pointsOfRussia="pointsOfRussia"
            :citesOfRussia="citesOfRussia"
            
        />
        <section class="map-section" id="map">

        </section>
        <button @click="getRussianArea"></button>
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
                citesOfRussia: {},
                propertiesValues: ["balloonContentHeader", "balloonContentBody", "balloonContentFooter", "clusterCaption"],
                placemarkCollections: {},
                russianArea: false
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
                
                console.log('PoR: ', this.pointsOfRussia) 
            },

            getCitesOfRussia() { 
                /*
                this.citesOfRussia[this.pointsOfRussia[0].city] = new Array(this.pointsOfRussia[0]);  
                
                for (let i = 1; i < this.pointsOfRussia.length; i++) {
                    let boolVar = false
                    for (let j = 0; j < Object.keys(this.citesOfRussia).length; j++) {
                        if (Object.keys(this.citesOfRussia)[j] == this.pointsOfRussia[i].city ){
                            console.log('e', Object.values(this.citesOfRussia)[j])//точно то
                            console.log('t', Object.values(Object.values(this.citesOfRussia)[j]))
                            Object.values(this.citesOfRussia)[j]
                            console.log(Object.values(this.citesOfRussia)[j])
                            boolVar = true
                        }   
                    }
                    if (!boolVar)   {
                        this.citesOfRussia[this.pointsOfRussia[i].city] = new Array(this.pointsOfRussia[i]);
                        console.log('BBBB', this.citesOfRussia)
                    }
                }
                console.log('citesOfRussia: ', this.citesOfRussia);
                /*
                this.citesOfRussia[this.pointsOfRussia[0].city] = [this.pointsOfRussia[0]];  
                
                for (let i = 1; i < this.pointsOfRussia.length; i++) {
                    let boolVar = false
                    for (let j = 0; j < Object.keys(this.citesOfRussia).length; j++) {
                        if (Object.keys(this.citesOfRussia)[j] == this.pointsOfRussia[i].city ){
                            console.log('e', Object.values(this.citesOfRussia)[j])//точно то
                            console.log('t', Object.values(Object.values(this.citesOfRussia)[j]))
                            let variable = Object.values(Object.values(this.citesOfRussia)[j]);
                            variable = variable.push(this.pointsOfRussia[i])
                            boolVar = true
                        }   
                    }
                    if (!boolVar)   {
                        this.citesOfRussia[this.pointsOfRussia[i].city] = [this.pointsOfRussia[i]];
                        console.log('BBBB', this.citesOfRussia)
                    }
                }
                console.log('citesOfRussia: ', this.citesOfRussia);
                */
                
                
               this.citesOfRussia[this.pointsOfRussia[0].city] = 1;  
                
                for (let i = 1; i < this.pointsOfRussia.length; i++) {
                    let boolVar = false
                    for (let j = 0; j < Object.keys(this.citesOfRussia).length; j++) {
                        if (Object.keys(this.citesOfRussia)[j] == this.pointsOfRussia[i].city ){
                            this.citesOfRussia[Object.keys(this.citesOfRussia)[j]] +=1 // тут массив с пропсами
                            console.log('AAAA', this.pointsOfRussia[0])
                            boolVar = true
                        }   
                    }
                    if (!boolVar)   {
                        this.citesOfRussia[this.pointsOfRussia[i].city] = 1;
                        console.log('BBBB', this.citesOfRussia)
                    }
                }
                console.log('citesOfRussia: ', this.citesOfRussia);
                
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
                /*
                console.log(BranchesPoints.features[0])
                new ymaps.GeoObjectCollection() new ymaps.Placemark
                myMap.setBounds(BranchesPoints.features[0].getBounds(), {checkZoomRange:true}).then(function(){
                    if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                });
                /*
                var placemarkCollections = {};
                var placemarkList = {};
                var cityCollection = new ymaps.GeoObjectCollection();
                cityCollection.add(Object.values(this.pointsOfRussia))
                placemarkCollections[0] = Object.values(this.pointsOfRussia)
                myMap.setBounds(cityCollection[0].getBounds(), {checkZoomRange:true}).then(function(){
                    if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                });
                */
            },

            getRussianArea(){
                this.russianArea = true
                this.createMap()
            }

                
        },

        async mounted() {
            await this.createMap()
            this.getRussianPoints()
            this.getCitesOfRussia() 
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