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
                    <span v-for="(value, index) in dataRow" :key="index" class="data_span">
                        {{value}}
                    </span>
                    <span>
                        <button @click="editRow(dataRow)">edit</button>
                        <button @click="removeRow(dataRow[priKey])">X</button>
                    </span>
                </li>

            </ul>
        </div>

        <!-- edit data modal -->
        <b-modal v-model="show_edit"
                title="Edit Data"
                :header-bg-variant="headerBgVariant"
                :header-text-variant="headerTextVariant"
                :body-bg-variant="bodyBgVariant"
                :body-text-variant="bodyTextVariant"
                :footer-bg-variant="footerBgVariant"
                :footer-text-variant="footerTextVariant">
            <b-container fluid>
                <ul>
                    <!-- 반복되는 부분(컬럼) -->
                    <li v-for="(item, index) in fieldArr" :key="index">
                        <b-row class="mb-1 text-center">
                            <!-- <span v-for="(value, key) in item" :key="key">
                                <span v-if="key === 'Field'">{{value}}</span>
                                <span v-if="key === 'Type'">({{value}})</span>
                                <b-form-input v-if="key === 'Field'" v-model="obj[value]" type="text"></b-form-input>
                            </span> -->
                            <label>
                                <span>{{item.Field}}({{item.Type}})</span> - 
                                <span v-if="item.Null">null : {{item.Null}}</span>
                                <span v-if="item.Key"> / {{item.Key}}</span>
                                <span v-if="item.Default"> / {{item.Default}}</span>
                                <span v-if="item.Extra"> / {{item.Extra}}</span>
                            </label>
                            <b-form-input v-model="obj[item.Field]" type="text" v-if="item.Field === priKey" disabled="disabled"></b-form-input>
                            <b-form-input v-model="obj[item.Field]" type="text" v-else></b-form-input>
                        </b-row>
                    </li>
                    <!--// 반복되는 부분(컬럼) -->
                </ul>

                {{tableName}}
                {{obj}}

                <b-button-group size="sm">
                    <b-button @click="submitData" variant="primary">Submit</b-button>
                    <b-button @click="reset" variant="danger">Reset</b-button>
                </b-button-group>

            </b-container>
            <div slot="modal-footer" class="w-100">
                <p class="float-left">Modal Footer Content</p>
                <b-btn size="sm" class="float-right" variant="primary" @click="show_edit=false">
                Close
                </b-btn>
            </div>
        </b-modal>
        <!--// edit data modal -->

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
            priKey : '',
            show_edit : false,
            headerBgVariant: 'dark',
            headerTextVariant: 'light',
            bodyBgVariant: 'light',
            bodyTextVariant: 'dark',
            footerBgVariant: 'warning',
            footerTextVariant: 'dark',
            obj: {}
        }
    },
    methods: {
        // 데이터 행 삭제
        removeRow: function(pk){
            var flag = confirm("정말 삭제하시겠습니까?");
            if(!flag){
                return;
            }
            
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
        },
        // 데이터 행 편집
        editRow: function(dataRow){
            // 행 데이터를 모달용 데이터로 저장하기
            this.obj = dataRow;

            // 모달창 띄우기
            this.show_edit = true;
        },
        // 데이터 수정 서브밋
        submitData: function(){

            var data = {
                params : this.obj,
                priKey : this.priKey
            };

            window.axios.put("http://localhost:3000/SJSQL/tables/" + this.tableName + "/" + this.obj[this.priKey], data)
            .then((response) => {
                console.log(response);
                if(response.data.affectedRows === 1){
                    alert("성공적으로 수정되었습니다.");
                    this.$EventBus.$emit("dataUpdateCompleted");
                    this.show_edit = false;
                }
            })
            .catch(function(err){
                console.log(err);
            });

        },
        reset: function(){

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


