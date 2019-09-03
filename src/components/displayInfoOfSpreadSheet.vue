<template>
    <div>
        <hr>
        <h1>
            title
        </h1>
        <p>{{'client id: ' + clientId}}</p>
        <p>{{'api key: ' + apiKey}}</p>
        <p>{{'is logined: ' + isLogined }}</p>
        <p>{{'data: ' + JSON.stringify(data)}}</p>
        <button @click="clientInitialize" v-if="!isLogined">login</button>
        <button @click="logOut" v-if="isLogined">logout</button>
        <button @click="fetchSpreadSheetData">fetch</button>
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
            data: [],
        };
    },
    methods: {
        clientInitialize() {
            const self = this;
             window.gapi.load('client:auth2', ()=> {
                // authの初期化
                window.gapi.client.init({
                    clientId: self.clientId,
                    scope: 'https://www.googleapis.com/auth/spreadsheets',
                    discoveryDocs: ['https://sheets.googleapis.com/$discovery/rest?version=v4']
                }).then(function () {
                    // GoogleAuthオブジェクトを取得
                    // ref: https://developers.google.com/identity/sign-in/web/reference
                    const auth = window.gapi.auth2.getAuthInstance();
                    // ポップアップが閉じられると同時に状態を変更
                    auth.signIn().then( () => 
                        self.isLogined = auth.isSignedIn.get()
                    )
                });
            });
        },

        logOut () {
            const self = this;
            const auth = window.gapi.auth2.getAuthInstance();
            // 既にログイン状態ならば
            if(auth.isSignedIn.get()){
                auth.signOut().then( function() {
                    auth.disconnect();
                    self.isLogined = auth.isSignedIn.get();
                })
            }
        },

        fetchSpreadSheetData() {
            // シートデータの取得
            window.gapi.client.sheets.spreadsheets.values.get({
                spreadsheetId: this.sheetId,
                range: 'time-keeper!A1:F13'
            }).then((res)=>{
                this.data = res.result.values;
            });
        }
    }
}
</script>
