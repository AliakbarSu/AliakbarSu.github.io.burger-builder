<template>
    <div class="c-monitor">
        <div class="c-monitor__display">
            <img class="display__thumbnail" src="https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581507919/burger-builder/ingredients/monitor-01.png"/>
            <div class="c-monitor__orders">
                <div 
                    v-for="order in orders" 
                    :key="order.id"
                    :class="{'display__caption--isActive': isActive(order.id)}" 
                    class="display__caption">
                    <span>{{order.order.name}}</span>
                    <div class='c-monitor__preview' v-if="preview && order.id == activeOrder.id">
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
             <div class="c-monitor__controls c-monitor__controls--bottom">
                <div class="c-monitor__buttons c-monitor__buttons--red" @click="prev">Prev</div>
                <div class="c-monitor__buttons c-monitor__buttons--blue" @click="next">Next</div>
            </div>
        </div>
        <div class="c-makeline__table"></div>
    </div>
</template>

<script>
// import draggable from 'vuedraggable'
    export default {
        props: ['preview', 'previewBurger', 'orders', 'activeOrder'],
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
            prev() {
                this.$emit('prev')
            },
            next() {
                this.$emit('next')
            },
            togglePreview() {
                this.$emit('preview')
            },
            isActive(orderId) {
                return this.activeOrder.id == orderId
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
    flex-wrap: wrap;
    top: 20px;
    right: 25px;
    width: 272px;
    max-height: 192px;
    overflow-y: scroll;
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
    /* overflow: scroll; */
    margin: 2px;
}

.display__caption--isActive {
    border-top: 3px solid red !important;
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

.c-monitor__controls--bottom {
    top: 241px;
    width: 100px;
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

    .c-monitor__controls--bottom {
        right: 20px;
        top: 181px;
        width: 71px;
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