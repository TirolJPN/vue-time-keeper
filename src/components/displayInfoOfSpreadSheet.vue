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
            sheetId: process.env.VUE_APP_SHEET_ID,
        };
    },
    mounted: function() {
        // for debug
        console.log(this.clientId)
        console.log(this.apiKey)
    },
    methods: {
        clientInitialize() {
            const self = this
            window.gapi.load('client:auth2', ()=> {
                // authの初期化
                gapi.client.init({
                    clientId: this.clientId,
                    scope: 'https://www.googleapis.com/auth/spreadsheets',
                    discoveryDocs: ['https://sheets.googleapis.com/$discovery/rest?version=v4']
                });
                // GoogleAuthオブジェクトを取得
                // ref: https://developers.google.com/identity/sign-in/web/reference
                const auth = window.gapi.auth2.getAuthInstance();
                auth.signIn();
                console.log('is logined?: ' + auth.isSignedIn.get())

                // シートデータの取得
                gapi.client.sheets.spreadsheets.values.get({
                    spreadsheetId: self.sheetId,
                    range: 'time-keeper!A1:F13'
                }).then((res)=>{
                    console.log(res)
                });
            });
            
        }
    }
}
</script>
