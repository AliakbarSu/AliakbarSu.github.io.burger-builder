<template>
  <div id="app">
      <Monitor 
        @newOrder="generateOrder" 
        @serve="serve" 
        @reset="reset"
        @auto="toggleAuto"
        @preview="togglePreview" 
        @next="next"
        @prev="prev"
        @clear="clear"
        @makeActive="makeActive"
        @addFilter="addFilter"
        :filters="filters"
        :categories="categories"
        :orders="orders"
        :activeOrder="getActiveOrder"
        class="c-monitor" 
        :preview="preview"
        :previewBurger='previewBurger'/>
      <!-- <Results :submit="submit" :wasMadeCorrectly="wasMadeCorrectly" class="c-results"/> -->
      <Makeline
        @leftUpdate="leftUpate"
        @rightUpdate="rightUpdate"
        @toggleSteam="toggleSteam"
        :steamed="currentBurger.steamed"
        :wasMadeCorrectly="wasMadeCorrectly"
        :submit="submit"
        class='o-makeline' 
        :left="currentBurger.left" 
        :right="currentBurger.right"/>
      <Ingredients class="c-ingredients" :buns="buns" :pfc="pfc" :sauces="sauces" :others="others"/>
  </div>
</template>

<script>
import { burgers, ingredients } from './data.js';
import Ingredients from './components/ingredients.vue'
import Makeline from './components/makeline.vue'
import Monitor from './components/monitor.vue'
// import Results from './components/results.vue'

export default {
  name: 'app',
  data() {
    return {
      auto: false,
      autoInterval: null,
      waste: [],
      ingredients,
      burgers,
      filters: [],
      orders: [],
      activeOrder: "",
      currentBurger: {
        left: {
          ingredients: [],
        },
        right: {
          ingredients: [],
        },
        steamed: false
      },
      wasMadeCorrectly: false,
      submit: false,
      preview: false
    }
  },
  components: {
    Ingredients,
    Makeline,
    Monitor,
    // Results
  },
  mounted() {
    this.audio = new Audio(require('./assets/sounds/newOrder.mp3'));
  },
  methods: {
    generateId() {
      return '_' + Math.random().toString(36).substr(2, 9);
    },
    serve() {
      this.submit = true;
      const burger = this.burgers.find(bg => bg.id == this.getActiveOrder.order.id);
      const leftIngredientsCopy = [...this.currentBurger.left.ingredients]
      leftIngredientsCopy.reverse()
      const currentBurgerIngredients = [...leftIngredientsCopy, ...this.currentBurger.right.ingredients]
      const currentIngredientIds = currentBurgerIngredients.map(ing => ing.id);
      if(burger) {
        this.wasMadeCorrectly = burger.ingredients.length === currentIngredientIds.length && burger.ingredients.every((item, index) => currentIngredientIds[index] === item) && burger.steamed == this.currentBurger.steamed;
      if(this.wasMadeCorrectly) {
        const orderIndex = this.orders.findIndex(ord => ord.id == this.activeOrder)
        const prevOrder = this.activeOrder
        if(orderIndex == this.orders.length - 1) {
          this.prev()
        }else {
          this.next()
        }
        this.orders = this.orders.filter(order => order.id !== prevOrder)
        setTimeout(() => {
          this.reset()
        }, 2000)
        }
      }
    },
    generateOrder() {
      function getRandomIndex(min, max) {
        // min = Math.ceil(min);
        // max = Math.floor(max);
        return Math.floor(Math.random() * (max - min)) + min;
      }
      const randomBurger = this.filteredBurgers[getRandomIndex(0, this.filteredBurgers.length)];
      this.currentBurger.id = randomBurger.id;
      this.currentBurger.name = randomBurger.name;
      const order = {
        id: this.generateId(),
        order: {
          id: randomBurger.id,
          name: randomBurger.name
        }
      }
      this.orders.push(order)
      this.newOrderPlay()
      if(this.orders.length == 1) {
        this.activeOrder = this.orders[0].id
      }
    },
    reset() {
      this.submit = false;
      this.currentBurger.left = {...this.currentBurger.left, ingredients: []}
      this.currentBurger.right = {...this.currentBurger.right, ingredients: []}
      this.currentBurger.steamed = false;
    },
    clearScreen() {
      this.currentBurger.id = "";
      this.currentBurger.name = "";
      this.currentBurger.ingredients = [];
      this.currentBurger.steamed = false;
    },
    toggleSteam() {
      this.currentBurger.steamed = !this.currentBurger.steamed;
    },
    togglePreview() {
      this.preview = !this.preview;
    },
    leftUpate(updatedIngredients) {
      this.currentBurger.left.ingredients = updatedIngredients
    },
    rightUpdate(updatedIngredients) {
      this.currentBurger.right.ingredients = updatedIngredients
    },
    next() {
      const orderIndex = this.orders.findIndex(ord => ord.id == this.activeOrder);
      if(orderIndex < (this.orders.length - 1)) {
        this.activeOrder = this.orders[orderIndex + 1].id
      }
    },
    prev() {
      const orderIndex = this.orders.findIndex(ord => ord.id == this.activeOrder);
      if(orderIndex > 0) {
        this.activeOrder = this.orders[orderIndex - 1].id
      }
    },
    clear() {
      this.activeOrder = ""
      this.orders = [];
    },
    toggleAuto() {
      if(this.auto) {
        this.auto = false;
        clearTimeout(this.autoInterval);
        return;
      }
      this.auto = true;
      this.ontimeout()
    },
    ontimeout() {
      if(this.auto){
          this.generateOrder();
          this.autoInterval = setTimeout(this.ontimeout, Math.random() * 30000);
        }
    },
    newOrderPlay() {
      this.audio.play()
    },
    makeActive(orderIndex) {
      this.activeOrder = this.orders[orderIndex].id
    },
    addFilter(filter) {
      if(this.filters.indexOf(filter) !== -1) {
        this.filters = this.filters.filter(f => f !== filter);
        return;
      }
      this.filters.push(filter)
    }
  },
  computed: {
    previewBurger() {
      const burger = this.burgers.find(b => b.id === this.getActiveOrder.order.id);
      if(burger) {
        return burger.ingredients.map(b => {
          const ingd = this.ingredients.find(ing => ing.id == b);
          if(ingd) {
            return ingd.name
          }
        })
      }else {
        return []
      }
      
    },
    filteredBurgers() {
      if(this.filters.length == 0) {
        return this.burgers
      }
      return this.burgers.filter(bg => this.filters.indexOf(bg.category) !== -1)
    },
    getActiveOrder() {
      const activeOrder = this.orders.find(ord => ord.id == this.activeOrder)
      if(activeOrder) {
        return activeOrder
      }else {
        return {
          id: '',
          order: {
            id: '',
            name: ''
          }
        }
      }
    },
    others() {
      return this.ingredients.filter(ing => ing.category == "others")
    },
    pfc() {
      return this.ingredients.filter(ing => ing.category == "kfc")
    },
    sauces() {
      return this.ingredients.filter(ing => ing.category == "sauce")
    },
    buns() {
      return this.ingredients.filter(ing => ing.category == "bun")
    },
    categories() {
      const categories = [];
      const all_categories = this.burgers.map(bg => bg.category);
      all_categories.forEach(bg => {
        if (categories.indexOf(bg) == -1) {
          categories.push(bg)
        }
      })
      return categories
    }
  },
  beforeDestroy() {
    clearInterval(this.autoInterval)
  }
}
</script>

