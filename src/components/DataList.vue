<template>
    <div class="data_list">
        <h2>Data</h2>
        <div class="data_list_area">
            <ul class="data_list_ul">

                <li>
                    <span v-for="field in fieldArr" :key="field.id" class="data_label_li">{{field.Field}}</span>
                    <span>&nbsp;</span>
                </li>
                <span v-if="dataArr.length === 0">
                    Table is empty!
                </span>
                <li v-for="dataRow in dataArr" :key="dataRow.id" class="data_li">
                    <span v-for="(value) in dataRow" :key="value.id" class="data_span">
                        {{value}}
                    </span>
                    <span>
                        <button>edit</button>
                        <button @click="removeRow(dataRow[priKey])">X</button>
                    </span>
                </li>

            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'data_list',
    props: {
        dataArr: Array,
        fieldArr: Array,
        tableName: String
    },
    data: function () {
        return {
            priKey : ''
        }
    },
    methods: {
        removeRow: function(pk){
            var that = this;
            var data = {
                priKey : this.priKey
            };

            window.axios.post("http://localhost:3000/SJSQL/tables/" + this.tableName + "/" + pk, data)
            .then((response) => {
                console.log(response);
                if(response.data.affectedRows === 1){
                    alert("성공적으로 삭제되었습니다.");
                    this.dataArr.forEach(function(item, index, thisArr){
                        // 각 obj 에 대해 모든 속성을 돌면서
                        // key 가 priKey 와 일치하고, 값이 pk 와 일치하면 해당 인덱스의 obj 를 제거
                        for(var key in item){
                            if(key === that.priKey && item[key] === pk){
                                thisArr.splice(index, 1);
                                return;
                            }
                        }
                    });
                }
            })
            .catch(function(err){
                console.log(err);
            });
        }
    },
    updated: function(){
        for(var i = 0; i < this.fieldArr.length; i+=1){
            if(this.fieldArr[i].Key === "PRI"){
                // pk가 어떤 필드인지 등록
                this.priKey = this.fieldArr[i].Field;
            }
        }
    }
}
</script>

<style>
    .data_list {
        width: 50%;
        box-sizing: border-box;
    }

    .data_list_area {
        border: 1px solid #fff;
    }

    .data_li:nth-child(odd) {
        background-color: #fff;
    }

    .data_li:nth-child(even) {
        background-color: #aaa;
    }

    .data_list_ul >li {
        display: flex;
        justify-content: space-around;
    }

    .data_list_ul >li >span {
        box-sizing: border-box;
        display: inline-block;
    }

</style>


