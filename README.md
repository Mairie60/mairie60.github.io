# mairie60.github.io
Demande de subvention
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Budget Subvention</title>

<style>
body {
    font-family: Arial;
    margin: 20px;
    background: #eef2f7;
}

h2 {
    text-align: center;
    margin-top: 30px;
    font-size: 1.1em;
    letter-spacing: 2px;
    padding: 10px 0;
    border-radius: 6px;
    color: white;
}

h2.titre-recettes {
    background: linear-gradient(90deg, #1b5e20, #43a047);
}

h2.titre-depenses {
    background: linear-gradient(90deg, #b71c1c, #e53935);
}

#rapport {
    background: white;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 3px 15px rgba(0,0,0,0.13);
}

table {
    width: 100%;
    border-collapse: collapse;
    table-layout: fixed;
    margin-bottom: 5px;
}

th, td {
    border: 1px solid #ccc;
    padding: 5px;
    height: 35px;
}

#recettes th {
    background: #1b5e20;
    color: white;
    font-size: 0.85em;
    text-align: center;
}

#depenses th {
    background: #b71c1c;
    color: white;
    font-size: 0.85em;
    text-align: center;
}

#recettes .label {
    background: #e8f5e9;
    font-weight: bold;
    color: #1b5e20;
}

#depenses .label {
    background: #ffebee;
    font-weight: bold;
    color: #b71c1c;
}

#recettes tr:nth-child(even) td:not(.label) { background: #f1f8e9; }
#recettes tr:nth-child(odd)  td:not(.label) { background: #ffffff; }
#depenses tr:nth-child(even) td:not(.label) { background: #fff8f8; }
#depenses tr:nth-child(odd)  td:not(.label) { background: #ffffff; }

#recettes .separator-top td {
    border-top: 3px solid #2e7d32;
    background: #a5d6a7 !important;
    font-weight: bold;
    color: #1b5e20;
}

#depenses tr.separator-top:first-of-type td {
    border-top: 3px solid #c62828;
    background: #ffcdd2 !important;
    font-weight: bold;
    color: #b71c1c;
}

#depenses tr.separator-top:last-of-type td {
    border-top: 3px solid #1565c0;
    background: #bbdefb !important;
    font-weight: bold;
    color: #0d47a1;
}

input {
    width: 100%;
    height: 100%;
    border: none;
    text-align: right;
    outline: none;
    background: transparent;
    font-size: 0.95em;
}

td input[type="text"] {
    text-align: left;
}

.locked {
    background: #e0e0e0 !important;
    pointer-events: none;
    color: #555;
}

button {
    margin: 8px 5px;
    padding: 9px 16px;
    cursor: pointer;
    border: none;
    border-radius: 6px;
    font-size: 0.9em;
    font-weight: bold;
    transition: opacity 0.2s, transform 0.1s;
}

button:hover {
    opacity: 0.88;
    transform: translateY(-1px);
}

button[onclick*="addCustomRow('recettes')"] {
    background: #2e7d32;
    color: white;
}

button[onclick*="addCustomRow('depenses')"] {
    background: #c62828;
    color: white;
}

button[onclick="calc()"] {
    background: #1565c0;
    color: white;
}

button[onclick="exportPDF()"] {
    background: #6a1b9a;
    color: white;
}
</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

</head>

<body>

<div id="rapport">

<h2 class="titre-recettes">🟢 RECETTES</h2>

<table id="recettes">
<tr>
    <th> </th>
    <th>2025 ou saison 24/25</th>
    <th>2026 ou saison 25/26</th>
    <th>Prévisionnel 2027 ou saison 26/27</th>
</tr>

<tr>
    <td class="label locked">Report excédentaire</td>
    <td><input id="rep1" type="number" style="background:white;pointer-events:auto;text-align:right;border:none;width:100%;height:100%;outline:none;"></td>
    <td><input id="rep2" class="locked"></td>
    <td><input id="rep3" class="locked"></td>
</tr>

<tr>
    <td class="label locked">État</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr>
    <td class="label locked">Région</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr>
    <td class="label locked">Département</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr>
    <td class="label locked">Commune de Hermes</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr>
    <td class="label locked">Cotisation / Adhésion</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr class="separator-top">
    <td class="label locked">TOTAL RECETTES</td>
    <td><input id="r1" class="locked"></td>
    <td><input id="r2" class="locked"></td>
    <td><input id="r3" class="locked"></td>
</tr>
</table>

<button onclick="addCustomRow('recettes')">➕ Ajouter une recette</button>
<button onclick="calc()">🧮 Calculer</button>

<h2 class="titre-depenses">🔴 DÉPENSES</h2>

