<template>
  <div class="colors">
    <button class="color" :class="getClassName(color)" v-for="(color, index) in colors" :key="index" @click="setColor(color)" :style="{ backgroundColor: color.color }"></button>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  data: () => ({
  }),
  methods: {
    initWith() {
      return {
        plugin: 'colors',
        currentColor: null
      }
    },
    setColor(color) {
      if(!this.model.currentColor) {
        this.model.currentColor = color;
        return;
      }
      this.model.currentColor = color.color == this.model.currentColor.color ? null : color;
    },
    getClassName(color) {
      let cn = ['color'];
      if(this.model.currentColor && this.model.currentColor.color === color.color) {
        cn.push('color--active')
      }
      return cn;
    },
    pluginCreated() {
      let listWithColors = [];
      for(let name in this.options) {
        listWithColors.push({
          color: this.options[name],
          className: name
        })
      }

      this.colors = listWithColors;
    },
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    },
  }
}
</script>

<style>
  .colors {
    display: flex;
    align-items: center;
  }
  .color {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    border: none;
    cursor: pointer;
    outline: 0;
    margin: 0 5px 5px 0;
  }
  .color--active {
    border: 2px solid #000;
  }
</style>
