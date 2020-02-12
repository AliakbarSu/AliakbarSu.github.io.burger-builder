<template>
    <div class='c-results'>
        <div class='c-results__promt' v-if="submit && wasMadeCorrectly">Correct</div>
        <div class='c-results__promt c-results__promt--error' v-if="submit && !wasMadeCorrectly">Try again!</div> 
    </div>
</template>

<script>
    export default {
        props: ['submit', 'wasMadeCorrectly']
    }
</script>

<style scoped>

.c-results {
    height: 120px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: whitesmoke;
}

.c-results__promt {
    width: 120px;
    padding: 20px;
    border-radius: 5px;
    background: green;
    color: white;
    text-align: center;
    box-shadow: 5px 5px 5px #bfbfbf;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.c-results__promt--error {
    background: red;
}

</style>