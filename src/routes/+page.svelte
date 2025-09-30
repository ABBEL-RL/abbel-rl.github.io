<script>
	import LinkButton from '$lib/components/LinkButton.svelte';
	import Block from '$lib/components/Block.svelte';

	import PaperIcon from 'virtual:icons/mingcute/pdf-line';
	import GithubIcon from 'virtual:icons/mdi/github';
	import ArxivIcon from 'virtual:icons/simple-icons/arxiv';

	// Update these links when available
	let paper_link = '#'; // Will be arXiv PDF link when available
	let arxiv_link = '#'; // Will be arXiv abstract link when available  
	let github_link = '#'; // Add your GitHub repo link when available
</script>

<svelte:head>
	<title>ABBEL: Acting through Belief Bottlenecks</title>
</svelte:head>

<div class="text-lg">
	<!-- START HEADER -->
	<div
		class="flex w-full justify-center font-serif items-center py-10 md:py-20 px-4 md:px-6 flex-col md:flex-row"
	>
		<div class="mx-4 md:mx-8 max-w-4xl text-black">
			<div class="text-left">
				<div class="text-3xl md:text-5xl font-serif leading-1">
					<span class="font-semibold">ABBEL:</span>
					<br />
					<span>LLM Agents Acting through Belief Bottlenecks Expressed in Language</span>
				</div>
				<div class="mt-6 md:mt-8 text-base md:text-lg">
					Anonymous authors <br />
					<span class="mt-2 block text-sm italic">Preprint.</span>
				</div>
				<div class="flex mt-8 space-x-4">
					<a href={paper_link}>
						<LinkButton>
							<div class="flex items-center">
								<PaperIcon class="inline-block mr-1" />
								Paper
							</div>
						</LinkButton>
					</a>
					<a href={arxiv_link}>
						<LinkButton>
							<div class="flex items-center">
								<ArxivIcon class="inline-block mr-1" />
								arXiv
							</div>
						</LinkButton>
					</a>
					<a href={github_link} target="_blank" rel="noreferrer">
						<LinkButton>
							<div class="flex items-center">
								<GithubIcon class="inline-block mr-1" />
								Code
							</div>
						</LinkButton>
					</a>
				</div>
			</div>
		</div>
		<div class="mx-4 md:mx-8 max-w-xl md:max-w-xl max-h-[500px] md:max-h-[600px] mt-8 md:mt-0">
			<img
				src="abbel_figure1.png"
				alt="ABBEL framework overview"
				class="object-contain h-full w-full"
			/>
		</div>
	</div>
	<div class="w-full border-b border-gray-300"></div>
	<!-- END HEADER -->

	<!-- START INTRO/ABSTRACT-->
	<Block padding="pb-12 pt-10">
		<h1 class="mb-4 text-2xl font-bold" id="intro">Introduction</h1>
		<p class="pb-4">
			As the length of multi-step interactive language tasks increases, it becomes computationally 
			impractical to keep full interaction histories in context. We propose a general and interpretable 
			approach: <strong>Acting through Belief Bottlenecks Expressed in Language (ABBEL)</strong>, 
			which replaces long multi-step interaction history by a belief state, i.e., a natural language 
			summary of what has been discovered about task-relevant unknowns.
		</p>
		<p class="pb-4">
			Under ABBEL, at each step the agent first updates the prior belief with the most recent 
			observation from the environment, then uses only the updated posterior belief to select 
			an action. We systematically evaluate frontier models under ABBEL across six diverse 
			multi-step environments, finding that they can generate interpretable beliefs while 
			maintaining near-constant memory use over interaction steps.
		</p>
		<p>
			We train LLMs to generate and act on beliefs within the ABBEL framework via reinforcement 
			learning (RL). We devise a scheme for belief grading that associates rewards with the 
			quality of the beliefs, and demonstrate its ability to improve ABBEL's performance beyond 
			full context RL in a sandbox Combination Lock setting. Our findings indicate that ABBEL 
			opens avenues to study issues and potential remedies for agents in long context settings.
		</p>
	</Block>
	<!-- END SETUP -->

	<!-- START FRAMEWORK -->
	<Block>
		<h1 class="mb-4 text-2xl font-bold" id="framework">The ABBEL Framework</h1>
		<p class="pb-4">
			ABBEL alternates between updating a belief state given new observations, and selecting 
			an action based solely on the current belief. Thus, ABBEL relies on the ability of the 
			language agents to propagate the correct information at each step: they must maintain 
			sufficient information for selecting good actions, while discarding superfluous information 
			to keep the context length manageable.
		</p>
	</Block>
	<Block size="max-w-3xl" padding="pb-0">
		<img src="abbel_wordle_example.png" alt="ABBEL Wordle example" class="w-full h-full" />
	</Block>
	<Block size="max-w-4xl" class="text-base text-gray-600">
		<em><strong>Example step of ABBEL in Wordle.</strong></em> Actions are word guesses, and 
		observations provide feedback on each letter of the guess. The agent updates its belief 
		state based on feedback, then selects the next action using only the updated belief.
	</Block>
	<!-- END FRAMEWORK -->

	<!-- START EVALUATION -->
	<Block>
		<h1 class="mb-4 text-2xl font-bold" id="evaluation">Evaluating Frontier Models</h1>
		<p class="pb-4">
			We systematically investigate to what extent frontier models can generate and reason 
			through natural language belief states as bottlenecks in reasoning. We evaluate across 
			six multi-step environments with varying levels of reasoning complexity and structure, 
			and compare with standard multi-step interaction (Vanilla) and prompting for belief generation
			before action selection while maintaining the trajectory in context (Belief prompting).
		</p>
	</Block>
	<Block size="max-w-5xl" padding="pb-0">
		<img
			src="abbel_figure2.png"
			alt="Frontier model performance"
			class="w-full h-full"
		/>
	</Block>
	<Block size="max-w-4xl" class="text-base text-gray-600">
		<em><strong>Performance of frontier models under each framework.</strong></em> Frontier models generally
		struggle to generate fully sufficient belief states across all environments, as indicated by the 
		performance drops under ABBEL compared to the other frameworks which keep the full trajectory in context.
	</Block>

	<Block>
		<p class="pb-4">
			We find that belief states generated by prompting frontier models are generally intelligible 
			and significantly shorter than the full interaction history. While the history grows linearly 
			with interaction steps, the belief lengths generally grow much more slowly, even decreasing in some 
			environments as possibilities are ruled out. However, the model that performed the best, Gemini 2.5 Pro,
			also generated the longest and most inefficient belief states. Thus, no model 
			generated beliefs that were both sufficient and compact across all environments.
		</p>
	</Block>
	<Block size="max-w-5xl" padding="pb-0">
		<img src="abbel_figure3.png" alt="Belief length comparison" class="w-full h-full" />
	</Block>
	<Block size="max-w-4xl" class="text-base text-gray-600">
		<em><strong>Belief states are significantly more compact than full histories.</strong></em> 
		Average length of beliefs generated under ABBEL at each interaction step, compared to the full interaction 
		history lengths.
	</Block>
	<!-- END EVALUATION -->

	<!-- START RL -->
	<Block>
		<h1 class="mb-4 text-2xl font-bold" id="rl">Reinforcement Learning for Belief Bottlenecks</h1>
		<p class="pb-4">
			Though frontier models faced a performance drop under ABBEL, there is a significant 
			divergence between ABBEL and typical LLM pre-training settings. We propose using 
			Reinforcement Learning (RL) to improve LLMs' ability to generate and reason through 
			belief bottlenecks.
		</p>
	</Block>
	<Block size="max-w-5xl" padding="pb-0">
		<div class="flex flex-col md:flex-row gap-4">
			<div class="flex-1">
				<img
					src="abbel_figure5a.png"
					alt="Test success rate"
					class="w-full h-full"
				/>
			</div>
			<div class="flex-1">
				<img
					src="abbel_figure5b.png"
					alt="Cumulative regret"
					class="w-full h-full"
				/>
			</div>
		</div>
	</Block>
	<Block size="max-w-4xl" class="text-base text-gray-600">
		<em><strong>RL training improves ABBEL performance.</strong></em> Left: Test success 
		rates over training steps show ABBEL quickly reduces its performance gap. Right: With 
		belief grading, ABBEL outperforms models with access to the full history.
	</Block>

	<Block>
		<h2 class="text-xl font-semibold mb-3">Belief Grading</h2>
		<p class="pb-4">
			We propose to take advantage of ABBEL's isolated belief states to provide additional 
			signal by grading the quality of the generated beliefs. We find that belief grading 
			is highly effective, resulting in ABBEL outperforming both vanilla and belief prompting 
			across Success Rate and Regret metrics.
		</p>
	</Block>
	<!-- END RL -->

	<!-- START RESULTS -->
	<Block>
		<h1 class="mb-4 text-2xl font-bold" id="results">Multi-Objective Question Answering</h1>
		<p class="pb-4">
			We train in a realistic setting with extreme horizon generalization. We compare with 
			MEM1, which also uses RL to train LLMs to generate and act on context summaries.
		</p>
	</Block>
	<Block size="max-w-5xl" padding="pb-0">
		<img src="abbel_figure7.png" alt="Multi-objective QA results" class="w-full h-full" />
	</Block>
	<Block size="max-w-4xl" class="text-base text-gray-600">
		<em><strong>ABBEL achieves superior performance and memory efficiency.</strong></em> 
		ABBEL performs best for 4+ objectives, while training with a belief length penalty 
		(ABBEL-Length-Penalty) still outperforms other models while using the least memory.
	</Block>
	<!-- END RESULTS -->

	<!-- START CITATION -->
	<Block>
		<h1 class="mb-4 text-2xl font-bold" id="citation">Citation</h1>
		<div
			class="font-mono text-xs mt-4 bg-gray-100 px-4 py-4 border-2 border-solid border-gray-200 rounded-md w-full"
		>
			<pre class="whitespace-pre-wrap">
{`@article{anonymous2025abbel,
	title={ABBEL: LLM Agents Acting through Belief Bottlenecks Expressed in Language},
	author={Anonymous},
	journal={Preprint. Under review.},
	year={2025}
}`}</pre>
		</div>
	</Block>
	<!-- END CITATION -->

	<!-- START FOOTER -->
	<div class="flex w-full justify-center pt-4 mt-8 font-sans bg-gray-100">
		<div class="mx-8 max-w-3xl w-full">
			<div class="text-sm text-gray-600 mb-24">
				This website is licensed under a <a
					class="underline"
					href="http://creativecommons.org/licenses/by-sa/4.0/"
					target="_blank"
					rel="noreferrer">Creative Commons Attribution-ShareAlike 4.0 International License</a
				>. This means you are free to borrow the source code of this website, we just ask that you
				link back to this page in the footer. This website is based on the
				<a class="underline" href="https://obfuscated-activations.github.io/" target="_blank" rel="noreferrer"
					>Obfuscated Activations website</a
				>.
			</div>
		</div>
	</div>
	<!-- END FOOTER -->
</div>
