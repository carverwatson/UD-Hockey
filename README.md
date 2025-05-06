@@ -1 +1,117 @@
 # UD-Hockey
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Carver Watson - Hockey Analytics</title>
     <script src="https://cdn.tailwindcss.com"></script>
 </head>
 <body class="bg-gray-100 font-sans">
     <header class="bg-blue-600 text-white py-6 text-center">
         <h1 class="text-3xl font-bold">Hockey Analytics by Carver W. Watson</h1>
         <p class="text-lg mt-2">Game Score Calculator for Player Evaluation</p>
     </header>
     <main class="max-w-4xl mx-auto py-8 px-4">
         <section class="bg-white shadow-lg rounded-lg p-6">
             <h2 class="text-2xl font-semibold mb-4">Input Player Statistics</h2>
             <p class="text-gray-700 mb-4">Enter game stats to calculate a custom Game Score, incorporating Corsi, Fenwick, and scoring chances.</p>
             <form id="statsForm" class="space-y-4">
                 <div>
                     <label class="block text-gray-700">Player Name:</label>
                     <input type="text" id="playerName" class="w-full p-2 border rounded" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Goals:</label>
                     <input type="number" id="goals" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Assists:</label>
                     <input type="number" id="assists" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Time on Ice (minutes):</label>
                     <input type="number" id="toi" class="w-full p-2 border rounded" min="0" step="0.1" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Blocked Shots:</label>
                     <input type="number" id="blocks" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Scoring Chances For:</label>
                     <input type="number" id="chancesFor" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Scoring Chances Against:</label>
                     <input type="number" id="chancesAgainst" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Corsi For (shot attempts):</label>
                     <input type="number" id="corsiFor" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Corsi Against (shot attempts):</label>
                     <input type="number" id="corsiAgainst" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Fenwick For (unblocked attempts):</label>
                     <input type="number" id="fenwickFor" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <div>
                     <label class="block text-gray-700">Fenwick Against (unblocked attempts):</label>
                     <input type="number" id="fenwickAgainst" class="w-full p-2 border rounded" min="0" value="0" required>
                 </div>
                 <button type="submit" class="bg-blue-600 text-white py-2 px-4 rounded hover:bg-blue-700">Calculate Game Score</button>
             </form>
             <div id="results" class="mt-6 hidden">
                 <h3 class="text-xl font-semibold">Analysis Results</h3>
                 <p id="playerResult" class="text-gray-700"></p>
                 <p id="gameScoreResult" class="text-gray-700 font-bold"></p>
                 <p id="pointsPer60Result" class="text-gray-700"></p>
                 <p id="corsiPercentResult" class="text-gray-700"></p>
                 <p id="fenwickPercentResult" class="text-gray-700"></p>
             </div>
         </section>
     </main>
     <footer class="bg-gray-800 text-white text-center py-4">
         <p>Contact: <a href="mailto:carver.watson@gmail.com" class="hover:underline">carver.watson@gmail.com</a> | (920) 287-4014</p>
         <p><a href="https://linkedin.com/in/carverwatson" target="_blank" class="hover:underline">LinkedIn Profile</a></p>
     </footer>
     <script>
         document.getElementById('statsForm').addEventListener('submit', function(e) {
             e.preventDefault();
             const name = document.getElementById('playerName').value;
             const goals = parseInt(document.getElementById('goals').value);
             const assists = parseInt(document.getElementById('assists').value);
             const toi = parseFloat(document.getElementById('toi').value);
             const blocks = parseInt(document.getElementById('blocks').value);
             const chancesFor = parseInt(document.getElementById('chancesFor').value);
             const chancesAgainst = parseInt(document.getElementById('chancesAgainst').value);
             const corsiFor = parseInt(document.getElementById('corsiFor').value);
             const corsiAgainst = parseInt(document.getElementById('corsiAgainst').value);
             const fenwickFor = parseInt(document.getElementById('fenwickFor').value);
             const fenwickAgainst = parseInt(document.getElementById('fenwickAgainst').value);
 
             // Calculate Game Score
             const baseScore = (2 * goals) + (1 * assists) +
                               (0.5 * blocks) - (0.2 * chancesAgainst) +
                               (0.1 * corsiFor) - (0.1 * corsiAgainst) +
                               (0.15 * fenwickFor) - (0.15 * fenwickAgainst) +
                               (0.3 * chancesFor) - (0.3 * chancesAgainst);
             const gameScore = toi > 0 ? (5 * (60 / toi) * baseScore).toFixed(2) : 0;
 
             // Additional Metrics
             const pointsPer60 = toi > 0 ? ((goals + assists) * (60 / toi)).toFixed(2) : 0;
             const corsiPercent = (corsiFor + corsiAgainst) > 0 ? ((corsiFor / (corsiFor + corsiAgainst)) * 100).toFixed(1) : 0;
             const fenwickPercent = (fenwickFor + fenwickAgainst) > 0 ? ((fenwickFor / (fenwickFor + fenwickAgainst)) * 100).toFixed(1) : 0;
 
             // Display Results
             document.getElementById('playerResult').textContent = `Player: ${name}`;
             document.getElementById('gameScoreResult').textContent = `Game Score: ${gameScore}`;
             document.getElementById('pointsPer60Result').textContent = `Points per 60 Minutes: ${pointsPer60}`;
             document.getElementById('corsiPercentResult').textContent = `Corsi%: ${corsiPercent}%`;
             document.getElementById('fenwickPercentResult').textContent = `Fenwick%: ${fenwickPercent}%`;
             document.getElementById('results').classList.remove('hidden');
         });
     </script>
 </body>
 </html>
