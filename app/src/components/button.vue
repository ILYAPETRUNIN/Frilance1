<template>
    <button>{{label}}</button>
</template>

<script>
export default {
    props:{
        name:{type:String},
        label:{type:String},
        disabled:{type:Boolean,default:false}
    }
}
</script>

<style>

</style>