<style >
#app {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  align-items: flex-end;
  padding-top: 30px;

}





.c-monitor {
  width: 30%;
}



.c-results {
  width: 70%;
  position: absolute;
  right: 0;
  top: 0;
}

.o-makeline {
  width: 70%;
}


.c-ingredients {
  margin-top: 30px;
  width: 100%;
}

body {
  margin: 0;
}

.c-burger {
  width: 200px;
  height: 200px;
  background: grey;
}

.success {
  font-size: 20px;
  color: green;
}

.failed {
  font-size: 20px;
  color: red;
}

.right {
  display: flex;
  justify-content: space-around;
  width: 55%;
}

.left {
  width:30%;
}



.centre {
  width: 25%;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.no-order {
  color: white;
  width: 100%;
  text-align: center;
  padding: 12px;
}

.buttons {
  width: 100%;
/*   display: flex; */
/*   justify-content: center;
  flex-wrap: wrap; */
}

.button {
  display: block;
  margin: auto;
  width: 200px;
  font-size: 18px;
  height: 40px;
  cursor: pointer;
}

.messages {
  padding: 5px;
  height: 50px;
  width: 280px;
  border-radius: 5px;
  border: 1px solid grey;
  display: flex;
  justify-content: center;
  align-items: center;
}

.serve-btn, .preview-btn {
  width: 100%;
  padding: 12px;
  background: green;
  border: none;
  margin-top: 5px;
  font-size: 20px;
  color: white;
  cursor: pointer;
}

.preview-btn {
  background: purple;
}

.category {
  padding: 12px;
  border: 1px solid grey;
  margin: 4px;
}

.orders {
  box-sizing: border-box;
  width: 100%;
  padding: 12px;
  height: 200px;
  background: black;
  position: relative;
}

.order {
  background: white;
  height: 100px;
  min-width: 180px;
  border-radius: 5px;
  padding: 5px;
  font-size: 25px;
  text-align: center;
  line-height: 90px;
}

.ingredients {
  padding: 2px 5px;
  cursor: pointer;
  font-size: 18px;
}

@media screen and (max-width: 900px) {
  .c-monitor {
    width: 100%;
  }

  .o-makeline {
    width: 100%;
    min-height: 284px;
    display: flex;
    align-items: flex-end;
    position: relative;
  }

}

@media screen and (max-width: 350px) {
  body {
    width: 350px;
  }
}
</style>
