<script>

import { onMount } from 'svelte';
import App from "../App.svelte";
import About from"./about.svelte";
import PostForm from '../components/PostForm.svelte';

    const apiBaseUrl= 'https://jsonplaceholder.typicode.com/posts';
    let posts = [];

    let editingPost = {
        body: '',
        title: '',
        id: null
    }

    onMount( async () => {
        const response = await fetch(apiBaseUrl);
        posts = await response.json();
    })

    function addPost({ detail: post}){
        posts = [post, ...posts];
    }

    function editPost(post){
        editingPost = post
    }

    function deletePost(id){
        if(confirm("are you sure?")){fetch(` ${apiBaseUrl}+'/'+${id} `, {
            method: 'DELETE'
        }).then(res => {
            return res.json()
        }).then(() => {
            posts = posts.filter(p => p.id !== id);
        })
    }
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
<div class="col s6">
    <PostForm on:postCreated={addPost} {editingPost}}/>
</div></div>

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