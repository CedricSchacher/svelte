<script>
    import {seite} from "./App.svelte";

    export let ausgabe=[]
    let content;

    function detail(id){
        localStorage.setItem("id", id);
        localStorage.setItem("page", "detail");
        seite();

    }




    fetch('https://api-m120.mailino.io/api/posts?active=true').then(response => {
        if (response.ok) {
            return response.json();
        } else {
            throw Error(response.statusText);
        }
    }).then(body => {
        content=body;

        body.forEach(einfugen);



    }).catch(err => {
        console.log(err);
    })

    function einfugen(item, index){

        ausgabe.push({title:content[index].title,
            thumbnail:content[index].thumbnail,
            name:content[index].creatorFirstName + " " + content[index].creatorLastName,
            id:content[index].id});

        ausgabe=ausgabe;
    }




</script>

    <main>

        {#each ausgabe as { title, thumbnail, name, id }, i}
            <a class="posts" on:click={detail(id)}>
                <label class="titel"><h2>{title}</h2></label>
                <img src="{thumbnail}" class="thumb"/>
                <p class="profil">{name}</p>
            </a>

        {/each}


    </main>




<style>
    main{
        display: grid;
        grid-template-columns: 20% auto 20%;
    }
    .posts{
        grid-column: 2;
        display: grid;
        grid-template-rows: auto auto auto;
        grid-template-columns: auto auto;
        padding: 50px;
        margin: 5px;
        outline: gray double 2px;
    }
    .titel{
        grid-row: 1;
        grid-column: 1;
    }
    .thumb{
        grid-row: 1;
        grid-column: 2;
    }
    .profil{
        grid-row: 2;
        grid-column: 1;
    }
    img{
        height: 50px;
    }
    a{
        text-decoration: none;
    }



</style>