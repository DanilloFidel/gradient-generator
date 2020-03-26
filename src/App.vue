<template>
  <v-app>
    <v-container fluid style="border: 1px solid red; width: 300px;">
      <v-row dense>
        <v-col cols="5">
            <div v-for="(color,index) in colors" :key="color.id" :class="{shake : color.last}">
            <v-text-field :value="color.hex" @change="changeColor($event, index, color)">
            </v-text-field>
              <button class="up" @click="up(index)" v-if="index>0"></button>
              <button class="down" @click="down(index)" v-if="index<colors.length-1"></button>
              <button @click="lockColor(index)" :class="{locked : color.disabled}"></button>
            </div> 
        </v-col>
        <v-col cols="7">
        <div :style="{background: gradient}" style="height: 85%;">
        </div>
        <div>
        <v-btn icon fab x-small>
          <v-icon>add</v-icon>
        </v-btn>
        </div>
    </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import Vue from "vue";
export default {
  name: 'App',
  data: () => ({
 colors: [
      { id: 0, hex: "#f12711", disabled: false },
      { id: 1, hex: "#f5af19", disabled: false }
    ],
    id: 2
  }),
  computed: {
 gradient() {
      let colors = "linear-gradient(45deg";
      this.colors.forEach(function(e) {
        colors += "," + e.hex;
      });
      colors += ")";
      return colors;
    }
  },
  methods: {
    changeColor(val, idx, oldObj){
      Vue.set(this.colors, idx, Object.assign(oldObj, {hex: val}))
    },
    addColor() {
      this.colors.push({ id: this.id, hex: this.randomHex(), disabled: false });
      this.id++;
    },
    removeColor() {
      if (this.colors[this.colors.length - 1].disabled == false) {
        if (this.colors.length > 2) {
          this.colors.pop();
        }
      }
    },
     generateGradient() {
      for (let i = 0; i < this.colors.length; i++) {
        if (this.colors[i].disabled === false)
          this.colors[i].hex = this.randomHex();
      }
    },
     lockColor(index) {
      this.colors[index].disabled === true
        ? (this.colors[index].disabled = false)
        : (this.colors[index].disabled = true);
    },
    randomHex() {
      return (
        "#" +
        Math.random()
          .toString(16)
          .slice(2, 8)
      );
    },
    up(index) {
      if (index > 0) {
        let temp = this.colors[index];
        this.$set(this.colors, index, this.colors[index - 1]);
        this.$set(this.colors, index - 1, temp);
      }
    },
    down(index) {
      if (index < this.colors.length - 1) {
        let temp = this.colors[index];
        this.$set(this.colors, index, this.colors[index + 1]);
        this.$set(this.colors, index + 1, temp);
      }
    }
  }
}
</script>

<style>
.preview-box{
  height: 86% !important;
}
</style>
