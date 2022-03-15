<script>
  import { Router, Link, Route, navigate } from "svelte-navigator";
  import firebase from "firebase/compat/app";
  import Map from "./components/Map.svelte";
  import Login from "./components/Login.svelte";
import AfterLogin from "./components/AfterLogin.svelte";

  let user = {};

  firebase.auth().onAuthStateChanged((_user) => {
    if (_user) {
      // User is signed in, see docs for a list of available properties
      // https://firebase.google.com/docs/reference/js/firebase.User
      user = _user;
      // ...
    } else {
      // User is signed out
      // ...
      navigate("/login");
    }
  });
</script>

<main>
  <Router>
    <Route path="/" component={Map} />
    <Route path="login" component={Login} />
    <Route path="login/success" component={AfterLogin} />
  </Router>
</main>

<style>
</style>
