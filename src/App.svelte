<script>
    import {PlacemarkService} from "./services/PlacemarkService.js";
    import {setContext} from "svelte";

    import Main from "./pages/Main.svelte";
    import Login from "./pages/Login.svelte";
    import Signup from "./pages/Signup.svelte";
    import Category from "./pages/Category.svelte";
    import Dashboard from "./pages/Dashboard.svelte";
    import Map from "./pages/Map.svelte";
    import Charts from "./pages/Charts.svelte";
    import Router from "svelte-spa-router";
    import ImageGallery from "./pages/ImageGallery.svelte";

    setContext("PlacemarkService", new PlacemarkService("http://localhost:4000"));

    let routes = {
        "/": Main,
        "/login": Login,
        "/signup": Signup,
        "/dashboard": Dashboard,
        "/category/:id": Category,
        "/place/:id": ImageGallery,
        "/map": Map,
        "/charts": Charts,
        "/logout": Main
    }
</script>

<div class="container">
    <Router {routes}/>
</div>
