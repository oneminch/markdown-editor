<script>
	import MarkdownIt from "markdown-it";

  let md = new MarkdownIt("commonmark", {
		html: true,
		linkify: true,
		typographer: true
	});

	let editing = false;
	let inputText = `# Hello, World\n\nI'm a super simple **markdown editor** powered by [Markdown-It](https://markdown-it.github.io/markdown-it/)\n\n![Markdown Logo](https://avatars.githubusercontent.com/u/10248321?s=100&v=4)\n\n---\n\nYou can write basic markdown syntax on the left/top here which will be rendered on the right/bottom.`;
	let html = "";

	function parseMarkdown() {
		html = md.render(inputText);
	}

	function exportMarkdown() {
		let dataUri = 'data:text/plain;charset=utf-8,'+ encodeURIComponent(inputText);
		let exportFileName = 'export.md';
		let linkElement = document.createElement('a');
		linkElement.setAttribute('href', dataUri);
		linkElement.setAttribute('download', exportFileName);
		linkElement.click();
	}

	parseMarkdown()
</script>

<main>
	<!-- Navigation -->
	<nav>
		<h3>Svelte Markdown Editor</h3>
		<span class="{editing === true ? 'visible' : ''}">Editing</span>
	</nav>

	<div class="editor">
		<!-- Input -->
		<textarea
			class="input"
			on:blur="{() => editing = false}"
			on:focus="{() => editing = true}"
			on:keyup="{parseMarkdown}"
			bind:value="{inputText}"
		></textarea>

		<!-- Output -->
		<div class="output" >{@html html}</div>

		<!-- Options -->
		<button on:click="{exportMarkdown}" aria-label="Export .md file" title="Export markdown file">
			<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
			</svg>
		</button>
	</div>
</main>

<style>
	:root {
		font-family: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", sans-serif;
	}

	* {
		transition: all 0.3s;
	}

	main {
		width: 95%;
		height: calc(100vh - 3rem);
		margin: 0 auto;
	}
	
	nav {
		text-align: left;
		height: 4rem;
	}

	nav > h3 {
		margin: 1rem 0 0.25rem 0;
	}

	nav > span {
		display: inline-block;
		font-size: .8rem;
		font-weight: bold;
		color: white;
		padding: .25rem .5rem;
		border-radius: 1rem;
		margin: 0.1rem;
		background-color: #20c997;
		visibility: hidden;
	}

	nav > span.visible {
		visibility: visible;
	}

	
	.editor {
		display: flex;
		justify-items: start;
		align-items: center;
		width: 100%;
		margin: 0 auto;
		height: 85%;
		border-radius: .5rem;
		box-sizing: border-box;
		border: 2px solid #222;
		border-bottom: 4px solid #222;
		overflow: hidden;
		position: relative;
	}
	
	.input, .output {
		width: 50%;
		height: 100%;
		padding: 1rem;
		word-break: break-word;
		box-sizing: border-box;
		overflow-y: scroll;
	}

	.input {
		border: none;
		resize: none;
		border-right: 2px solid #000;
	}

	.input:focus {
		outline: none;
	}

	button {
		width: 3rem;
		height: 3rem;
		border-radius: 50%;
		border: none;
		position: absolute;
		bottom: 1.5rem; right: 1.5rem;
		background-color: #20c997;
		color: #fff;
		font-weight: bold;
		cursor: pointer;
		font-size: .75rem;
	}

	button svg {
		width: 1.5rem;
		height: 1.5rem;
	}

	@media only screen and (max-width: 768px) {
		.editor {
			flex-direction: column;
		}

		.input, .output {
			width: 100%;
			height: 50%;
		}


		.input {
			border: none; 
			border-bottom: 2px solid #000;
		}
	}
</style>
