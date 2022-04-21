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
        background: #FFF;
        border-radius: 0;
        
        Box-shadow:0px 1px 0px #000,0px -1px 0px #000,1px 0px 0px #000,-1px 0px 0px #000;
        transition: all 1s ease 0s;
    }

    .nav-pills .nav-link.active{
        background-color: #FF9e00;
        border: none;
        Box-shadow: none;
        transition: all 1s ease 0s;
    }

</style>