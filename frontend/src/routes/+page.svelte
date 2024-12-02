<script>
    import { onMount } from 'svelte';
  
    let resume = '';
    let jobRequirements = '';
    let result = '';
    let isLoading = false;
  
    async function handleSubmit() {
      isLoading = true;
      try {
        const response = await fetch('http://localhost:8080/api/match-resume', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ resume, jobRequirements }),
        });
        
        if (!response.ok) {
          throw new Error('Failed to process the request');
        }
        
        const data = await response.json();
        result = data.result;
      } catch (error) {
        console.error('Error:', error);
        result = 'An error occurred while processing your request.';
      } finally {
        isLoading = false;
      }
    }
  
    onMount(() => {
      // Any initialization code can go here
    });
  </script>
  
  <main class="container mx-auto p-4 max-w-2xl">
    <h1 class="text-3xl font-bold mb-6 text-center">Resume Matcher</h1>
    
    <form on:submit|preventDefault={handleSubmit} class="space-y-4">
      <div>
        <label for="resume" class="block mb-2 font-medium">Your Resume</label>
        <textarea
          id="resume"
          bind:value={resume}
          class="w-full h-40 p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500"
          placeholder="Paste your resume here..."
        ></textarea>
      </div>
      
      <div>
        <label for="jobRequirements" class="block mb-2 font-medium">Job Requirements</label>
        <textarea
          id="jobRequirements"
          bind:value={jobRequirements}
          class="w-full h-40 p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500"
          placeholder="Paste the job requirements here..."
        ></textarea>
      </div>
      
      <button
        type="submit"
        class="w-full py-2 px-4 bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75"
        disabled={isLoading}
      >
        {isLoading ? 'Processing...' : 'Submit'}
      </button>
    </form>
    
    {#if result}
      <div class="mt-6 p-4 bg-gray-100 rounded-md">
        <h2 class="text-xl font-semibold mb-2">Result:</h2>
        <p>{result}</p>
      </div>
    {/if}
  </main>
  
  <style global lang="postcss">
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
  </style>