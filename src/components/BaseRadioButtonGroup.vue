<template>
  <div class="radio-button-group">
    <div class="radio-button-item" v-for="item in items" :key="item.id">
      <input
        type="radio"
        :id="item.id"
        :name="name"
        :value="item.id"
        :disabled="isDisabled(item.id)"
        @change="onSelectedItemChange($event)"
      >
      <label :for="item.id">{{ item.value }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BaseRadioButtonGroup',
  props: {
    name: {
      type: String,
      required: true,
    },
    items: {
      type: Array,
      required: true
    },
    disabled: {
      type: Boolean,
      default() {
        return false
      }
    },
    disabledItems: {
      type: Array,
      default() {
        return []
      }
    },
  },
  methods: {
    onSelectedItemChange(event) {
      this.$emit('change', event)
    },
    isDisabled(itemId) {
      return this.disabled ||
        this.disabledItems.includes(parseInt(itemId)) ? true : false
    },
  }
}
</script>

<style scoped>
  .radio-button-group {
    border:1px solid gainsboro;
    margin: 10px;
    padding: 10px;
    text-align: left
  }
  .radio-button-item {
    margin: 5px;
  }
</style>
