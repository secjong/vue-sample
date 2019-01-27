<template>
    <div class="custom_query">
        <h2>Custom Query</h2>
        <div class="custom_query_area">
            <b-form-input v-model="query" type="text" placeholder="Enter some query"></b-form-input>
            <b-button-group size="sm">
                <b-button variant="primary">Run Query</b-button>
                <b-button variant="danger">Clear Result</b-button>
            </b-button-group>
            <div>
                <h3>Query Result</h3>
                <div>

                </div>
            </div>
            <div>
                <h3>Row Result</h3>
                <div>

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
            query: ""
        }
    },
    methods: {
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


