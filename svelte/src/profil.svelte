<script>
        import {seite} from "./App.svelte";


        let content;
        let ausgabe=[];


    function ubersicht(){
        localStorage.setItem("page", "ubersicht");
        seite();
    }
        function detail(id){
            localStorage.setItem("id", id);
            localStorage.setItem("page", "detail");
            seite();

        }

            fetch('https://api-m120.mailino.io/api/users/' + localStorage.getItem("profilId") + '/posts').then(response => {
                if (response.ok) {
                    return response.json();
                } else {
                    throw Error(response.statusText);
                }
            }).then(body => {

                content = body;
                body.forEach(einfugen);


            }).catch(err => {
                console.log(err);
            })



        function einfugen(item, index) {

                ausgabe.push({
                    title: content[index].title,
                    thumbnail: content[index].thumbnail,
                    id: content[index].id

                });

                document.getElementById("vorname").innerHTML=content[index].creatorFirstName;
                document.getElementById("nachname").innerHTML=content[index].creatorLastName;

                ausgabe = ausgabe;

        }



</script>
<main>

<label id="vorname"></label>
<label id="nachname"></label>


{#each ausgabe as { title, thumbnail, name, id }, i}
    <a class="posts" on:click={detail(id)}>
        <label class="titel"><h2>{title}</h2></label>
        <img src="{thumbnail}" class="thumb"/>
    </a>

{/each}

    <button id="ubersicht" on:click={ubersicht}>Zurück zur Übersicht</button>

</main>







<style>
    main{
        display: grid;
        grid-template-columns: 10% 10% auto 20%;
    }
    #vorname{
        grid-row: 1;
        grid-column: 1;
        margin: auto;
        font-size: 25px;
        font-weight: bold;
    }
    #nachname{
        grid-row: 1;
        grid-column: 2;
        margin: auto;
        font-size: 25px;
        font-weight: bold;
    }
    #ubersicht{
        grid-row: 2;
        grid-column: 1/3;
        text-align: center;
        display: block;

    }
    .posts{
        grid-column: 3;
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

    img{
        height: 50px;
    }


    a{
        text-decoration: none;
    }



</style>