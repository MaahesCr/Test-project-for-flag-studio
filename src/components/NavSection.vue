<template>
        <section class="nav-section">
            <ul class="nav nav-pills" id="pills-tab" role="tablist">
                <li class="nav-item nav-li" role="presentation">
                    <button class="nav-link active" id="pills-home-tab" data-bs-toggle="pill" data-bs-target="#pills-home" type="button" role="tab" aria-controls="pills-home" aria-selected="true">Россия</button>
                </li>
                <li class="nav-item nav-li" role="presentation">
                    <button class="nav-link" id="pills-profile-tab" data-bs-toggle="pill" data-bs-target="#pills-profile" type="button" role="tab" aria-controls="pills-profile" aria-selected="false">Белоруссия</button>
                </li>
            </ul>
            <div class="tab-content" id="pills-tabContent">
                <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">Это некий заполнитель содержимого, связанного с содержимым вкладки "Главная". Щелчок по другой вкладке переключает видимость этой на следующую. Вкладка JavaScript меняет местами классы для управления видимостью и стилем содержимого. Вы можете использовать его с вкладками, таблетками и любой другой навигацией с поддержкой .nav.</div>
                <div class="tab-pane fade" id="pills-profile" role="tabpanel" aria-labelledby="pills-profile-tab">...</div>
            </div>
        </section>
</template>

<script>
    export default {
        
    }
</script>

<style>
    .nav-section{
        background: #FAFAFA;
        width: 25%;
        height: 100vh;
        border-right: 1px solid black;
    }

    .nav-li{
        width: 50%;
        border-radius: 1px;
    }

    .nav-link{
        width: 100%;
        color: #929292;
    }

    .nav-pills .nav-link.active{
        background-color: orange;
    }

</style>