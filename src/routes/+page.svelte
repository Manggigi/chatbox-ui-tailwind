<script lang="ts">
	import { afterUpdate, beforeUpdate } from 'svelte';
	// import Icon from 'svelte-icons-pack/Icon.svelte';
	// import AiOutlineSend from 'svelte-icons-pack/ai/AiOutlineSend';
	import { fly } from 'svelte/transition';

	enum ChatMessageRole {
		USER = 'user',
		SYSTEM = 'system',
		ASSISTANT = 'assistant'
	}

	type AiChatMessage = {
		role?: ChatMessageRole;
		content: string;
	};

	let userMessage = '';
	let latestUserMessage = '';
	let latestAiResponse = '';
	let aiChatArray: AiChatMessage[] = [];
	let div: HTMLElement;
	let autoscroll = false;

	beforeUpdate(() => {
		if (div) {
			const scrollableDistance = div.scrollHeight - div.offsetHeight;
			autoscroll = div.scrollTop > scrollableDistance - 20;
		}
	});

	afterUpdate(() => {
		if (autoscroll) {
			div.scrollTo(0, div.scrollHeight);
		}
	});

	const handleSendAiMessage = async () => {
		if (!userMessage) return;
		latestUserMessage = userMessage;
		aiChatArray = [...aiChatArray, { content: latestUserMessage, role: ChatMessageRole.USER }];

		userMessage = '';
		setTimeout(() => {
			latestAiResponse = '...';
			aiChatArray = [
				...aiChatArray,
				{ content: latestAiResponse, role: ChatMessageRole.ASSISTANT }
			];
		}, 300);

		// your api call here to get response
		// if (response) {
		// 	aiChatArray.pop();
		// 	aiChatArray = [...aiChatArray, { content: response, role: ChatMessageRole.ASSISTANT }];
		// }

		// mock response
		setTimeout(() => {
			aiChatArray.pop();
			const aiResponse = "Hello, I'm Gigi, your personal assistant. How can I help you today?";
			aiChatArray = [...aiChatArray, { content: aiResponse, role: ChatMessageRole.ASSISTANT }];
		}, 1000);
	};
</script>

<div class="mx-auto max-w-screen-sm px-4 mb-16 sm:mt-10 mt-8">
	<div class="flex space-x-2 mb-4">
		<span class="font-medium text-xl">Gigi</span>
	</div>

	<div
		class="h-[70vh] bg-neutral-200 dark:bg-slate-700 mb-4 rounded-lg space-y-2 py-6 px-4 overflow-auto"
		bind:this={div}
	>
		{#each aiChatArray as msg}
			{#if msg.role === ChatMessageRole.ASSISTANT}
				<div class="flex items-end space-x-2" transition:fly={{ y: '100%' }}>
					<img src="/gigi.webp" alt="deepy-bot" class="w-8 h-8 rounded-full" />
					<p
						class="whitespace-pre-wrap px-4 py-2 bg-neutral-300 dark:bg-slate-600 rounded-md max-w-[75%] rounded-bl-none break-words"
					>
						{msg.content}
					</p>
				</div>
			{:else}
				<div class="flex justify-end" transition:fly={{ y: '100%' }}>
					<p
						class="whitespace-pre-wrap px-4 py-2 bg-purple-700 text-white rounded-md max-w-[75%] rounded-br-none break-words"
					>
						{msg.content}
					</p>
				</div>
			{/if}
		{/each}
	</div>
	<div class="flex w-full">
		<input
			class="px-4 rounded-md w-full focus:bg-neutral-100 dark:focus:bg-slate-700"
			bind:value={userMessage}
			on:keydown={(e) => e.key === 'Enter' && handleSendAiMessage()}
			type="text"
			name="message"
			placeholder="Type a message..."
		/>
		<button
			class="px-4 py-3 cursor-pointer rounded-md ml-3 bg-purple-700 hover:bg-purple-400 hover:text-white hover:border-transparent"
			disabled={!userMessage}
			on:click={handleSendAiMessage}
		>
			<!-- <Icon src={AiOutlineSend} size="20px" color="white" /> -->
			Send
		</button>
	</div>
</div>
