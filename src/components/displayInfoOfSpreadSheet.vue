<template>
    <div>
        <hr>
        <h1>
            title
        </h1>
        <p>{{'client id: ' + clientId}}</p>
        <p>{{'api key: ' + apiKey}}</p>
        <p>{{'is logined: ' + isLogined }}</p>
        <button @click="clientInitialize">login</button>
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
            isLogined: false,
        };
    },
    mounted: function() {
        // for debug
        console.log(this.clientId)
        console.log(this.apiKey)
    },
    methods: {
        clientInitialize() {
            const self = this;
             window.gapi.load('client:auth2', ()=> {
                // authの初期化
                gapi.client.init({
                    clientId: self.clientId,
                    scope: 'https://www.googleapis.com/auth/spreadsheets',
                    discoveryDocs: ['https://sheets.googleapis.com/$discovery/rest?version=v4']
                }).then(function () {
                    // GoogleAuthオブジェクトを取得
                    // ref: https://developers.google.com/identity/sign-in/web/reference
                    const auth = window.gapi.auth2.getAuthInstance();
                    // ポップアップが閉じられると同時に状態を変更
                    auth.signIn().then( function () {
                        self.isLogined = auth.isSignedIn.get();
                        console.log('is logined?: ' + self.isLogined)
                    });
                });
    
            });
        },

        fetchSpreadSheetData() {
            // シートデータの取得
            window.gapi.client.sheets.spreadsheets.values.get({
                spreadsheetId: self.sheetId,
                range: 'time-keeper!A1:F13'
            }).then((res)=>{
                console.log(res)
            });
        }
    }
}
</script>
