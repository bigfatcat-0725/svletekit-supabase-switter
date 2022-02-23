<script>
	import { createPost, getUser } from '$lib/services'
	import Error from '$lib/Error.svelte'
	import UploadImage from '$lib/UploadImage.svelte'

	let file = null
	let postContext = ''

	let createPostPromise = Promise.resolve({})
	function handleCreatePost() {
		createPostPromise = createPost({ user: getUser().email, content: postContext, imageFile: file })
	}
</script>

<form class="form-control" on:submit|preventDefault="{handleCreatePost}">
	<label for="" class="label">
		<span class="label-text text-xl">What would you like to swit?</span>
	</label>
	<UploadImage bind:file />
	<textarea bind:value="{postContext}" id="post" required class="textarea h-24 textarea-bordered"
	></textarea>
	{#await createPostPromise}
		<button disabled type="button" class="btn">Swit that Shwit!</button>
	{:then { data, error }}
		<button class="btn">Swit that Shwit!</button>
		{#if data}
			<strong class="text-green-600">Successfully create post!</strong>
		{/if}
		<Error error="{error}" />
	{/await}
</form>
