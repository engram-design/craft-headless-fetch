<template>
    <form ref="form" @submit.prevent="submitForm">
        <input type="hidden" name="action" value="entries/save-entry">
        <input type="hidden" name="sectionId" value="1">

        Title
        <input type="text" name="title" value="Some Title">

        <button type="submit">Submit</button>
    </form>
</template>

<script>


export default {
    methods: {
        async submitForm() {
            const baseUrl = 'http://craft.test/';
            const formData = new FormData(this.$refs.form);

            // Fetch a fresh CSRF token
            fetch(baseUrl + 'actions/users/session-info', {
                method: 'get',
                headers: {
                    'Accept': 'application/json',
                    'X-Requested-With': 'XMLHttpRequest',
                },
            }).then((response) => {
                console.log(response);

                response.json().then((data) => {
                    fetch(baseUrl, {
                        method: 'post',
                        body: formData,
                        headers: {
                            'Accept': 'application/json',
                            'X-Requested-With': 'XMLHttpRequest',
                            'X-CSRF-Token': data.csrfTokenValue,
                        },
                    }).then((response) => {
                        console.log(response);
                    });
                });
            });
        },
    },
};

</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
