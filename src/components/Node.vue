<template>
  <div class="node" :class="this.computeColor(this.num)">
    {{ !!num ? num : '' }}
  </div>
</template>

<script>

export default {
  name: 'Node',
  props: ['number'],
  data () {
    return {
      num: 0
    }
  },

  mounted() {
    this.num = this.number;
  },

  watch: {
    number() {
      this.num = this.number;
    }
  },

  methods: {
    computeColor(num) {
      switch (num) {
        case 2:
          return 'color2';
          break;
        case 4:
          return 'color4';
          break;
        case 8:
          return 'color8';
          break;
        case 16:
          return 'color16';
          break;
        case 32:
          return 'color32';
          break;
        case 64:
          return 'color64';
          break;
        case 128:
          return 'color128';
          break;
        case 256:
          return 'color256';
          break;
        case 512:
          return 'color512';
          break;
        case 1024:
          return 'color1024';
          break;
        case 2048:
          return 'color2048';
          break;
      }
    }
  }

}
</script>

<style scoped>
  .node {
    flex: 1 1 0%;
    background: white;
    border-radius: 8px;
    margin: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: "microsoft yahei";
    font-size: 30px;
    font-weight: bold;
    color: #666;
  }

  .color2 {
    background-color: #FAE1BC;
  }

  .color4 {
    background-color: #CAF0F0;
  }

  .color8 {
    background-color: #75E7C1;
  }

  .color16 {
    background-color: #F08484;
  }

  .color32 {
    background-color: #B5F0B5;
  }

  .color64 {
    background-color: #B6D2F6;
  }

  .color128 {
    background-color: #FFCF7E;
  }

  .color256 {
    background-color: #FAD8D8;
  }

  .color512 {
    background-color: #7CB7E7;
  }

  .color1024 {
    background-color: #E1DBD7;
  }

  .color2048 {
    background-color: #C878C7;
  }
</style>
