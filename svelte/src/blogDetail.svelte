
<script>
    import {seite } from "./App.svelte";
    let content;
    
    setInterval(function(){
        fetch('https://api-m120.mailino.io/api/posts/' + localStorage.getItem("id")).then(response => {
        if (response.ok) {
            return response.json();
        } else {
            throw Error(response.statusText);
        }
    }).then(body => {

        content = body;
            document.getElementById("titel").innerHTML=body.title;
            document.getElementById("thumb").src=body.thumbnail;
            document.getElementById("name").innerHTML=body.creatorFirstName + " " + body.creatorLastName;
            document.getElementById("text").innerHTML=body.body;



    }).catch(err => {

    });
        }, 500);





    function ubersicht(){
        localStorage.setItem("page", "ubersicht");
        seite();
    }
    function profil(){

        localStorage.setItem("page", "profil");
        getProfilId();
        seite();

    }
    function getProfilId(){
        localStorage.setItem("profilId", content.creatorId);
    }

    function nachster(){
        let id=parseInt(localStorage.getItem("id"))+1;
        localStorage.setItem("id", id);

    }



</script>

<main>
    <button id="zuruck" on:click={ubersicht}>Zurück</button>
    <div class="posts">
        <h2><label id="titel" >Titel</label></h2>
        <img src="" id="thumb"/>
        <p on:click={ profil }  id="name">Ersteller (vorname)(nachname)</p>
        <p id="text" >Inhalt</p>
    </div>
    <button id="nachster" on:click={nachster}>Nächster Post</button>
</main>


<style>
    #thumb{
        height: 50px;
        grid-column: 2;
    }
    main{
        display:grid;
        grid-template-columns: 20% auto 20%;
    }
    .posts{
        display: grid;
        grid-template-columns: auto auto;
        grid-template-rows: auto auto auto;
        grid-column: 2;
        padding: 50px;
        outline: gray double 2px;
    }
    #titel{
        grid-column: 1;
        grid-row: 1;
    }
    #name{
        color: blue;
        grid-column: 1;
        grid-row: 2;
    }
    #text{
        grid-column: 1;
        grid-row: 3;
    }
    #zuruck{
        font-size: 25px;
        font-weight: bold;
    }
    #nachster{
        font-size: 25px;
        font-weight: bold;
    }




</style>