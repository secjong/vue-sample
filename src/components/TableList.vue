<template>
    <div class="table_list">
        나는 자식 컴포넌트야
        <b-table :items="tableArr"></b-table>
    </div>
</template>

<script>
export default {
  name: 'table_list',
  props: {
      tableArr: Array
  },
  data: function(){
      return {

      }
  },
}
</script>

<style>
    
</style>


