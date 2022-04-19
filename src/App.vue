<template>
    <div class="all-content">
        <NavSection style="overflow:auto"
            :pointsOfRussia="pointsOfRussia" 
            :pointsOfBelarus="pointsOfBelarus"
            @updateCountryArea="changeArea"
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
                arrayOfRusPoint: [],
                arrayOfBelPoint: [],
                propertiesValues: ["balloonContentHeader", "balloonContentBody", "balloonContentFooter", "clusterCaption"],
                curentArea: -1
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

                this.arrayOfRusPoint = this.pointsOfRussia

                let citesAndPointsOfRussia = {}
                let boolVal = false 
                citesAndPointsOfRussia[Object.values(this.pointsOfRussia)[0].city] = new Array(Object.values(this.pointsOfRussia)[0])
                for(let i = 1; i < Object.keys(this.pointsOfRussia).length; i++){
                    for (let j = 0; j < Object.keys(citesAndPointsOfRussia).length; j++) {
                        if (Object.values(this.pointsOfRussia)[i].city == Object.keys(citesAndPointsOfRussia)[j]) {  
                            Object.values(citesAndPointsOfRussia)[j].push(Object.values(this.pointsOfRussia)[i]) 
                            boolVal = true
                        }
                    }
                                    
                    if (!boolVal)   {
                        citesAndPointsOfRussia[Object.values(this.pointsOfRussia)[i].city] = new Array(Object.values(this.pointsOfRussia)[i])
                    }
                    boolVal = false
                }
                this.pointsOfRussia = citesAndPointsOfRussia;
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
                
                this.arrayOfBelPoint = this.pointsOfBelarus

                let citesAndPointsOfBelarus = {}
                let boolVal = false
                citesAndPointsOfBelarus[Object.values(this.pointsOfBelarus)[0].city] = new Array(Object.values(this.pointsOfBelarus)[0])
                for(let i = 1; i < Object.keys(this.pointsOfBelarus).length; i++){
                    for (let j = 0; j < Object.keys(citesAndPointsOfBelarus).length; j++) {
                        if (Object.values(this.pointsOfBelarus)[i].city == Object.keys(citesAndPointsOfBelarus)[j]) {  //Object.values(this.citesAndPointsOfRussia)[0][0].city
                            Object.values(citesAndPointsOfBelarus)[j].push(Object.values(this.pointsOfBelarus)[i]) 
                            boolVal = true
                        }
                    }

                    if (!boolVal)   {
                        citesAndPointsOfBelarus[Object.values(this.pointsOfBelarus)[i].city] = new Array(Object.values(this.pointsOfBelarus)[i])
                    }
                    boolVal = false
                }
                this.pointsOfBelarus = citesAndPointsOfBelarus;
            },

            async createMap() {
                ymaps.ready(this.init);
            }, 

            init(){
                let myMap = new ymaps.Map('map', {
                    center: [55.76, 37.64],
                    zoom: 3,
                    controls: [
                        'zoomControl'
                    ],
                    zoomMargin: [20]
                }, {
                    searchControlProvider: 'yandex#search'
                }),
                    objectManager = new ymaps.ObjectManager({
                    clusterize: true,
                    gridSize: 32,
                    clusterDisableClickZoom: true
                });
                //this.myMapArea = myMap
                this.initPartTwo(myMap, objectManager)
            },
                
            initPartTwo(myMap, objectManager) {
                objectManager.objects.options.set('preset', 'islands#nightCircleDotIcon');
                objectManager.clusters.options.set('preset', 'islands#nightClusterIcons');
                myMap.geoObjects.add(objectManager);
                    /*
                    $.ajax({
                        url: "data.json"
                    }).done(function(data) {
                        objectManager.add(data);
                    });
                    */

                    //objectManager.add(BranchesPoints)   //тут цикл   
                    // ----------------- добавление всего на карту, НЕ УДАЛЯТЬ ----------- 
                for (let i = 0; i < BranchesPoints.features.length; i++) {
                    objectManager.add(BranchesPoints.features[i])
                }

                /*myMap.setBounds(myMap.geoObjects.getBounds(), {checkZoomRange:true}).then(function(){
                    if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                });*/
                
                this.getCurrentArea(myMap, objectManager)
                },
                
            getCurrentArea(myMap, objectManager) {
                let cityCollection = new ymaps.GeoObjectCollection({}, {
                    clusterize: true,
                    gridSize: 32,
                    clusterDisableClickZoom: true
                }) 
                var placemarkList = {} 
                var placemarkCollections = {};
                var arr = [this.arrayOfRusPoint, this.arrayOfBelPoint]

                for (let j = 0; j < arr.length; j++){
                    for (let key in arr[j]) {
                        console.log('Вот ето', Object.values(Object.values(Object.values(arr[j][key]))[2].coordinates))
                        let shopPlacemark = new ymaps.Placemark(
                            Object.values(Object.values(Object.values(arr[j][key]))[2].coordinates), 
                            {
                                features: Object.values(arr[j][key])[1],
                                balloonContentHeader: Object.values(arr[j][key])[3].balloonContentHeader,
                                balloonContentBody: Object.values(arr[j][key])[3].balloonContentBody,
                                balloonContentFooter: Object.values(arr[j][key])[3].balloonContentFooter
                            }
                        )
                        console.log('lookAtMe', Object.values(arr[j][key])[3].balloonContentBody)    
                        placemarkList[key] = shopPlacemark  

                        cityCollection.add(shopPlacemark)
                    }
                    placemarkCollections[j] = cityCollection
                    console.log('cc', cityCollection.toArray())

                    cityCollection.options.set('preset', 'islands#nightCircleIcon');
                    cityCollection.options.set('visible', false);
                    //cityCollection.clusters.options.set('preset', 'islands#greenClusterIcons');

                    myMap.geoObjects.add(cityCollection);

                    cityCollection = new ymaps.GeoObjectCollection({}, {
                    clusterize: true,
                    gridSize: 32,
                    clusterDisableClickZoom: true
                })
                }
                console.log('pc', placemarkCollections)
                console.log('CU', this.curentArea)
                
                myMap.setBounds(placemarkCollections[this.curentArea].getBounds(), {checkZoomRange:true}).then(function(){
                    console.log('+')
                    if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                });
                

                /*myMap.setBounds(myMap.geoObjects.getBounds(), {checkZoomRange:true}).then(function(){
                    if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                });*/
            },

            changeArea(curentCountry) {
                this.curentArea = curentCountry
                this.init()
                //this.init(curentCountry)
                console.log(curentCountry)

                /*
                let myMap = new ymaps.Map('map', {
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
                
                objectManager.objects.options.set('preset', 'islands#greenDotIcon');
                objectManager.clusters.options.set('preset', 'islands#greenClusterIcons');
                myMap.geoObjects.add(objectManager);  

                for (let i = 0; i < curentCountry.length; i++) {
                    objectManager.add(Object.values(curentCountry)[i])
                }

                this.getCurrentArea(myMap, objectManager)
                */
            }
                
        },

        async mounted() {
            await this.createMap()
            this.getRussianPoints()
            this.getBelarusPoints()
            //this.changeArea(this.arrayOfRusPoint)
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