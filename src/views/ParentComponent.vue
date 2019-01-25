<template>
    <div class="parent_component">
        안녕 나는 부모 페이지야
        <ChildComponent msg="나는 전달되는 메시지야!" :msgObj="msgObj" @emitVar="parentFunc"/>
        <b-table :items="test_arr"></b-table>
    </div>
</template>

<script>
import ChildComponent from '@/components/ChildComponent'
export default {
  name: 'parent_component',
  components: {
    ChildComponent
  },
  data: function(){
      return {
          msgObj: {
              test: "test"
          },
          test_arr: [
                {name: "sam", age: 22, location: "seoul"},
                {name: "jack", age: 33, location: "busan"},
                {name: "janne", age: 32, location: "incheon"}
          ]
      }
  },
  methods: {
      parentFunc: function(no){
          alert("이벤트 올라옴!" + no);
      }
  }
}
</script>

<style>
    
</style>


