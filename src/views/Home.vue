<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title class="ion-text-center" style="color: white">Home</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div style="margin-top: 3em">
        <ion-grid>
          <div class="footer">
            <ion-row class="ion-justify-content-center">
              <ion-col size="8">
                <ion-card>
                  <ion-row class="ion-padding">
                    <ion-col class="ion-text-center">
                      <ion-label>
                        <h2>OFSM</h2>
                      </ion-label>
                    </ion-col>
                  </ion-row>
                  <ion-row
                    class="ion-justify-content-center ion-padding-horizontal"
                  >
                    <ion-col class="ion-text-left"  size="10">
                     <ion-list>
    <ion-list-header>
      <ion-label>
        State Selection
      </ion-label>
    </ion-list-header>

    <ion-item>
      <ion-label>State</ion-label>
      <ion-select placeholder="Select One">
        <ion-select-option value="f">Manual</ion-select-option>
        <ion-select-option value="m">Auto</ion-select-option>
      </ion-select>
    </ion-item>
                     </ion-list>
                    </ion-col>
                  </ion-row>
                    <ion-row
                        class="ion-justify-content-center ion-padding-horizontal"
                        style="padding-top: 0.3em">
                        <ion-col class="ion-text-center">
                            Map
                        </ion-col>
                    </ion-row>
                  <!-- <ion-row
        class="ion-justify-content-center ion-padding-horizontal"
        style="padding-bottom: 1em"
      >
        <ion-col class="ion-text-left">
          <ion-label (click)="forget()" style="font-size: 0.8em; color: #3eb5df; padding-left: 0.5em;" translate>
            Forgot password?
          </ion-label>
        </ion-col>
      </ion-row> -->

                  <ion-row
                    class="ion-padding ion-justify-content-center ion-padding-horizontal"
                  >
                    <ion-col class="ion-text-center" size="4">
                      <ion-button
                        @click="auth({ user: this.user, pass: this.pass })"
                        shape="round"
                        style="
                          width: 100%;
                          color: white;
                          --background: #ff2626;
                          text-transform: none;
                        "
                      >
                        Enter
                      </ion-button>
                    </ion-col>
                  </ion-row>
                </ion-card>
              </ion-col>
            </ion-row>
          </div>
        </ion-grid>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonRow,
  IonCol,
  IonInput,
  IonCard,
  IonButton,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonLabel,
  IonGrid,
  IonContent,
  IonPage,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { useMutation } from "@vue/apollo-composable";
import gql from "graphql-tag";

import { useRouter, useRoute } from "vue-router";

export default defineComponent({
  name: "HomePage",
  components: {
    IonRow,
    IonCol,
    IonCard,
    IonButton,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonLabel,
    IonGrid,
    IonContent,
    IonPage,
  },
  setup() {
    const router = useRouter();
    const route = useRoute();
    //   return { router };
    const { mutate: auth, onDone } = useMutation(gql`
      mutation auth($user: String!, $pass: String!) {
        tokenAuth(username: $user, password: $pass) {
          token
          user {
            id
            firstName
            lastName
            melliCode
          }
        }
      }
    `);

    onDone(() => {
      router.push({ path: "/home" });
      // router.replace()
    });

    return {
      auth,
      router,
      route,
    };
  },
  data() {
    return {
      user: "",
      pass: "",
    };
  },
  methods: {
    key: function (evt: any) {
      const ew = evt.key.charCodeAt();
      // console.log(ew)
      if (ew > 1000) {
        alert("لطفا زبان خود را عوض کنید");
        this.user = "";
      } // TS will throw an error here
      else {
        console.log("ok");
      }
    },
    login() {
      console.log(this.user, this.pass);

      // return{
      //     auth({username:this.username,password:this.pass})
      // }
    },
  },
});
</script>

<style scoped>
ion-toolbar {
  --background: #ff2626;
}

ion-input {
  border: 1px solid #aaaaaa;
  border-radius: 20px 20px 20px 20px;
  --placeholder-color: #aaaaaa;
  --background: #ffffff;
  --padding-start: 8px;
  text-align: right;
  --padding-end: 8px;
}

ion-content {
  --ion-background-color: #f7f7f7;
}
</style>
