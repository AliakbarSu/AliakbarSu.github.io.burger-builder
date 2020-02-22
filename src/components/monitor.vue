<template>
    <div class="c-monitor">
        <div class="c-monitor__display">
            <img class="display__thumbnail" src="https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581507919/burger-builder/ingredients/monitor-01.png"/>
            <div class="c-monitor__orders">
                <div
                    @click="makeActive(index)" 
                    v-for="(order, index) in orders" 
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
                <div class="c-monitor__buttons c-monitor__buttons--blue" @click="clear">Clear</div>
                <div class="c-monitor__buttons c-monitor__buttons--red" @click="prev">Prev</div>
                <div class="c-monitor__buttons c-monitor__buttons--blue" @click="next">Next</div>
            </div>
            <div class="c-monitor__categories">
                <h2 class="c-categories__heading">Filters</h2>
                <div @click="addFilter(cat)" class="c-categories__items" :class="{'c-categories__items--active': isInFilters(cat)}" v-for="cat in categories" :key="cat">{{cat}}</div>
            </div>
        </div>
        <div class="c-makeline__table"></div>
    </div>
</template>

<script>
// import draggable from 'vuedraggable'
    export default {
        props: ['preview', 'previewBurger', 'orders', 'activeOrder', 'categories', 'filters'],
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
            clear() {
                this.$emit('clear')
            },
            togglePreview() {
                this.$emit('preview')
            },
            isActive(orderId) {
                return this.activeOrder.id == orderId
            },
            makeActive(orderIndex) {
                this.$emit("makeActive", orderIndex);
            },
            addFilter(filter) {
                this.$emit("addFilter", filter)
            },
            isInFilters(cat) {
                return this.filters.indexOf(cat) !== -1;
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
    max-height: 40px;
    height: auto;
    overflow: visible;
    margin: 2px;
    cursor: pointer;
}

.display__caption--isActive {
    border-top: 3px solid red !important;
    max-height: 600px;
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
    width: 150px;
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

.c-monitor__categories {
    display: flex;
    flex-wrap: wrap;
    position: absolute;
    right: 32px;
    top: 300px;
    justify-content: space-between;
    width: 254px;
}

.c-categories__heading {
    width: 100%;
    background: #e8e8e8;
    padding: 2px 5px;
    margin: 0;
    margin-bottom: 5px;
    font-size: 1em;
    text-align: left;
}

.c-categories__items {
    background: brown;
    padding: 5px;
    border-radius: 5px;
    color: white;
    cursor: pointer;
}

.c-categories__items--active {
    background: green;
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
        width: 110px;
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

    .c-monitor__categories {
        font-size: 0.8em;
        right: 11px;
        top: 217px;
        width: 220px;
    }
}

</style>