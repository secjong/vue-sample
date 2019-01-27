<template>
    <div class="field_list">
        <h2>Describe</h2>
        <div class="field_list_area">
            <ul class="field_list_ul">
                <li>
                    <span>Field</span>
                    <span>Type</span>
                    <span>Null</span>
                    <span>Key</span>
                    <span>Default</span>
                    <span>Extra</span>
                </li>
                <li v-for="field in fieldArr" :key="field.id" class="field_li">
                    <span>{{field.Field}}</span>
                    <span>{{field.Type}}</span>
                    <span>{{field.Null}}</span>
                    <span>{{field.Key}}</span>
                    <span>{{field.Default}}</span>
                    <span>{{field.Extra}}</span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'field_list',
    props: {
        fieldArr: Array
    },
    data: function () {
        return {
        }
    },
    methods: {
    }

}
</script>

<style>
    .field_list {
        width: 50%;
        box-sizing: border-box;
    }

    .field_list_area {
        border: 1px solid #fff;
    }

    .field_li:nth-child(odd) {
        background-color: #fff;
    }

    .field_li:nth-child(even) {
        background-color: #aaa;
    }

    .field_list_ul >li {
        display: flex;
        justify-content: space-around;
    }

    .field_list_ul >li >span {
        border: 1px solid #ddd;
        box-sizing: border-box;
    }

    .field_list_ul >li >span:nth-child(1) {
        width: 18%
    }

    .field_list_ul >li >span:nth-child(2) {
        width: 18%
    }

    .field_list_ul >li >span:nth-child(3) {
        width: 14%
    }

    .field_list_ul >li >span:nth-child(4) {
        width: 14%
    }

    .field_list_ul >li >span:nth-child(5) {
        width: 14%;
    }

    .field_list_ul >li >span:nth-child(6) {
        width: 22%
    }


</style>


