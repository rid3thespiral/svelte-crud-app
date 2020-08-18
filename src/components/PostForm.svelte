<script>
    
	import { createEventDispatcher } from 'svelte';
    export let editingPost;
	const dispatch = createEventDispatcher();


    $: title = editingPost.title;
    $: body = editingPost.body;
    let loading = false;

    const apiBaseUrl = 'https://jsonplaceholder.typicode.com/posts';

    async function onSubmit(event){
        event.preventDefault();

        if(title.trim() === '' || body.trim() === ''){
            return;
        }

        loading = true;

        const newPost = {title, body};

        let url, method;

        if(editingPost.id){
            url = `${apiBaseUrl}/${editingPost.id}`;
            method = "PUT"
        } else {
            url = `${apiBaseUrl}`
            method = "POST"
        }

        const res = await fetch(url, {
            method,
            body: JSON.stringify(newPost)
        });
        const post = await res.json();
        dispatch('postCreated', post);
        title = body = "";
        loading = false;
    }
</script>

<style>
    form{
        margin: 50px;
    }
    .progress {
        margin: 100px 0px;
    }
</style>

{#if !loading}
<form on:submit={onSubmit}>
    <div class="input-field">
        <label for="title">Title</label>
        <input type="text" bind:value={editingPost.title}>
    </div>
    <div class="input-field">
        <label for="body">Body</label>
        <input type="text" bind:value={editingPost.body}>
    </div>
    <button type="submit" class="waves-effect waves-light btn">
        {editingPost.id ? "Update" : "Add"}
    </button>
</form>
{:else}
    <div class="progress">
        <div class="indeterminate">

        </div>
    </div>
{/if}