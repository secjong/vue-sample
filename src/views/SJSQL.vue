<template>
    <div class="SJSQL">
        안녕 나는 sj sql 부모페이지야.
        <TableList :tableArr="tableArr" />
    </div>
</template>

<script>
import TableList from '@/components/TableList'
export default {
    name: 'SJSQL',
    components: {
        TableList
    },
    data: function () {
        return {
            tableArr: []
        }
    },
    methods: {
        parentFunc: function (no) {
            alert("이벤트 올라옴!" + no);
        }
    },
    mounted: function () {
        this.$nextTick(function () {
            // Code that will run only after the
            // entire view has been rendered
            // 모든 테이블 가져오기
            axios.get("http://localhost:3000/SJSQL/tables", {})
            .then((response) => {
                console.log(response);
                this.tableArr = response.data;
            })
            .catch(function(err){
                console.log(err);
            });
        })
    }
}
</script>

<style>
    
</style>


