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
                    <b-form-input v-model="tableName" type="text" placeholder="Enter Table Name"></b-form-input>
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
                        </b-row>
                    </li>
                    <!--// 반복되는 부분(컬럼) -->
                </ul>
               
                <b-button-group size="sm">
                    <b-button @click="addRow" variant="success">Add Row</b-button>
                    <b-button @click="submit" variant="primary">Submit</b-button>
                    <b-button @click="reset" variant="danger">Reset</b-button>
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


<!-- { "Field": "mNo", "Type": "int(11)", "Null": "NO", "Key": "PRI", "Default": null, "Extra": "auto_increment" } -->

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
                            <b-form-input v-model="obj[item.Field]" type="text"></b-form-input>
                        </b-row>
                    </li>
                    <!--// 반복되는 부분(컬럼) -->
                </ul>
               
                {{obj}}

                <b-button-group size="sm">
                    <b-button @click="submit" variant="primary">Submit</b-button>
                    <b-button @click="reset" variant="danger">Reset</b-button>
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
        fieldArr: Array
    },
    data: function () {
        return {
            buttons: [
                { variant: 'primary', caption: 'create', event: this.create },
                { variant: 'danger', caption: 'truncate', event: this.truncate },
                { variant: 'warning', caption: 'edit', event: this.edit },
                { variant: 'secondary', caption: 'delete', event: this.delete },
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
            tableName: '',
            params: [],
            obj: {}
        }
    },
    methods: {
        create: function(){
            this.show_create = true;
        },
        truncate: function(){
            this.show_create = true;
        },
        edit: function(){
            this.show_create = true;
        },
        delete: function(){
            this.show_create = true;
        },
        add: function(){
            var obj = {};
            for(var i = 0; i < this.fieldArr.length; i+=1){
                var key = this.fieldArr[i].Field;
                obj[key] = '';
            }
            this.obj = obj;
            this.show_add = true;
        },
        addRow: function(){
            this.params.push({
                colName: undefined,
                type: undefined,
                size: undefined,
                constraints: undefined,
            });
        },
        submit: function(){
            console.log(this.params);
            var data = {
                params : this.params,
                tableName : this.tableName
            };
            window.axios.post("http://localhost:3000/SJSQL/tables", data)
            .then((response) => {
                console.log(response);
                // 성공인 경우
                alert(this.tableName + " 테이블 생성에 성공했습니다.");
                location.reload();
            })
            .catch(function(err){
                console.log(err);
            });
        },
        reset: function(){

        },
        deleteRow(index) {
            this.inputs.splice(index,1);
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


