<template>
    <div class="button_list">
        <b-button-group size="sm">
            <b-button v-for="btn in buttons" @click="btn.event" :variant="btn.variant" :key="btn.variant">
                {{ btn.caption }}
            </b-button>
        </b-button-group>

        <!-- create table modal -->
        <b-modal v-model="show_create"
                title="Create Table"
                :header-bg-variant="headerBgVariant"
                :header-text-variant="headerTextVariant"
                :body-bg-variant="bodyBgVariant"
                :body-text-variant="bodyTextVariant"
                :footer-bg-variant="footerBgVariant"
                :footer-text-variant="footerTextVariant">
            <b-container fluid>
                <b-row class="mb-1 text-center">
                    <label>Table name : </label>
                    <b-form-input v-model="newTableName" type="text" placeholder="Enter Table Name"></b-form-input>
                </b-row>
                
                <ul>
                    <!-- 반복되는 부분(컬럼) -->
                    <li v-for="(item, index) in params" :key="item.id">
                        <b-row class="mb-1 text-center">
                            <b-col cols="4">
                                <label>Column name</label>
                                <b-form-input v-model="params[index].colName" type="text"></b-form-input>
                            </b-col>
                            <b-col cols="4">
                                <label>Type</label>
                                <b-form-select :options="variants" v-model="params[index].type" />
                            </b-col>
                            <b-col cols="4">
                                <label>Size</label>
                                <b-form-input v-model="params[index].size" type="number"></b-form-input>
                            </b-col>
                        </b-row>
                        <b-row>
                            <b-form-group>
                                <b-form-checkbox-group v-model="params[index].constraints" :options="options">
                                </b-form-checkbox-group>
                            </b-form-group>
                            <b-button size="sm" variant="danger" @click="removeRow(index)">remove row</b-button>
                        </b-row>
                    </li>
                    <!--// 반복되는 부분(컬럼) -->
                </ul>

                {{newTableName}}
                <br />
                {{params}}
               
                <b-button-group size="sm">
                    <b-button @click="addRow" variant="success">Add Row</b-button>
                    <b-button @click="submitTable" variant="primary">Submit</b-button>
                    <b-button @click="resetTable" variant="danger">Reset</b-button>
                </b-button-group>

            </b-container>
            <div slot="modal-footer" class="w-100">
                <p class="float-left">Modal Footer Content</p>
                <b-btn size="sm" class="float-right" variant="primary" @click="show_create=false">
                Close
                </b-btn>
            </div>
        </b-modal>
        <!--// create table modal -->

        <!-- add data modal -->
        <b-modal v-model="show_add"
                title="Add Data"
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
                            <b-form-input v-model="obj[item.Field]" type="text" v-if="item.Key === 'PRI'" disabled="disabled"></b-form-input>
                            <b-form-input v-model="obj[item.Field]" type="text" v-else></b-form-input>
                        </b-row>
                    </li>
                    <!--// 반복되는 부분(컬럼) -->
                </ul>

                <!-- {{tableName}} -->
                {{obj}}

                <b-button-group size="sm">
                    <b-button @click="submitData" variant="primary">Submit</b-button>
                    <b-button @click="resetData" variant="danger">Reset</b-button>
                </b-button-group>

            </b-container>
            <div slot="modal-footer" class="w-100">
                <p class="float-left">Modal Footer Content</p>
                <b-btn size="sm" class="float-right" variant="primary" @click="show_add=false">
                Close
                </b-btn>
            </div>
        </b-modal>
        <!--// add data modal -->

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
            buttons: [
                { variant: 'primary', caption: 'create', event: this.create },
                { variant: 'danger', caption: 'truncate', event: this.truncate },
                { variant: 'warning', caption: 'edit', event: this.edit },
                { variant: 'secondary', caption: 'drop', event: this.drop },
                { variant: 'success', caption: 'add', event: this.add },
            ],
            variants: [
                'INT', 'DOUBLE', 'CHAR', 'VARCHAR', 'DATE'
            ],
            show_create: false,
            show_add: false,
            headerBgVariant: 'dark',
            headerTextVariant: 'light',
            bodyBgVariant: 'light',
            bodyTextVariant: 'dark',
            footerBgVariant: 'warning',
            footerTextVariant: 'dark',
            options: [
                {text: 'primary key', value: 'primary key'},
                {text: 'not null', value: 'not null'},
                {text: 'auto_increment', value: 'auto_increment'},
            ],
            newTableName: '',
            params: [],
            obj: {}
        }
    },
    methods: {
        create: function(){
            this.show_create = true;
        },
        truncate: function(){
            if(this.tableName === ''){
                alert("잘라낼 테이블을 먼저 선택해주세요.");
                return;
            }
            var flag = confirm("정말 잘라내시겠습니까?");
            if(!flag){
                return;
            }

            window.axios.put("http://localhost:3000/SJSQL/tables/" + this.tableName, {})
            .then((response) => {
                console.log(response);
                // 성공인 경우
                alert(this.tableName + " 테이블을 잘라냈습니다.");
                this.$emit("emitChangeCompleted", response.data);
            })
            .catch(function(err){
                alert("테이블 잘라내기에 실패했습니다.");
                console.log(err);
            });
        },
        edit: function(){
            alert("준비중인 기능입니다.");
        },
        drop: function(){
            if(this.tableName === ''){
                alert("삭제할 테이블을 먼저 선택해주세요.");
                return;
            }
            var flag = confirm("정말 삭제하시겠습니까?");
            if(!flag){
                return;
            }
            
            window.axios.delete("http://localhost:3000/SJSQL/tables/" + this.tableName, {})
            .then((response) => {
                console.log(response);
                // 성공인 경우
                alert(this.tableName + " 테이블을 드랍했습니다.");
                location.reload();
            })
            .catch(function(err){
                alert("테이블 드랍에 실패했습니다.");
                console.log(err);
            });
        },
        add: function(){
            if(this.tableName === ''){
                alert("데이터를 추가할 테이블을 먼저 선택해주세요.");
                return;
            }
            var obj = {};
            for(var i = 0; i < this.fieldArr.length; i+=1){
                var key = this.fieldArr[i].Field;
                obj[key] = '';
            }
            this.obj = obj;
            this.show_add = true;
        },


        // 테이블 생성시 필드 추가
        addRow: function(){
            this.params.push({
                colName: undefined,
                type: undefined,
                size: undefined,
                constraints: undefined,
            });
        },
        // 테이블 생성
        submitTable: function(){
            var data = {
                params : this.params,
                tableName : this.newTableName
            };
            window.axios.post("http://localhost:3000/SJSQL/tables", data)
            .then((response) => {
                console.log(response);
                // 성공인 경우
                alert(this.newTableName + " 테이블 생성에 성공했습니다.");
                location.reload();
            })
            .catch(function(err){
                alert("테이블 생성에 실패했습니다.");
                console.log(err);
            });
        },
        // 테이블 생성 폼 리셋
        resetTable: function(){
            this.newTableName = '';
            this.params = [];
        },
        // 테이블 데이터 삭제시 삭제된 행 삭제
        deleteRow(index) {
            this.inputs.splice(index,1);
        },
        // 데이터 추가
        submitData: function(){
            console.log("this.obj : " , this.obj);
            var data = {
                params : this.obj
            };
            console.log("this.tableName : ", this.tableName);
            window.axios.post("http://localhost:3000/SJSQL/tables/" + this.tableName, data)
            .then((response) => {
                console.log(response);
                // 성공인 경우
                alert("데이터 삽입에 성공했습니다.");
                location.reload();
            })
            .catch(function(err){
                alert("데이터 삽입에 실패했습니다.");
                console.log(err);
            });
        },
        // 데이터 추가 폼 리셋
        resetData : function(){
            // obj 의 모든 value를 지우기
            for(var key in this.obj){
                this.obj[key] = '';
            }
        },
        // 테이블 생성 행 삭제
        removeRow: function(index){
            this.params.splice(index, 1);
        }
    }
}
</script>

<style>
.button_list {
    position: absolute;
    top: 10px;
    left: 10px;
}

.data_list {
    width: 50%;
    box-sizing: border-box;
}
</style>





{
    tableName = "aaa",
    params = [
        {colname: "col1", type: "int", size: "10", constraints: ["primary key", "not null", "auto_increment"]},
        {colname: "col1", type: "int", size: "10"}
    ]
}
