<script>
    import { fly, scale, slide } from "svelte/transition";
    import { onMount } from "svelte";
    import { handlers } from "svelte/legacy";
    import { base } from "$app/paths";

    let animate = $state(false);
    onMount(function() {
        animate = true;
    }, 100)

    let id = $state("");
    let filter = $state("");
    function handle() {
        event.preventDefault();
        sessionStorage.setItem("id", id);
        sessionStorage.setItem("filter", filter);
        window.location.href = base + "/query";
    }

</script>
<svelte:head>
    <title>Oskar</title>
</svelte:head>
<style>
    #title {
        font-size: 90px;
    }
    #oskar {
        width: 40%;
        height: auto;
        margin: 0 auto;
        display: block;
        margin-bottom: 30px;
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
</style>
<div id="cover"></div>
{#if animate}
    <h1 transition:scale id="title">OSKAR</h1>
    <h2 transition:slide>Hackatime Viewer</h2>
    <br>
    <img id="oskar" src="oskar.png" alt="Oskar the Octopus" transition:fly={{duration:1000, y:500}}/>
    <div id="formBox" transition:slide={{delay:1500}}>
        <form onsubmit={() => {handle()}}>
            <div><input bind:value={id} type="text" placeholder="Slack ID" /></div>
            <div><input bind:value={filter} type="text" placeholder="Project Filter" /></div>
            <br>
            <button onclick={() => {handle()}}>Submit</button>
        </form>
    </div>
{/if}
<!--https://hackatime.hackclub.com/api/v1/users/U08EMT46G3V/stats?filter_by_project=<project name>-->