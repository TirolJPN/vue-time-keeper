<template>
  <div>
    <h1>
      {{ this.errorMsg }}
    </h1>
    
    <div  v-if="isLogined">
      <input v-model="sheetId" placeholder="sheet ID">
      <input v-model="sheetName" placeholder="sheet name">

      <button @click="logOut" v-if="isLogined">logout</button>
      <button @click="fetchSpreadSheetData">fetch</button>
    </div>
    <div  v-else>
      <button @click="clientInitialize">login</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';


@Component
export default class displayInfoOfSpreadSheet extends Vue {
  /** data **/
  clientId: string = process.env.VUE_APP_CLIENT_ID;
  apiKey: string = process.env.VUE_APP_API_KEY;
  isLogined: boolean =  false;
  spreadsheetRows: Array<Array<any>> = [];

  /** model **/
  sheetId: string = '';
  sheetName: string = ''; 
  errorMsg: string = '';

  mounted () {
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

  /** methods **/
  clientInitialize(): void {
    const self = this;
      (<any>window).gapi.load('client:auth2', ()=> {
      // authの初期化
      (<any>window).gapi.client.init({
        clientId: self.clientId,
        scope: 'https://www.googleapis.com/auth/spreadsheets',
        discoveryDocs: ['https://sheets.googleapis.com/$discovery/rest?version=v4']
      }).then(function () {
        // GoogleAuthオブジェクトを取得
        //   ref: https://developers.google.com/identity/sign-in/web/reference
        const auth = (<any>window).gapi.auth2.getAuthInstance();
        // ポップアップが閉じられると同時に状態を変更
        auth.signIn().then( () => 
          self.isLogined = auth.isSignedIn.get()
        )
      });
    });
  }

  logOut (): void {
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

  fetchSpreadSheetData(): void {
    // シートデータの取得
    (<any>window).gapi.client.sheets.spreadsheets.values.get({
      spreadsheetId: this.sheetId,
      range: this.sheetName + '!A1:C100',
    }).then((res: any)=>{
      this.spreadsheetRows = res.result.values;
      this.errorMsg = 'OK'

      // FIXME:
      // スプレッドシートの情報をAppにemit
      this.$emit('fetch', this.spreadsheetRows)
      // 自動再生のロジックはTimeKeeperに書く
    }, (reason: any) => {
      this.errorMsg = 'スプレッドシートを読み取れません'
    }) 
  }
}
</script>
