<template>
    <Page>
        <ActionBar>
            <Label text="Postshare"/>
        </ActionBar>
        <StackLayout >
             <ListView height="50%" for="tweet in tweets" class="list-group">
                <v-template>
                    <StackLayout class="list-group-item" height="100%">
                        <TextView :text="tweet.tweet" class="h2" height="100%"/>
                    </StackLayout>
                </v-template>
            </ListView>
            <ActivityIndicator :busy="loading"/>
            <Button height="auto" class="button generate" text="GENERATE"
                @tap="dataFetch" />
            <AbsoluteLayout  height="auto"
                backgroundColor="lightgray" class="absolute">
                <Label left="10" width="100" class="toast-lable"  v-if="generatedVisible"
                  text="generated" />
            </AbsoluteLayout>
        </StackLayout>
    </Page>
</template>
<script>
import * as http from "@nativescript/core/http";
  export default {
    data() {
        return {
        tweets:[{"tweet":"Please wait... The Server is experiencing numerous requests."}],
        text:"",
        generatedVisible: false,
        loading :false
        };
    },
    methods: {
        async dataFetch(){
        this.loading = true
        var tweets = await http.getJSON("https://webservices.cyou/1").then(result => {
        this.tweets = result;
        this.generatedVisible = true;
        }, error => {
            alert('Oops, unable to get tweet. Please Check Internet Connection');
                console.log(error);
            }).finally(() => (this.loading = false, setTimeout(() => (this.generatedVisible = false), 1000)));
        },
    }
};
</script>
<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';
    ActionBar {
        background-color: #56aded;
        color: #ffffff;
    }
    .button{
        font-size:18;
        border-radius:30;
    }
    .absolute{
        width: 200;
        height: auto;
        text-align: right;
        color: white;
        max-width: 130px;
        border-radius: 25px;
    }
    .toast-lable{
        text-align: center;
        padding: 15;
    }
</style>
