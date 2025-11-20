<script>
    import { fly, scale, slide } from "svelte/transition";
    import { onMount } from "svelte";
    import { handlers } from "svelte/legacy";
    import { base } from "$app/paths";

    let animate = $state(false);
    onMount(function() {
        animate = true;
    }, 100)

    let username = $state("");
    let fraud = $state(false);
    let filter = $state("");
    let totalTime = $state("");
    let langs = $state([]);

    onMount(() => {
        let id = sessionStorage.getItem("id");
        filter = sessionStorage.getItem("filter");
        fetch (`https://hackatime.hackclub.com/api/v1/users/${id}/stats?filter_by_project=${filter}`)
        .then ((data) => {
            return data.json();
        })
        .then((data) => {
            username = data.data.username;
            totalTime = data.data.human_readable_total;
            for (let i = 0; i < data.data.languages.length; i++) {
                langs.push([
                    data.data.languages[i].name.toUpperCase(),
                    data.data.languages[i].text
                ])
            }
            if (data.trust_factor.trust_value < 0) {
                fraud = true;
            }
            console.log(langs);
        })
    })
</script>
<svelte:head>
    <title>Oskar</title>
</svelte:head>
<style>
    #title {
        font-size: 90px;
    }
    #cover {
        position: fixed;
        z-index: -1000;
        top: -20px;
        bottom: -20px;
        left: 0px;
        right: 0px;
        background-color: rgb(146, 123, 74);
        background-image: repeating-linear-gradient(30deg, rgba(255,255,255,0.03) 0 10px,rgba(0,0,0,0.03) 5px 20px);
    }
    #username {
        span {
            background-color: rgb(90, 71, 31);
            padding: 15px;
            border-radius: 20px;
            margin-left: 10px;
        }
    }

    #details {
        margin-left: 0;
        margin-right: 0;
        padding: 20px;
        border-top-left-radius: 30px;
        border-top-right-radius: 30px;
        background-color: rgb(61, 61, 13);
    }

    .codeLang {
        background-color: rgb(63, 75, 13);
        margin-left: 30%;
        margin-right: 30%;
        margin-bottom: 20px;
        padding: 20px;
        border-radius: 20px;
        border: 5px ridge rgb(137, 197, 135);
        font-size: 20px;

        span {
            background-color: rgb(109, 114, 88);
            padding: 5px;
            border-radius: 10px;
            font-family: Space Grotesk, Epilogue;
        }
    }
</style>
<div id="cover"></div>
<br>
<button onclick={()=> {window.location.href = base + "/"}} style:background-color="rgb(142, 155, 28)" style:color="white">Return</button>
{#if animate}
    <h1 transition:scale id="title">OSKAR</h1>
    <h2 id="username" transition:slide>{username} <span>{filter}</span></h2>
    <br>
    {#if fraud}<h3><span style="background-color: brown; padding: 10px; border-radius: 10px">User has committed Hackatime fraud</span></h3>{/if}
    <br>
    <div id="details" transition:scale={{delay:2000}}>
        <h2 style="font-family: Montserrat; font-weight: 900; font-size:40px">{totalTime}</h2>
        <br>
        {#each langs as codeLang}
            <div class="codeLang"> 
                <h3 style= "font-family: Montserrat; font-weight: 900;">{codeLang[0]} <span>{codeLang[1]}</span></h3>
            </div>
        {/each}
    </div>
{/if}
<!--https://hackatime.hackclub.com/api/v1/users/<slackID>/stats?filter_by_project=<project name>-->