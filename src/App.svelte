<script>
  import { Router, Link, Route, navigate } from "svelte-navigator";
  import firebase from "firebase/compat/app";
  import Map from "./components/Map.svelte";
  import Login from "./components/Login.svelte";

  let user = {};

  firebase.auth().onAuthStateChanged((_user) => {
    if (_user) {
      // User is signed in, see docs for a list of available properties
      // https://firebase.google.com/docs/reference/js/firebase.User
      user = _user;

      console.log(user.displayName);
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
  </Router>
</main>

<style>
</style>
