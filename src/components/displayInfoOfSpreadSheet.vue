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

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';


@Component
export default class displayInfoOfSpreadSheet extends Vue {
    /** data **/
    clientId: string = process.env.VUE_APP_CLIENT_ID;
    apiKey: string = process.env.VUE_APP_API_KEY;
    sheetId: string = process.env.VUE_APP_SHEET_ID;
    isLogined: boolean =  false;
    data: Array<Array<any>> = [];

    /** methods **/
    clientInitialize() {
        const self = this;
            (<any>window).gapi.load('client:auth2', ()=> {
            // authの初期化
            (<any>window).gapi.client.init({
                clientId: self.clientId,
                scope: 'https://www.googleapis.com/auth/spreadsheets',
                discoveryDocs: ['https://sheets.googleapis.com/$discovery/rest?version=v4']
            }).then(function () {
                // GoogleAuthオブジェクトを取得
                // ref: https://developers.google.com/identity/sign-in/web/reference
                const auth = (<any>window).gapi.auth2.getAuthInstance();
                // ポップアップが閉じられると同時に状態を変更
                auth.signIn().then( () => 
                    self.isLogined = auth.isSignedIn.get()
                )
            });
        });
    }

    logOut () {
        const self = this;
        const auth = (<any>window).gapi.auth2.getAuthInstance();
        // 既にログイン状態ならば
        if(auth.isSignedIn.get()){
            auth.signOut().then( function() {
                auth.disconnect();
                self.isLogined = auth.isSignedIn.get();
            })
        }
    }

    fetchSpreadSheetData() {
        // シートデータの取得
        (<any>window).gapi.client.sheets.spreadsheets.values.get({
            spreadsheetId: this.sheetId,
            range: 'time-keeper!A1:F13'
        }).then((res: any)=>{
            this.data = res.result.values;
        });
    }
}
</script>
