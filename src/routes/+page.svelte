<script lang="ts">
	const prompts = [
		'Why are you holding on to this?',
		'What is so important about this?',
		'Do you want to hold on to this?'
	];
	let prompt = 'What are you holding on to?';
	let lastPromptIndex: number = -1;

	let firstInput: boolean = true;
	let userInput: string = '';
	let holdingOnTo: string = '';

	let buttonTransparency: number = 0;
	const buttonTransparencyIncrement = 0.1;

	let showNothing = false;

	function getRandomPrompt() {
		let promptIndex = Math.floor(Math.random() * prompts.length);
		while (promptIndex === lastPromptIndex) {
			promptIndex = Math.floor(Math.random() * prompts.length);
		}

		lastPromptIndex = promptIndex;
		return prompts[promptIndex];
	}

	function onKeyDown(e: KeyboardEvent) {
		if (e.key === 'Enter') {
			if (firstInput) {
				firstInput = false;
				holdingOnTo = userInput;
			}
			if (buttonTransparency >= 1 - buttonTransparencyIncrement) {
				prompt = 'Are you ready to let go?';
			} else {
				prompt = getRandomPrompt();
			}

			userInput = '';
			buttonTransparency += buttonTransparencyIncrement;
		}
	}

	function onClickReadyToLetGo() {
		firstInput = true;
		userInput = '';
		holdingOnTo = '';
		buttonTransparency = 0;
		showNothing = true;
	}

	function onClickNotReadyToLetGo() {
		prompt = getRandomPrompt();
		userInput = '';
		buttonTransparency = 0;
	}
</script>

<div class="mx-auto h-screen max-w-7xl px-4 sm:px-6 lg:px-8">
	<div class="flex h-1/2 items-center justify-center">
		<p
			class="text-xl sm:text-3xl"
			style="transition: opacity 0.5s ease-in-out; opacity: {showNothing ? 0 : 1}"
		>
			{prompt}
		</p>
	</div>

	<div class="relative">
		<input
			type="text"
			name="input"
			class="top-1/2 w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
			style="transition: opacity 0.5s ease-in-out; opacity: {showNothing ? 0 : 1}"
			bind:value={userInput}
			on:keydown={onKeyDown}
		/>
		<button>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="absolute right-1 top-2 h-6 w-6 rounded-md text-cyan-600 opacity-70 ring-opacity-10 sm:hidden"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M6 12L3.269 3.126A59.768 59.768 0 0121.485 12 59.77 59.77 0 013.27 20.876L5.999 12zm0 0h7.5"
				/>
			</svg>
		</button>
	</div>

	<button
		class="mt-6 w-full break-words rounded-md border border-transparent bg-cyan-500 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-cyan-600 focus:outline-none focus:ring-2 focus:ring-cyan-400"
		style="transition: opacity 0.5s ease-in-out; opacity: {buttonTransparency}"
		disabled={buttonTransparency === 0}
		on:click={onClickReadyToLetGo}
	>
		I am ready to let go of {holdingOnTo}
	</button>
	<button
		class="mt-2 w-full break-words rounded-md border border-transparent bg-indigo-500 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-600 focus:outline-none focus:ring-2 focus:ring-indigo-400"
		style="transition: opacity 0.5s ease-in-out; opacity: {buttonTransparency >= 1 ? 1 : 0}"
		disabled={buttonTransparency < 1}
		on:click={onClickNotReadyToLetGo}
	>
		I am not ready to let go
	</button>
</div>