<table id="depenses">
<tr>
    <th> </th>
    <th>2025 ou saison 24/25</th>
    <th>2026 ou saison 25/26</th>
    <th>Prévisionnel 2027 ou saison 26/27</th>
</tr>

<tr>
    <td class="label locked">Frais de personnel</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr>
    <td class="label locked">Frais de gestion</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr>
    <td class="label locked">Frais immobiliers</td>
    <td><input type="number"></td>
    <td><input type="number"></td>
    <td><input type="number"></td>
</tr>

<tr class="separator-top">
    <td class="label locked">TOTAL DÉPENSES</td>
    <td><input id="d1" class="locked"></td>
    <td><input id="d2" class="locked"></td>
    <td><input id="d3" class="locked"></td>
</tr>

<tr class="separator-top">
    <td class="label locked">SOLDE DE L'ANNÉE</td>
    <td><input id="s1" class="locked"></td>
    <td><input id="s2" class="locked"></td>
    <td><input id="s3" class="locked"></td>
</tr>
</table>

<button onclick="addCustomRow('depenses')">➕ Ajouter une dépense</button>

<br><br>
<button onclick="calc()">🧮 Calculer</button>
<button onclick="exportPDF()">📄 Export PDF</button>

</div>

<script>

function addCustomRow(tableId) {
    let name = prompt("Nom de la ligne ?");
    if (!name) return;

    let table = document.getElementById(tableId);
    let lastRowIndex = table.rows.length - 2;

    let row = table.insertRow(lastRowIndex);

    row.innerHTML = `
        <td class="label">${name}</td>
        <td><input type="number"></td>
        <td><input type="number"></td>
        <td><input type="number"></td>
    `;
}

/* 🔢 CALCUL MANUEL UNIQUEMENT */
function calc() {

    /* ── PASSE 1 : calcul de l'année 2025 ── */
    let recettes = calcTable('recettes', ['r1','r2','r3']);
    let depenses = calcTable('depenses', ['d1','d2','d3']);

    // Solde 2025
    let solde2025 = recettes[0] - depenses[0];
    document.getElementById('s1').value = solde2025.toFixed(2);

    /* 🔥 Report 2025 → 2026 */
    document.getElementById('rep2').value = solde2025.toFixed(2);

    /* ── PASSE 2 : recalcul avec report 2026 intégré ── */
    recettes = calcTable('recettes', ['r1','r2','r3']);
    depenses = calcTable('depenses', ['d1','d2','d3']);

    // Solde 2026
    let solde2026 = recettes[1] - depenses[1];
    document.getElementById('s2').value = solde2026.toFixed(2);

    /* 🔥 Report 2026 → 2027 */
    document.getElementById('rep3').value = solde2026.toFixed(2);

    /* ── PASSE 3 : recalcul avec report 2027 intégré ── */
    recettes = calcTable('recettes', ['r1','r2','r3']);
    depenses = calcTable('depenses', ['d1','d2','d3']);

    // Solde 2027
    let solde2027 = recettes[2] - depenses[2];
    document.getElementById('s3').value = solde2027.toFixed(2);
}

function calcTable(id, targets) {
    let table = document.getElementById(id);
    let sums = [0, 0, 0]; // 2025, 2026, 2027

    // On commence à 1 pour ignorer l'en-tête
    for (let i = 1; i < table.rows.length; i++) {
        let row = table.rows[i];

        // On récupère le texte de la première colonne uniquement
        let label = row.cells[0]?.innerText || "";

        // On ignore TOTAL et SOLDE
        if (label.includes("TOTAL") || label.includes("SOLDE")) continue;

        // 🔥 On lit directement les cellules 1,2,3 (au lieu de querySelectorAll)
        for (let j = 1; j <= 3; j++) {
            let input = row.cells[j]?.querySelector("input");

            if (input) {
                let val = parseFloat(input.value);
                if (!isNaN(val)) {
                    sums[j - 1] += val;
                }
            }
        }
    }

    // On met à jour les totaux
    for (let i = 0; i < 3; i++) {
        document.getElementById(targets[i]).value = sums[i].toFixed(2);
    }

    return sums;
}

function exportPDF() {
    const element = document.getElementById("rapport");

    if (typeof html2pdf === "undefined") {
        alert("Erreur : html2pdf non chargé");
        return;
    }

    html2pdf()
        .from(element)
        .set({
            margin: 10,
            filename: 'rapport_subvention.pdf',
            image: { type: 'jpeg', quality: 0.98 },
            html2canvas: { scale: 2, useCORS: true },
            jsPDF: { unit: 'mm', format: 'a4', orientation: 'landscape' }
        })
        .save();
}

</script>

</body>
</html>
