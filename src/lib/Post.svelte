<script>
	import { createComment, createLike, getUser } from '$lib/services'
	import Error from './Error.svelte'

	export let id,
		user,
		content,
		likes = 0,
		comments = [],
		publicURL = null,
		error

	let commentContent = ''
	let createCommentPromise = Promise.resolve({})
	function addComment() {
		createCommentPromise = createComment({
			user: getUser().email,
			post: id,
			content: commentContent
		}).then(({ data, error }) => {
			commentContent = ''
			comments = [...data, ...comments]
			return { data, error }
		})
	}

	function addLike() {
		likes += 1
		createLike({ user: getUser().email, post: id })
	}
</script>

<article class="card shadow-xl bg-transparent text-accent-content mt-8">
	{#if publicURL}
		<figure>
			<img src="{publicURL}" alt="img" />
		</figure>
	{/if}
	<div class="card-body">
		<h2 class="card-title">{user} says..</h2>
		<p class="mb-8">{content}</p>
		<div class="card-actions flex justify-between">
			{#await createCommentPromise}
				Posting comments..
			{:then { data, err }}
				<Error error="{error}" />
				{#if data}
					Thanks for creating an insightful and kind comment!
				{:else if error}
					<strong class="text-red-500">{error}</strong>
				{/if}
				<form class="form-control" on:submit|preventDefault="{addComment}">
					<div class="flex space-x-2">
						<input
							bind:value="{commentContent}"
							type="text"
							placeholder="Yout thoughts.."
							class="w-full input input-primary input-bordered"
						/>
						<button class="btn btn-primary">Comment</button>
					</div>
				</form>
			{/await}
			<button on:click="{addLike}" class="btn btn-secondary"
				>{likes} {likes === 0 ? 'Like' : 'Likes'}</button
			>
		</div>
		{#each comments as comment}
			{comment.user} says.. <q>{comment.content}</q>
		{/each}
	</div>
</article>
