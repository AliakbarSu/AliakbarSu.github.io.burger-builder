<template>
    <div class="c-monitor">
        <div class="c-monitor__display">
            <img class="display__thumbnail" src="https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581507919/burger-builder/ingredients/monitor-01.png"/>
            <div class="c-monitor__orders">
                <div class="display__caption">
                    <span>{{currentOrder.name}}</span>
                    <div class='c-monitor__preview' v-if="preview">
                    <div v-for="(pr, index) in previewBurger" :key="index" class='c-ingredients__items'>
                        {{pr}}
                    </div>
                </div>
                </div>
            </div>
            <div class="c-monitor__controls">
                <div @click="togglePreview" class="c-monitor__buttons">{{preview ? 'Hide' : 'Show'}}</div>
                <div class="c-monitor__buttons c-monitor__buttons--blue" @click="auto">Auto</div>
                <div class="c-monitor__buttons c-monitor__buttons--red" @click="reset">Reset</div>
                <div class="c-monitor__buttons c-monitor__buttons--green" @click="serve">Serve</div>
                <div class="c-monitor__buttons c-monitor__buttons--purple" @click="newOrder">New</div>
            </div>
        </div>
        <!-- <div class="c-monitor__controls">
            <div class="c-monitor__buttons c-monitor__buttons--purple" @click="newOrder">New Order</div>
            <div class="c-monitor__buttons c-monitor__buttons--blue" @click="auto">Auto</div>
            <div class="c-monitor__buttons c-monitor__buttons--green" @click="serve">Serve</div>
            <div class="c-monitor__buttons c-monitor__buttons--red" @click="reset">Reset</div>
            <div @click="togglePreview" class="c-monitor__buttons c-monitor__buttons--full-width">{{preview ? 'Hide Ingredients' : 'Show Ingredients'}}</div>
        </div>
        <draggable 
            class="c-monitor__bin" 
            tag='div' 
            group="people" 
            @start="drag=true"  
            @end="drag=false">
            <img class="rabish-bin__thumbnail" src='https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581199981/burgerIngredients/text-symbol-brand-trash.png'/>
        </draggable> -->
        <div class="c-makeline__table"></div>
    </div>
</template>

<script>
// import draggable from 'vuedraggable'
    export default {
        props: ['currentOrder', 'preview', 'previewBurger'],
        methods: {
            serve() {
                this.$emit('serve')
            },
            newOrder() {
                this.$emit('newOrder')
            },
            auto() {
                this.$emit('auto')
            },
            reset() {
                this.$emit('reset')
            },
            togglePreview() {
                this.$emit('preview')
            }
        },
        components: {
            // draggable
        }
    }
</script>

<style scoped>

.c-monitor {
    text-align: center;
}

.c-monitor__display {
    position: relative;
    min-height: 220px;
    width: 400px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
}

.display__thumbnail {
    width: 100%;
}

.c-monitor__orders {
    position: absolute;
    display: flex;
    top: 20px;
    right: 25px;
    width: 272px;
    max-height: 192px;
}

.display__caption {
    text-align: center;
    text-align: left;
    background: green;
    padding: 3px;
    padding-left: 5px;
    width: 100px;
    color: white;
    font-size: 0.8em;
    border-top: 3px solid yellow;
    min-height: 20px;
    max-height: 176px;
    overflow: scroll;
}

.c-monitor__controls {
    display: flex;
    flex-wrap: wrap;
    position: absolute;
    right: 27px;
    top: 223px;
    justify-content: space-between;
    width: 254px;
}

.c-monitor__buttons {
    box-sizing: border-box;
    padding: 0 10px;
    text-align: center;
    color: white;
    font-family: sans-serif;
    cursor: pointer;
    font-size: 0.7em;
    width: 48px;
}

.c-monitor__preview {
    font-size: 0.8em;
}


.c-monitor__buttons--full-width {
    width: 100%;
}

.rabish-bin__thumbnail {
    height: 150px;
    margin: 20px auto;
}

.c-makeline__table {
    height: 20px;
    width: 100%;
    background-image: url('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581079889/burgerIngredients/85328920-light-wood-texture-background-surface-with-old-natural-pattern-or-old-wood-texture-table-top-view-gr.jpg');
    background-size: cover;
}

@media screen and (max-width: 450px) {
    .c-monitor__display {
        width: 300px;
    }

    .c-monitor__controls {
        right: 20px;
        top: 168px;
        width: 190px;
    }

    .c-monitor__buttons {
        padding: 0 5px;
        font-size: 0.6em;
        width: 32px;
    }

    .c-monitor__orders {
        width: 200px;
        max-height: 135px;
    }
}

</style>