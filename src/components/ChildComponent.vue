<template>
  <div class="child_component">안녕 나는 자식 페이지야
    <h1>{{ msg }}</h1>
    <h2>{{ msgObj.test }}</h2>
    <button @click="childBtnClick(no)">클릭</button>
  </div>
</template>

<script>
export default {
  name: "child_component",
  props: {
    msg: String,
    msgObj: Object
  },
  data: function() {
    return {
      no: 123
    };
  },
  methods: {
    childBtnClick: function(no) {
      this.$emit("emitVar", no);
    }
  }
};
</script>

<style>
</style>

