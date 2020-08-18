<script>

import { onMount } from 'svelte';
import App from "../App.svelte";
import About from"./about.svelte";


    const apiBaseUrl= 'https://jsonplaceholder.typicode.com/posts';
    let posts = [];

    onMount( async () => {
        const response = await fetch(apiBaseUrl);
        posts = await response.json();
    })

    function editPost(post){
        console.log(post);
    }

    function deletePost(){
        console.log("Deleting post with id: ", id);
    }
    
</script>

<style>
    .delete-btn{
        color: red !important;
    }
    .card .card-content .card-title{
        margin-bottom: 0;
        color: #9999;
        margin-bottom: 10px;
    }
</style>

<div class="row">
    {#if posts.length === 0}
    <h3>Loading posts...</h3>
    {:else}
        {#each posts as post}
            <div class="col s6">
                <div class="card">
                    <div class="card-content">
                        <p class="card-title">{post.title}</p>
                        <p>{post.userId}</p>
                    </div>
                    <div class="card-action">
                        <a href="#" on:click={ () => editPost(post) }> Edit</a>
                        <a href="#" class="delete-btn" on:click={() => deletePost(post.id)}>Delete</a>
                    </div>
                </div>
            </div>
            {/each}
            {/if}
</div>