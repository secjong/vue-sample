<template>
    <div class="custom_query">
        <h2>Custom Query</h2>
        <div class="custom_query_area">
            <b-form-input v-model="query" type="text" placeholder="Enter some query" ref="queryInput"></b-form-input>
            <b-button-group size="sm">
                <b-button variant="primary" @click="runQuery">Run Query</b-button>
                <b-button variant="danger" @click="clearResult">Clear Result</b-button>
            </b-button-group>
            <div>
                <h3>Query Result</h3>
                <div>
                    <b-table striped hover :items="resultArr"></b-table>
                </div>
            </div>
            <div>
                <h3>Row Result</h3>
                <div>
                    {{resultArr}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'custom_query',
    props: {
    },
    data: function () {
        return {
            query: "",
            resultArr: []
        }
    },
    methods: {
        // 커스텀 쿼리 실행
        runQuery: function(){
            if(this.query.trim() === ''){
                alert("쿼리를 먼저 입력해주세요.");
                this.query = '';
                this.$refs.queryInput.focus();
                return;
            }

            var data = {
                query: this.query
            };

            window.axios.post("http://localhost:3000/SJSQL", data)
            .then((response) => {
                if(response.status === 200){
                    this.resultArr = response.data;
                }
            })
            .catch(function(err){
                alert("올바른 쿼리가 아닙니다.");
                console.log(err);
            });

        },
        clearResult: function(){
            this.resultArr = [];
        }
    }
}
</script>

<style>
    .custom_query {
        width: 50%;
        box-sizing: border-box;
    }

    .custom_query_area {
        border: 1px solid #fff;
    }

</style>


