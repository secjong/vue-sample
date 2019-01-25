<template>
    <div class="add">
        안녕 나는 등록 페이지야
        <form method="post" action="/movies/add">
            <input type="text" id="mTitle" name="mTitle" placeholder="영화제목" v-model="mTitle" required="required" />
            <input type="text" id="mDirector" name="mDirector" placeholder="영화감독" v-model="mDirector" required="required" />
            <b-button @click="showInputData">등록</b-button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'add',
    data: function () {
        return {
            mTitle: "",
            mDirector: ""
        }
    },
    methods: {
        showInputData: function () {
            // 빈값이 아니라는 가정 하에
            var data = {
                mTitle: this.mTitle,
                mDirector: this.mDirector
            };
            axios.post("http://localhost:3000/movies/add/ajax", data)
                .then((response) => {
                    console.log(response);
                })
                .catch(function (err) {
                    console.log(err);
                });
        }
    }
}
</script>

<style>
    
</style>


