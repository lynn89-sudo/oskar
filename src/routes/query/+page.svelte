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
    let filter = $state("");
    let totalTime = $state("");

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
</style>
<div id="cover"></div>
<br>
<button onclick={()=> {window.location.href = base + "/"}} style:background-color="rgb(142, 155, 28)" style:color="white">Return</button>
{#if animate}
    <h1 transition:scale id="title">OSKAR</h1>
    <h2 id="username" transition:slide>{username} <span>{filter}</span></h2>
    <br>
    <h2 style="font-family: Montserrat; font-weight: 700">{totalTime}</h2>
{/if}
<!--https://hackatime.hackclub.com/api/v1/users/<slackID>/stats?filter_by_project=<project name>-->