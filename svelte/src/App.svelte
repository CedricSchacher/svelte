<script context="module">
	export function seite(){
		document.getElementById("ubersicht").hidden=true;
		document.getElementById("detail").hidden=true;
		document.getElementById("profil").hidden=true;
		document.getElementById(localStorage.getItem("page")).hidden=false;
	}



</script>
<script>
	import BlogUbersicht from "./blogUbersicht.svelte";
	import BlogDetail from "./blogDetail.svelte";
	import Profil from "./profil.svelte";
	let loggedIn=false;

	localStorage.setItem("id", 1);
		localStorage.setItem("page", "ubersicht");





	let logIn = function() {

		fetch('https://api-m120.mailino.io/api/auth/login', {
			method: 'POST',
			body: JSON.stringify({
				email: document.getElementById("email").value,
				password: document.getElementById("password").value
			}),
			headers: {
				'Content-Type': 'application/json'
			}
		}).then(response => {
			if (response.ok) {
				return response.json();
			} else {
				loginFehler();
			}
		}).then(body => {
			if (body.token) {
				loggedIn = true;

				localStorage.setItem("Token", body.token);
				console.log(localStorage.getItem("Token"));
				document.getElementById("loginFehler").style.display="none";
				getUser();
			}

		}).catch(err => {
			console.log(err);
		})



	}


	function getUser() {fetch('https://api-m120.mailino.io/api/users/self', {
		method: 'GET',
		headers: {
			'Authorization': `Bearer ${localStorage.getItem('Token')}`
		}
		}).then(response => {
				if (response.ok) {
					return response.json();
				} else {
					throw Error(response.statusText);
				}
			}).then(body => {
				document.getElementById("benutzerName").innerHTML=body.firstName+ " " +body.lastName;
			}).catch(err => {
				console.log(err);
			})

		}


	function logOut(){
		loggedIn=false;
		localStorage.setItem("Token", null);
	}
	function loginFehler(){
		document.getElementById("loginFehler").style.display="block";
	}
	function emailKontrolle(){
		if (document.getElementById("email").value.length<5&&document.getElementById("email").value.length>0){
			document.getElementById("email").style.backgroundColor="#FF8888";

		}
		else{
			document.getElementById("email").style.backgroundColor="#FFFFFF";

		}
		
	}






</script>

<main>
	{#if !loggedIn}
		<div id="einloggen">
		<form novalidate>
			<label for="einloggen"><h2>Login</h2></label>
			<input on:change={emailKontrolle} placeholder="Email" id="email">

			<input placeholder="Passwort" type="password" id="password">
		</form>
		<button on:click={logIn}>Login</button>
			<label id="loginFehler" for="password" >Die Einlogdaten sind nicht korrekt.</label>
		</div>
	{:else}
		<div id="benutzer">
			<label id="benutzerName">Benutzername</label>
			<button id="knopf" on:click={logOut}>Ausloggen</button>
		</div>
	{/if}




	<div id="inhalt">
	<div id="ubersicht" >
		<BlogUbersicht/>
	</div>

	<div id="detail" hidden>
		<BlogDetail/>
	</div>

	<div id="profil" hidden>
		<Profil/>
	</div>
	</div>













</main>


<style>




	main {
		display: grid;
		grid-template-columns: auto;
		grid-template-rows: 250px auto;
	}
	#einloggen{
		color: white;
		text-align: center;
		outline: beige solid 5px;
		border-radius: 5px;
		grid-column: 1;
		grid-row: 1;
		background: #4c1508;
	}
	#loginFehler{
		display: none;

	}
	#inhalt{
		grid-row: 2;
		grid-column: 1;
	}
	#benutzer{
		font-size: 25px;
		text-align: center;
		border: beige solid 5px;
		border-radius: 5px;
		padding: 45px;
		background-color: #3b1207;
		color: white;
	}
	#knopf{
		display: block;
		margin:auto;
	}
	#ubersicht{
		border: black solid 5px;
		border-radius: 15px;
		margin-left: 15px;
		margin-right: 15px;

	}
	#detail{
		border: black solid 5px;
		border-radius: 15px;
		margin-left: 15px;
		margin-right: 15px;
	}
	#profil {
		border: black solid 5px;
		border-radius: 15px;
		margin-left: 15px;
		margin-right: 15px;

	}


</style>