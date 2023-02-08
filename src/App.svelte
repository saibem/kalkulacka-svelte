<script lang="ts">
	import {UserInputData, calculatorData} from './data'
	import MySelect from "./components/MySelect.svelte";
	import CalculatorResult from "./components/CalculatorResult.svelte";

	let userInput = {} as UserInputData;

	const calculateWaterIntake = (userInput: UserInputData) => {
		let totalSumOfLitres = 0;

		calculatorData.forEach((question) => {
			let chosenOption = question.options.find(
					(option) => option.id === userInput[question.type]
			);
			if (!chosenOption) return;
			totalSumOfLitres = chosenOption.fn(userInput, totalSumOfLitres);
		});
		return totalSumOfLitres;
	};



	const updateData = (questionType, value) => {
		userInput[questionType] = value
	};


</script>

<main>
	<div class="Container">
		<h1>Kalkulačka pitného režimu</h1>
		<div class="Wrapper">
			<p>Zadejte vaši váhu</p>
			<input
					type="range"
					min="1"
					max="200"
					bind:value={userInput.weight}
					on:change={(event) => updateData("weight", +event.target.value)}
			/>
			{#if userInput.weight}
				<p>{userInput.weight} kg.</p>
				{/if}
		</div>

		{#each calculatorData as questionSelector}
			{#if Object.keys(userInput).includes(questionSelector.dependency)}
				<div class="Wrapper">
					<p>{questionSelector.question}</p>
					<MySelect
							   options={questionSelector.options.map((question) => ({
            value: question.id.toString(),
            label: question.label,
          }))}
							   onChange={(value) =>
            updateData(questionSelector.type, parseInt(value))
          }
					/>
				</div>
			{/if}
		{/each}
		<div class="Result">
			{#if Object.keys(userInput).length > calculatorData.length}
					<CalculatorResult totalLitres={calculateWaterIntake(userInput)} />
			{/if}
		</div>
	</div>
</main>

<style>
	.Container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.Wrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 1em;
	}

	input[type="range"] {
		width: 100%;
	}

	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>