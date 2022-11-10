<template>
  <button class="btn" v-bind:style="{background:color}" @click="onClick()">
    {{text}}
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text:String,
    color:String,
    },
  methods: {
    onClick() {
        //console.log('click');
        this.$emit('btn-click');
    }
    },
}  
</script>