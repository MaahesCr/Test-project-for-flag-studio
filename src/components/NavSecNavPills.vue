<template>
            <ul class="nav nav-pills" id="pills-tab" role="tablist">
                <li class="nav-item nav-li" role="presentation">
                    <button class="nav-link active" id="pills-home-tab" data-bs-toggle="pill" data-bs-target="#pills-home" type="button" role="tab" aria-controls="pills-home" aria-selected="true"
                    @click="updateRussinPoints"
                    >Россия
                    </button>
                </li>
                <li class="nav-item nav-li" role="presentation">
                    <button class="nav-link" id="pills-profile-tab" data-bs-toggle="pill" data-bs-target="#pills-profile" type="button" role="tab" aria-controls="pills-profile" aria-selected="false"
                    @click="updateBelarusPoints"
                    >Белоруссия</button>
                </li>
            </ul>
</template>

<script>
    export default {
        methods: {
            updateRussinPoints (){
                this.$emit('updateRu', 0)
            },

            updateBelarusPoints (){
                this.$emit('updateBl', 1)
            }
        }
    }
</script>

<style scoped>
    .nav-li{
        width: 50%;
        border-radius: 1px;
    }

    .nav-link{
        width: 100%;
        color: #929292;
        border-radius: 0;
    }

    .nav-pills .nav-link.active{
        background-color: orange;
    }

</style>