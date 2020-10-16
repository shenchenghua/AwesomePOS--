<template>
    <div class="sy">
        旧： {{old}}
        新： {{newVal}}
        <button @click="changeVal">change</button>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                old:1
            }
        },
        computed:{
            newVal(){
                return this.old*2
            }
        },
        methods:{
            changeVal(){
                this.old+=1
            }
        }
    }
</script>

<style  scoped>
    .sy{
        height: 100%;
        background-color: red;
    }
</style>