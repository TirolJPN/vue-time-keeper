<template>
    <div>
        <hr>
        <h1>
            title
        </h1>
        <h2>{{clientId}}</h2>
        <h2>{{apiKey}}</h2>
        <button @click="clientInitialize">alert</button>
    </div>
</template>

<script>
export default {
    name: 'displayInfoOfSpreadSheet',
    data () {
        return {
            clientId: process.env.VUE_APP_CLIENT_ID,
            apiKey: process.env.VUE_APP_API_KEY,

        };
    },
    mounted: function() {
        // for debug
        console.log(this.clientId)
        console.log(this.apiKey)
    },
    methods: {
        clientInitialize() {
            window.gapi.load('client:auth2', ()=> {
                gapi.client.init({
                    clientId: this.clientId,
                    scope: 'https://www.googleapis.com/auth/spreadsheets',
                    discoveryDocs: ['https://sheets.googleapis.com/$discovery/rest?version=v4']
                });
                const auth = window.gapi.auth2.getAuthInstance();
                auth.signIn();
            });
        }
    }
}
</script>
