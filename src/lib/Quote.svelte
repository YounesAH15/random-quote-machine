<script>
    import { apiData, Quotes } from './store.js';
    import { onMount } from 'svelte';
    let apiKey = "j9q7Sqj3Gop8jvIlqNZ75w==mRUMvwpnufKEjMIZ";

    let r,g,b;
    let randomColor ;

    let setColors = () => {
        r = Math.random()*255;
        g = Math.random()*255;
        b = Math.random()*255;
        randomColor = "rgb("+r+","+g+","+b+")";
        let section = document.getElementById("centered");
        let btn = document.getElementById("btn");
        section.style.backgroundColor = randomColor;
        btn.style.backgroundColor = randomColor;
        btn.style.color = "white";
        section.style.color = randomColor;
        console.log("setColors::", {
            randomColor, r,g,b
        })
    }

    let loadQuote = async () => {
        apiData.set(null)
        setColors();
        let quoteElement = document.getElementById("qte");
        quoteElement.style.backgroundColor = "white";
        quoteElement.classList.toggle("puff-in-center");
        await fetch("https://api.api-ninjas.com/v1/quotes", {
                    headers: {'X-Api-Key': apiKey},
                    contentType: 'application/json',
                    method: "GET"
        })
        .then(response => response.json())
        .then(data => {
            console.log(data);
            apiData.set(data);
            quoteElement.classList.toggle("puff-in-center");

        }).catch(error => {
            console.log(error);
            return [];
        });


    }

    onMount(async ()=> {
        setColors();
        await loadQuote();
    });


</script>


    <section id="centered">

        <div class="q" id="qte">
            {#if $Quotes}
                {#each $Quotes as quote}
                    "{quote.quote}"
                    <span class="auth">
                        - {quote.author}
                    </span>
                {/each}

            {/if }
            {#if !$Quotes}
                <div class="loading">LOADING</div>
            {/if}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            <div class="cta" id="btn" on:click={loadQuote}>
                <div>New Quote</div>
            </div>
        </div>

    </section>

<style lang="scss">

    :global(body) {
        margin: 0;
        font-weight: 500;
        font-size: 1.75em;
    }

    #centered {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;

    }

    .q {
        width: 60vw;
        border: 1px solid grey;
        padding: 5vw;
        text-align: center;
    }

    .auth {
        display: block;
        padding: 5px;
        padding-left: 45vw;
    }

    .cta {
        width: 20%;
        display: flex;
        justify-content: center;
        margin-top: 5vh;
        margin-left: auto;
        text-align: center;
        border-radius: 10px;
        padding: 5px;
        cursor: pointer;
    }
</style>