<template>
    <div class="all-content">
        <NavSection/>
        <section class="map-section" id="map">

        </section>
    </div>
    <button  @click="logData"></button>
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
                dataJson: {}
            }
            
        },
        methods: {
            logData(data){
                console.log(BranchesPoints)
            },

            async createMap() {
                ymaps.ready(init);
                function init () {
    var myMap = new ymaps.Map('map', {
            center: [55.76, 37.64],
            zoom: 10
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

    objectManager.add(BranchesPoints)

        var placemark = new ymaps.Placemark(myMap.getCenter(), {
        // Зададим содержимое заголовка балуна.
        balloonContentHeader: '<a href = "#">Рога и копыта</a><br>' +
            '<span class="description">Сеть кинотеатров</span>',
        // Зададим содержимое основной части балуна.
        balloonContentBody: '<img src="img/cinema.jpg" height="150" width="200"> <br/> ' +
            '<a href="tel:+7-123-456-78-90">+7 (123) 456-78-90</a><br/>' +
            '<b>Ближайшие сеансы</b> <br/> Сеансов нет.',
        // Зададим содержимое нижней части балуна.
        balloonContentFooter: 'Информация предоставлена:<br/>OOO "Рога и копыта"',
        // Зададим содержимое всплывающей подсказки.
        hintContent: 'Рога и копыта'
    });
    // Добавим метку на карту.
    //{"type": "Feature", "id": 0, "geometry": {"type": "Point", "coordinates": [55.831903, 37.411961]}, "properties": {"balloonContentHeader": "<font size=3><b><a target='_blank' href='https://yandex.ru'>Здесь может быть ваша ссылка</a></b></font>", "balloonContentBody": "<p>Ваше имя: <input name='login'></p><p><em>Телефон в формате 2xxx-xxx:</em>  <input></p><p><input type='submit' value='Отправить'></p>", "balloonContentFooter": "<font size=1>Информация предоставлена: </font> <strong>этим балуном</strong>", "clusterCaption": "<strong><s>Еще</s> одна</strong> метка", "hintContent": "<strong>Текст  <s>подсказки</s></strong>"}},
        
    myMap.geoObjects.add(placemark);
}
            }
        },
        async mounted() {
            await this.createMap()
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