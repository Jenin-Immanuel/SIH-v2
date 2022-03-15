<script context="module">
  import firebase from "firebase/compat/app";
  import * as firebaseui from "firebaseui";
  import "firebaseui/dist/firebaseui.css";
  import { firebaseConfig } from "./config";

  firebase.initializeApp(firebaseConfig);

  let ui = new firebaseui.auth.AuthUI(firebase.auth());

  ui.start("#firebaseui-auth-container", {
    signInOptions: [
      firebase.auth.EmailAuthProvider.PROVIDER_ID,
      firebase.auth.GoogleAuthProvider.PROVIDER_ID,
    ],
    signInSuccessUrl: "/login/success",
  });
</script>

<div>
  <div id="firebaseui-auth-container" />
</div>

<style>
  #firebaseui-auth-container {
    position: absolute;
    inset: 0;
    display: grid;
    place-items: center;
  }
</style>
