<template>
    <div class="SJSQL">
        {{changeCompleted}}
        <h1>선택한 테이블 : {{tableName}}</h1>
        <TableList :tableArr="tableArr" @emitTableDesc="passTableDesc" @emitTableData="passTableData" @emitTableName="passTableName" :changeCompleted="changeCompleted" />
        <FieldList :fieldArr="fieldArr" />
        <ButtonList :fieldArr="fieldArr" :dataArr="dataArr" :tableName="tableName" @emitChangeCompleted="passChangeCompleted" />
        <DataList :dataArr="dataArr" :fieldArr="fieldArr" :tableName="tableName" />
        <CustomQuery />
    </div>
</template>

<script>
import TableList from '@/components/TableList'
import FieldList from '@/components/FieldList'
import ButtonList from '@/components/ButtonList'
import DataList from '@/components/DataList'
import CustomQuery from '@/components/CustomQuery'
export default {
    name: 'SJSQL',
    components: {
        TableList,
        FieldList,
        DataList,
        ButtonList,
        CustomQuery
    },
    data: function () {
        return {
            tableArr: [],
            fieldArr: [],
            dataArr: [],
            tableName: '',
            changeCompleted: false
        }
    },
    methods: {
        // FieldList 컴포넌트로 테이블정보 전달
        passTableDesc: function(fieldArr){
            this.fieldArr = fieldArr;
        },
        // DataList 컴포넌트로 행 정보 전달
        passTableData: function(dataArr){
            this.dataArr = dataArr;
        },
        // 테이블 이름을 사용
        passTableName: function(tableName){
            this.tableName = tableName;
        },
        // TableList 컴포넌트로 
        passChangeCompleted: function(){
            this.changeCompleted = !this.changeCompleted;
        },
        // Table List 가져오기
        getTableList: function(){
            window.axios.get("http://localhost:3000/SJSQL/tables", {})
            .then((response) => {
                this.tableArr = response.data;
            })
            .catch(function(err){
                console.log(err);
            });
        }
    },
    mounted: function () {
        this.$nextTick(function () {
            // Code that will run only after the
            // entire view has been rendered
            this.getTableList();
        })
    },
    watch: {
        changeCompleted: function(){
            this.getTableList();
        }
    },
    created: function(){
        // 테이블 드랍된 경우 발생
        this.$EventBus.$on("emitTableDropCompleted", () => {
            // 이벤트 발생시키기
            this.getTableList();
        });
        // 테이블이 생성된 경우 발생
        this.$EventBus.$on("tableCreateCompleted", () => {
            this.getTableList();
        });
    }

}
</script>

<style>
    .SJSQL {
        background-color: #ddd;
        font-size: 10px;
    }

    .SJSQL:after {
        content: '';
        display: block;
        clear: both;
    }

    .SJSQL >div {
        padding: 10px;
        float: left;
        min-height: 200px;
    }
</style>


