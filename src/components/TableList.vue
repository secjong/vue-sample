<template>
    <div class="table_list">
        <h2>Show Tables</h2>
        <div class="table_list_area">
            <ul>
                <li v-for="table in tableArr" :key="table.id" @click="getTableInfo(table.Tables_in_mydb)" class="table_li">
                    <span v-if="table.Tables_in_mydb === tableName" class="on">{{table.Tables_in_mydb}}</span>
                    <span v-if="table.Tables_in_mydb !== tableName">{{table.Tables_in_mydb}}</span>
                </li>
            </ul>
        </div>
        <!-- <b-table :items="tableArr"></b-table> -->
        <div class="selected_table_name">
            <span>Selected Table : </span>
            <span>{{tableName}}</span>    
        </div>
    </div>
</template>

<script>
export default {
    name: 'table_list',
    props: {
        tableArr: Array,
        changeCompleted: Boolean
    },
    data: function () {
        return {
            tableName: ""
        }
    },
    methods: {
        getTableInfo: function (tableName) {
            this.tableName = tableName;

            // 테이블의 desc 데이터 받아오기
            window.axios.get("http://localhost:3000/SJSQL/tables/" + tableName + "/desc", {})
            .then((response) => {
                this.$emit("emitTableDesc", response.data);
            })
            .catch(function(err){
                console.log(err);
            });

            // 테이블의 행 데이터 받아오기
            window.axios.get("http://localhost:3000/SJSQL/tables/" + tableName + "/data", {})
            .then((response) => {
                this.$emit("emitTableData", response.data);
            })
            .catch(function(err){
                console.log(err);
            });

            // 테이블 이름 올려주기
            this.$emit("emitTableName", tableName);
        }
    },
    watch: {
        changeCompleted: function(){
            this.getTableInfo(this.tableName);    
        }
    }
}
</script>

<style>
    .table_list {
        width: 50%;
        box-sizing: border-box;
    }

    .table_list_area {
        background-color: #fff;
    }

    .table_li {
        cursor: pointer;
    }

    .table_li >span {
        display: block;
    }

    .table_li >span.on{
        background-color: #333;
        color: #fff;
    }

    .selected_table_name {
        text-align: left;
        font-weight: 800;
    }

    .selected_table_name >span:first-child {
        color: magenta;
    }

    .selected_table_name >span:last-child {
        color: green;
    }
</style>


