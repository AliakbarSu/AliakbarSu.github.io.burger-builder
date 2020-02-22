<template>
    <div class="o-makeline">
        <div class="c-makeline">
            <draggable
                tag='div' class="c-makeline__plates" 
                v-model="leftIngredients" 
                group="people"
                :style="{'background-image': getBoxThumbnail('top')}" 
                @start="drag=true" 
                @end="drag=false">
                <img 
                    v-for="ingredient in leftIngredients"
                    :key="ingredient.name" 
                    :src="ingredient.thumbnail" 
                    class="burger__ingredient"/>
            </draggable>
        <draggable
                tag='div' 
                class="c-makeline__plates"  
                v-model="rightIngredients" 
                group="people"
                :style="{'background-image': getBoxThumbnail('bottom')}"
                @start="drag=true"  
                @end="drag=false">
                <img 
                    v-for="ingredient in rightIngredients"
                    :key="ingredient.name" 
                    :src="ingredient.thumbnail" 
                    class="burger__ingredient"/>
                <!-- <div ></div> -->
            </draggable>

            <div class='c-steam-oven'>
                <img @click="toggleSteam" v-if='isSteamed' class='c-steam-oven__thumbnail' src='https://res.cloudinary.com/dxuf2ssx6/image/upload/v1582240312/burger-builder/ingredients/on-oven-01.png'/>
                <img @click="toggleSteam" v-else class='c-steam-oven__thumbnail' src='https://res.cloudinary.com/dxuf2ssx6/image/upload/v1582240312/burger-builder/ingredients/oven-01.png'/>
            </div>
        
            <draggable 
                class="c-makeline__bin" 
                tag='div' 
                group="people" 
                @start="drag=true"  
                @end="drag=false">
                <img  class='c-bin__thumbnail' src='https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581514138/burger-builder/ingredients/bin-01.png'/>
            </draggable>

            <div class="c-makeline__table"></div>
        </div>
    </div>
   
</template>

<script>
import draggable from 'vuedraggable'

    export default {
        props: ['left', 'right', 'steamed', 'wasMadeCorrectly', 'submit'],
        data() {
            return {
                leftIngredients: [],
                rightIngredients: []
            }
        },
        beforeMount() {
            this.leftIngredients = this.left.ingredients;
            this.rightIngredients = this.right.ingredients;
        },
        components: {
            draggable
        },
        watch: {
            leftIngredients: function (updatedIngredients) {
                this.$emit('leftUpdate', updatedIngredients)
            },
            rightIngredients: function (updatedIngredients) {
                this.$emit('rightUpdate', updatedIngredients)
            },
            left: function() {
                this.leftIngredients = this.left.ingredients;
                this.rightIngredients = this.right.ingredients;
            }
        },
        methods: {
            removeIngredient() {
                return;
            },
            test() {
                console.log('test')
            },
            toggleSteam() {
                this.$emit('toggleSteam')
            },
            getUrl(url) {
                return `url(${url})`;
            },
            getBoxThumbnail(pos) {
                if(this.submit && !this.wasMadeCorrectly) {
                    if(pos == "top") {
                        return this.getUrl('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1582240312/burger-builder/ingredients/red-burger-box-01.png');
                    }
                    return this.getUrl('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1582240312/burger-builder/ingredients/red-burger-box-bottom-01.png')

                }else if(this.submit && this.wasMadeCorrectly) {
                    return this.getUrl('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581542822/burger-builder/ingredients/green-burger-box-01.png')
                }else {
                    if(pos == "top") {
                        return this.getUrl('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1582240312/burger-builder/ingredients/burger-box-top-01.png')                        
                    }
                    return this.getUrl('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1582240312/burger-builder/ingredients/burger-box-01.png')
                }
                
            }
        },
        computed: {
            isSteamed() {
                return this.steamed
            }
        }
    }
</script>

<style scoped>


.c-makeline {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    width: 100%;
}

.c-steam-oven {
    position: absolute;
    top: 0;
    right: 0;
    padding-right: 5px;
}

.c-makeline__bin {
    position: absolute;
    top: 120px;
    right: 0;
    padding: 5px;
}

.c-bin__thumbnail {
    height: 35px;
    cursor: pointer;
}

.c-steam-oven__thumbnail {
    height: 120px;
    cursor: pointer;
}

.burger__ingredient {
  width: 115px;
  height: 20px;
  margin-top: -2px;
}

.c-makeline__table {
    height: 20px;
    width: 100%;
    background-image: url('https://res.cloudinary.com/dxuf2ssx6/image/upload/v1581079889/burgerIngredients/85328920-light-wood-texture-background-surface-with-old-natural-pattern-or-old-wood-texture-table-top-view-gr.jpg');
    background-size: cover;
}

.c-makeline__plates {
    min-height: 65px;
    padding-bottom: 37px;
    width: 200px;
    margin: 0 10px;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: bottom;
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    align-items: center;
    justify-content: flex-end;
}

@media screen and (max-width: 900px) {
  .c-makeline__plates {
    width: 180px;
  }

  .c-makeline__bin {
        left: 0;
        top: 0;
        width: 40px;
    }

    .o-makeline {
        padding-top: 120px;
    }
}


@media screen and (max-width: 380px) {
    .c-makeline__plates {
        width: 150px;
    }
}

</style>