<template>
    <div class="all-content">
        <NavSection style="overflow:auto"
            :pointsOfRussia="pointsOfRussia" 
            :pointsOfBelarus="pointsOfBelarus"
            @updateCountryArea="changeArea"
            @getIdOfPoint="openBalloonById"
        />
        <div class="shadow-on-the-map"></div>
        <section class="map-section" id="map" :key="updateMap">

        </section>
    </div>
</template>

<script>
    import NavSection from '@/components/NavSection.vue'
    import BranchesPoints from '../public/data.json';
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
                curentArea: -1,
                idOfPoint: -1,
                updateMap: ''
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
                    zoom: 5,
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
                    clusterDisableClickZoom: true,
                });
                this.initPartTwo(myMap, objectManager)
            },
                
            initPartTwo(myMap, objectManager) {
                objectManager.objects.options.set('preset', 'islands#nightCircleDotIcon');
                objectManager.clusters.options.set('preset', 'islands#nightClusterIcons');

                // стиль для балуна
                let MyBalloonLayout = ymaps.templateLayoutFactory.createClass(
                    '<div style ="color:white; background:#253b62; border-radius: 0; padding: 10px 15px; display: flex;flex-direction: column; width: auto; max-width: 500px; top: -90px;" class="popover top">' +
                        '<a style ="color:#FFF; text-decoration:none; align-self: flex-end; position:absolute" class="close" href="#">&times;</a>' +
                        '<div class="popover-inner">' +
                            '$[[options.contentLayout observeSize width=auto height=auto maxHeight=350 word-wrap=break-word]]' +
                        '</div>' +
                        '<div class="arrow"></div>'+
                        '<div style="transform: rotate(45deg); height: 20px; width: 20px;align-self: center;margin-bottom: -20px; background:#253b62 ;"></div>' +
                    '</div>'
                    , {
                    build: function () {
                        this.constructor.superclass.build.call(this);
                        this._$element = $('.popover', this.getParentElement());
                        this.applyElementOffset();
                        this._$element.find('.close')
                            .on('click', $.proxy(this.onCloseClick, this));
                    },

                    clear: function () {
                        this._$element.find('.close')
                            .off('click');
                        this.constructor.superclass.clear.call(this);
                    },

                    onSublayoutSizeChange: function () {
                        MyBalloonLayout.superclass.onSublayoutSizeChange.apply(this, arguments);
                        if(!this._isElement(this._$element)) {
                            return;
                        }
                        this.applyElementOffset();
                        this.events.fire('shapechange');
                    },

                    applyElementOffset: function () {
                        this._$element.css({
                            left: -(this._$element[0].offsetWidth / 2),
                            top: -(this._$element[0].offsetHeight + this._$element.find('.arrow')[0].offsetHeight)
                        });
                    },

                    onCloseClick: function (e) {
                        e.preventDefault();

                        this.events.fire('userclose');
                    },

                    getShape: function () {
                        if(!this._isElement(this._$element)) {
                            return MyBalloonLayout.superclass.getShape.call(this);
                        }
                        var position = this._$element.position();
                        return new ymaps.shape.Rectangle(new ymaps.geometry.pixel.Rectangle([
                            [position.left, position.top], [
                                position.left + this._$element[0].offsetWidth,
                                position.top + this._$element[0].offsetHeight + this._$element.find('.arrow')[0].offsetHeight
                            ]
                        ]));
                    },

                    _isElement: function (element) {
                        return element && element[0] && element.find('.arrow')[0];
                    }
                })
                // установка стиля для балуна
                objectManager.options.set({
                    hideIconOnBalloon: true,
                    balloonLayout: MyBalloonLayout,
                    balloonShadow: false,
                    balloonAutoPan: false,
                    balloonOffset: [3, -20]
                    });
                //установка стиля для кластера
                objectManager.objects.options.set({
                    hideIconOnBalloon: true,
                    balloonLayout: MyBalloonLayout,
                    balloonShadow: false,
                    balloonAutoPan: false,
                    balloonOffset: [3, -20]
                    });
                // центрация при клике на поинт 
                objectManager.events.add('click', async function (e) { 
                    let coords = e.get('coords');
                    myMap.setCenter([coords[0], coords[1]])
                });
                // центрация при клике на ujhjl cktdf
                if (this.idOfPoint != -1){
                    let coords = BranchesPoints.features[this.idOfPoint].geometry.coordinates
                    myMap.setCenter([coords[0], coords[1]])
                    myMap.setZoom(6)
                }
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
                
                myMap.geoObjects.add(objectManager);

                try {
                    objectManager.objects.balloon.open(this.idOfPoint)
                    objectManager.objects.balloon.open(this.idOfPoint)
                } catch {}
                
                this.getCurrentArea(myMap, objectManager)
            },
                
            getCurrentArea(myMap, objectManager) {
                let cityCollection = new ymaps.GeoObjectCollection({}, {
                    clusterize: true,
                    gridSize: 32,
                    clusterDisableClickZoom: true
                }) 

                let placemarkList = {} 
                let placemarkCollections = {};
                let arr = [this.arrayOfRusPoint, this.arrayOfBelPoint]

                for (let j = 0; j < arr.length; j++){
                    for (let key in arr[j]) {
                        let shopPlacemark = new ymaps.Placemark(
                            Object.values(Object.values(Object.values(arr[j][key]))[2].coordinates), 
                            {
                                features: Object.values(arr[j][key])[1],
                                balloonContentHeader: Object.values(arr[j][key])[3].balloonContentHeader,
                                balloonContentBody: Object.values(arr[j][key])[3].balloonContentBody,
                                balloonContentFooter: Object.values(arr[j][key])[3].balloonContentFooter
                            }
                        )   
                        placemarkList[key] = shopPlacemark  
                        cityCollection.add(shopPlacemark)
                    }
                    placemarkCollections[j] = cityCollection

                    cityCollection.options.set('preset', 'islands#nightCircleIcon');
                    cityCollection.options.set('visible', false);
                    
                    myMap.geoObjects.add(cityCollection);

                    cityCollection = new ymaps.GeoObjectCollection({}, {
                    clusterize: true,
                    gridSize: 32,
                    clusterDisableClickZoom: true
                })
                }
                
                if (this.idOfPoint == -1) {myMap.setBounds(placemarkCollections[this.curentArea].getBounds(), {checkZoomRange:true}).then(function(){
                    if(myMap.getZoom() > 15) myMap.setZoom(15); // Если значение zoom превышает 15, то устанавливаем 15.
                });}
            },

            changeArea(curentCountry) {
                this.curentArea = curentCountry
                this.updateMap = Date.now()
                this.idOfPoint = -1
                this.createMap()
            },

            openBalloonById(id) {
                this.idOfPoint = id
                this.updateMap = Date.now()
                //$('#map').empty()
                this.createMap()
            }
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

    .shadow-on-the-map{
        position: absolute;
        width: 8px;
        height: 100vh;
        margin-left: 20%;
        z-index: 999999;
        background: rgb(0,0,0);
        background: linear-gradient(90deg, rgba(70,70,70,1) 0%, rgba(255,255,255,0) 80%);
        pointer-events: none;
    }

    .map-section{
        height: 100vh;
        width: 100%;
    }
    
    
    
</style>