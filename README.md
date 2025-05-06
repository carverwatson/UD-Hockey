function calculateGameScore(stats) {
    const baseScore = (2 * stats.goals) + (1 * stats.assists) +
                      (0.5 * stats.blocks) - (0.2 * stats.chancesAgainst) +
                      (0.1 * stats.corsiFor) - (0.1 * stats.corsiAgainst) +
                      (0.15 * stats.fenwickFor) - (0.15 * stats.fenwickAgainst) +
                      (0.3 * stats.chancesFor) - (0.3 * stats.chancesAgainst);
    return stats.toi > 0 ? (5 * (60 / stats.toi) * baseScore).toFixed(2) : 0;
}

function calculateMetrics(stats) {
    const pointsPer60 = stats.toi > 0 ? ((stats.goals + stats.assists) * (60 / stats.toi)).toFixed(2) : 0;
    const corsiPercent = (stats.corsiFor + stats.corsiAgainst) > 0 ? ((stats.corsiFor / (stats.corsiFor + stats.corsiAgainst)) * 100).toFixed(1) : 0;
    const fenwickPercent = (stats.fenwickFor + stats.fenwickAgainst) > 0 ? ((stats.fenwickFor / (stats.fenwickFor + stats.fenwickAgainst)) * 100).toFixed(1) : 0;
    return { pointsPer60, corsiPercent, fenwickPercent };
}

function saveGameData(playerId) {
    const stats = {
        date: document.getElementById('gameDate').value,
        goals: parseInt(document.getElementById('goals').value),
        assists: parseInt(document.getElementById('assists').value),
        toi: parseFloat(document.getElementById('toi').value),
        blocks: parseInt(document.getElementById('blocks').value),
        chancesFor: parseInt(document.getElementById('chancesFor').value),
        chancesAgainst: parseInt(document.getElementById('chancesAgainst').value),
        corsiFor: parseInt(document.getElementById('corsiFor').value),
        corsiAgainst: parseInt(document.getElementById('corsiAgainst').value),
        fenwickFor: parseInt(document.getElementById('fenwickFor').value),
        fenwickAgainst: parseInt(document.getElementById('fenwickAgainst').value)
    };
    const games = JSON.parse(localStorage.getItem(playerId) || '[]');
    games.push(stats);
    localStorage.setItem(playerId, JSON.stringify(games));
}

function loadPlayerData(playerId) {
    const games = JSON.parse(localStorage.getItem(playerId) || '[]');
    const tableBody = document.getElementById('gameTableBody');
    tableBody.innerHTML = '';
    let totalGameScore = 0, totalPointsPer60 = 0, totalCorsiPercent = 0, totalFenwickPercent = 0;
    games.forEach(game => {
        const gameScore = calculateGameScore(game);
        const metrics = calculateMetrics(game);
        const row = `<tr>
            <td class="border p-2">${game.date}</td>
            <td class="border p-2">${gameScore}</td>
            <td class="border p-2">${metrics.pointsPer60}</td>
            <td class="border p-2">${metrics.corsiPercent}%</td>
            <td class="border p-2">${metrics.fenwickPercent}%</td>
        </tr>`;
        tableBody.insertAdjacentHTML('beforeend', row);
        totalGameScore += parseFloat(gameScore);
        totalPointsPer60 += parseFloat(metrics.pointsPer60);
        totalCorsiPercent += parseFloat(metrics.corsiPercent);
        totalFenwickPercent += parseFloat(metrics.fenwickPercent);
    });
    const gameCount = games.length;
    document.getElementById('seasonGameScore').textContent = `Average Game Score: ${gameCount > 0 ? (totalGameScore / gameCount).toFixed(2) : 0}`;
    document.getElementById('seasonPointsPer60').textContent = `Average Points per 60: ${gameCount > 0 ? (totalPointsPer60 / gameCount).toFixed(2) : 0}`;
    document.getElementById('seasonCorsiPercent').textContent = `Average Corsi%: ${gameCount > 0 ? (totalCorsiPercent / gameCount).toFixed(1) : 0}%`;
    document.getElementById('seasonFenwickPercent').textContent = `Average Fenwick%: ${gameCount > 0 ? (totalFenwickPercent / gameCount).toFixed(1) : 0}%`;
}
