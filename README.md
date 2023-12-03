# Diamond.github.io
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title contenteditable="true">Diamond Price Calculator</title>
<style>
table {
    border-collapse: collapse;
    width: 100%;
}

table, th, td {
    border: 1px solid black;
}

th, td {
    padding: 10px;
    text-align: center;
}

#total-row {
    font-weight: bold;
}

@media only screen and (max-width: 600px) {
    button {
        width: 100%;
    }
}

@media print {
    button {
        display: none;
    }
}
</style>
</head>
<body>
<h2 contenteditable="true">Diamond Price Calculator</h2>
<table id="excel-table">
<thead> <tr>
    <th>No.</th>
    <th>Price</th>
    <th>Weight</th>
    <th>Total</th>
</tr>
</thead> <tbody>
<!-- Rows will be dynamically added here -->
</tbody> <tfoot> <tr id="total-row">
    <td>Total</td>
    <td id="total-column-1">0</td>
    <td id="total-column-2">0</td>
    <td id="grand-total">0</td>
</tr>
<tr id="average-row">
    <td>Average</td>
    <td id="average-column-1">0</td>
    <td id="average-column-2">0</td>
    <td id="overall-average">0</td>
</tr>
</tfoot>
</table>
<script>
let rowCount = 0;

function addRow() {
    rowCount++;

    const tableBody = document.querySelector('#excel-table tbody');
    const newRow = tableBody.insertRow(-1);

    const cell1 = newRow.insertCell(0);
    const cell2 = newRow.insertCell(1);
    const cell3 = newRow.insertCell(2);
    const cell4 = newRow.insertCell(3);

    cell1.textContent = rowCount;
    cell2.innerHTML = '<input type="text" class="column-input" oninput="calculateTotals()">';
    cell3.innerHTML = '<input type="text" class="column-input" oninput="calculateTotals()">';
    cell4.textContent = 0;

    calculateTotals();
    calculateAverages();
}

function calculateTotals() {
    const rows = document.querySelectorAll('#excel-table tbody tr');
    let totalColumn1 = 0;
    let totalColumn2 = 0;

    rows.forEach(row => {
        const cells = row.cells;
        const value1 = parseFloat(cells[1].querySelector('input').value) || 0;
        const value2 = parseFloat(cells[2].querySelector('input').value) || 0;

        totalColumn1 += value1;
        totalColumn2 += value2;

        cells[3].textContent = value1 + value2;
    });

    document.getElementById('total-column-1').textContent = totalColumn1;
    document.getElementById('total-column-2').textContent = totalColumn2;
    document.getElementById('grand-total').textContent = totalColumn1 + totalColumn2;

    calculateAverages();
}

function calculateAverages() {
    const rowCount = document.querySelectorAll('#excel-table tbody tr').length;
    const totalColumn1 = parseFloat(document.getElementById('total-column-1').textContent) || 0;
    const totalColumn2 = parseFloat(document.getElementById('total-column-2').textContent) || 0;

    document.getElementById('average-column-1').textContent = (totalColumn1 / rowCount).toFixed(2);
    document.getElementById('average-column-2').textContent = (totalColumn2 / rowCount).toFixed(2);

    const overallAverage = ((totalColumn1 + totalColumn2) / (rowCount * 2)).toFixed(2);
    document.getElementById('overall-average').textContent = overallAverage;
}

document.addEventListener('DOMContentLoaded', function() {
    for (let i = 0; i < 10; i++) {
        addRow();
    }
});

function togglePrintStyles() {
    const printStyles = document.createElement('style');
    printStyles.innerHTML = '@media print { button { display: none; } }';
    document.head.appendChild(printStyles);
    window.print();
    document.head.removeChild(printStyles);
}
</script>
<button onclick="addRow()">Add Row</button><br>
<br>
<button onclick="togglePrintStyles()">Print</button>
</body>
</html>
