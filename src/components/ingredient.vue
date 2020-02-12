<template>
    <div class="c-ingredient">
        <img class="ingredient__thumbnail" :src="thumbnail"/>
        <div class="ingredient__name">{{name}}</div>
    </div>
</template>

<script>
    export default {
        props: ['name', 'thumbnail', 'id']
    }
</script>

<style scoped>
.c-ingredient {
    height: 40px;
    width: 100%;
    border: 1px solid #b7b7b7;
    box-sizing: border-box;
    border-radius: 5px;
    box-shadow: 5px 5px 5px #c7c7c7;
    cursor: pointer;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.ingredient__thumbnail {
    width: 100%;
    border-radius: 5px;
}

.ingredient__name {
    font-size: 0.6em;
    font-family: sans-serif;
    padding: 1px;
}

</style>