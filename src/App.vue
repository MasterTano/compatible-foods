<template>
  <div id="app">
    <form @submit.prevent="submitForm" class="select-food-form">
      <template v-for="(item, index) in items">
        <base-radio-button-group
          :key="index"
          :name="getRadioGroupName(index)"
          :items="item"
          :disabled="disabledRadioGroups[index]"
          :disabledItems="incompatibleFoods"
          @change="setSelectedItem(index, $event.target.value)"
        ></base-radio-button-group>
      </template>
      <input type="submit" value="Submit" :disabled="disableButton">
    </form>
  </div>
</template>

<script>
import BaseRadioButtonGroup from './components/BaseRadioButtonGroup.vue'

export default {
  name: 'App',
  components: {
    BaseRadioButtonGroup,
  },
  data() {
    return {
      selectedItems: [],
      incompatibleFoods: [],
      disabledRadioGroups: [],
      items: [
        // first group of radio-buttons
        [
          { id: '101', value: 'Vegetarian' },
          { id: '102', value: 'Nut allergy' },
          { id: '103', value: 'Halal' }
        ],
        // second group of radio-buttons
        [
          { id: '201', value: 'Cashew chicken' },
          { id: '202', value: 'Sweet and sour pork' },
          { id: '203', value: 'Stir fried Tofu' },
          { id: '204', value: 'Vegetable fried rice' },
          { id: '205', value: 'Pad Thai' },
          { id: '206', value: 'Massaman beef' },
        ],
        // third group of radio-buttons
        [
          { id: '301', value: 'Peanut sauce' },
          { id: '302', value: 'Oyster sauce' },
          { id: '303', value: 'Vegetable spring rolls' },
          { id: '304', value: 'Steamed rice' },
        ],
      ],
      foodIncompatability: {
        // 'Vegetarian' is NOT compatible with 'Cashew chicken', 'Sweet and sour pork', 'Massaman beef', 'Oyster sauce'
        101: [201, 202, 206, 302],
        // 'Nut allergy' is NOT compatible with 'Cashew chicken', 'Peanut sauce',
        102: [201, 301],
        // 'Halal' is NOT compatible with 'Sweet and sour pork',
        103: [202], 
        // 'Vegetable fried rice' is NOT compatible with 'Steamed rice' (you don't need more rice... carb overload),
        204: [304],
        // 'Pad thai' is NOT compatible with 'Steamed rice' (Pad thai comes with noodles),
        205: [304],
      }
    }
  },
  computed: {
    disableButton() {
      return this.ifASelectedItemIsInvalid() || this.selectedItems.length !== this.items.length
    },
  },
  mounted() {
    this.initDisabledRadioGroups()
  },
  methods: {
    initDisabledRadioGroups() {
      this.items.forEach(() => this.disabledRadioGroups.push(true))
      this.disabledRadioGroups[0] = false
    },
    ifASelectedItemIsInvalid() {
      let invalidItems = this.selectedItems.filter(item => this.isSelectedItemInvalid(item))
      return (invalidItems.length) ? true : false
    },
    isSelectedItemInvalid(item) {
      return item === undefined || this.incompatibleFoods.includes(parseInt(item))
    },
    setSelectedItem(index, value) {
      this.$set(this.selectedItems, index, value)
      this.setDisabledRadioGroups(index+1, false)
      this.setIncompatibleFoods()
    },
    setDisabledRadioGroups(index, value) {
      this.$set(this.disabledRadioGroups, index, value)
    },
    setIncompatibleFoods() {
      if(!this.selectedItems.length) {
        return
      }
      this.incompatibleFoods.splice()
      this.incompatibleFoods = this.getIncompatibleFoods()
    },
    getRadioGroupName(index) {
      return `group-${index}`
    },
    getIncompatibleFoods() {
      let incompatibleFoods = []
      this.selectedItems.forEach((foodId) => {
        incompatibleFoods.push(this.getIncompatibleFoodsFor(parseInt(foodId)))
      })
      return incompatibleFoods.flat()
    },
    getIncompatibleFoodsFor(id) {
      if(!id || !this.foodIncompatability.hasOwnProperty(id)) {
        return []
      }
      return this.foodIncompatability[id]
    },
    submitForm() {
      if(window.confirm('Form submitted successfully! Hire me.=)')) {
        alert("Thank you so much! =)")
        window.location.href="https://www.linkedin.com/in/cristian-cardi%C3%B1o/"
        return
      }
      alert("=(")
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.select-food-form {
    width: 300px;
	height: 450px;
	position: absolute;
	top:0;
	bottom: 0;
	left: 0;
	right: 0;
	margin: auto;
}
</style>
