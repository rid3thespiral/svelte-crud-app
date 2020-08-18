<script>
	import '../node_modules/materialize-css/dist/css/materialize.min.css';
	import '../node_modules/materialize-css/dist/js/materialize.min.js';

	import {Router, Link, Route} from "svelte-routing";
	import Navbar from './layout/Navbar.svelte';
	import Home from './pages/home.svelte';
	import About from './pages/about.svelte';
</script>



<style>
	
</style>

<Router>
	<Navbar></Navbar>
	<div class="container">
		<Route path="/" component={Home}></Route>
		<Route path="/about" component={About}></Route>
	</div>
</Router>