<template>
    <div class="c-ingredients">
        <draggable tag="div" class="o-tray" v-model="ingredients" group='people' @start="drag=true" @end="drag=false">
            <div 
                :key="ingredient.name"
                v-for="ingredient in ingredients" class="c-tray__items">
                <Ingredient
                    :name="ingredient.name"
                    :id="ingredient.id"
                    :thumbnail="ingredient.thumbnail"></Ingredient>
            </div>
        </draggable>
       <div class="ingredients__categories" >
          <div class="ingredients__items" :class="{'ingredients__items--active': activeCategory == 'pfc'}" @click="activateItem('pfc')">PFC</div>
          <div class="ingredients__items" :class="{'ingredients__items--active': activeCategory == 'buns'}" @click="activateItem('buns')">Buns</div>
          <div class="ingredients__items" :class="{'ingredients__items--active': activeCategory == 'sauces'}" @click="activateItem('sauces')">Sauces</div>
          <div class="ingredients__items" :class="{'ingredients__items--active': activeCategory == 'others'}" @click="activateItem('others')">Others</div>
      </div>
    </div>
</template>

<script>
import draggable from 'vuedraggable'
import Ingredient from './ingredient.vue'
    export default {
        props: ['pfc', 'buns', 'sauces', 'others'],
        data() {
            return {
                activeCategory: 'buns'
            }
        },
        computed: {
            ingredients: {
                set() {

                },
                get() {
                    return this[this.activeCategory]
                }
            }
        },
        methods: {
            activateItem(category) {
                this.activeCategory = category
            }
        },
        components: {
            draggable,
            Ingredient
        }
    }
</script>

<style  scoped>

.c-ingredients {
    display: flex;
    border: 1px solid #d0d0d0;
}


.o-tray {
    width: 90%;
    display: flex;
    flex-wrap: wrap;
    padding: 5px;
}

.c-tray__items {
    width: 100px;
    margin: 2px;
}

.ingredients__categories {
    width: 10%;
    background: #e47b49;
}

.ingredients__items {
    padding: 5px;
    font-size: 1.2em;
    border-bottom: 1px solid white;
    color: white;
    cursor: pointer;
    font-family: sans-serif;
}

.ingredients__items--active {
    background: white;
    color: #e47b49;
    border: 1px solid #e47b49;
}

@media screen and (max-width: 900px) {
    .o-tray {
        width: 75%;
    }
    .ingredients__categories {
        width: 25%;
        background: #e47b49;
    }
    .c-tray__items {
        width: 90px;
    }
}

</style>