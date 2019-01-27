<template>
    <div class="SJSQL">
        <h1>선택한 테이블 : {{tableName}}</h1>
        <TableList :tableArr="tableArr" @emitTableDesc="passTableDesc" @emitTableData="passTableData" @emitTableName="passTableName" />
        <FieldList :fieldArr="fieldArr" />
        <ButtonList :fieldArr="fieldArr" :dataArr="dataArr" />
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
            tableName: ''
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
        }
    },
    mounted: function () {
        this.$nextTick(function () {
            // Code that will run only after the
            // entire view has been rendered
            // 모든 테이블 가져오기
            window.axios.get("http://localhost:3000/SJSQL/tables", {})
            .then((response) => {
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


