fetch('resorts.json')
.then(res => res.json())
.then(data => renderResorts(data));

function renderResorts(resorts) {
const grid = document.getElementById("resortGrid");
grid.innerHTML = "";
resorts.forEach(resort => {
grid.innerHTML += `
<div class="bg-white p-6 rounded-xl shadow-sm">
<h3 class="text-xl font-bold text-gray-900">${resort.name}</h3>
<p class="text-sm text-gray-600">${resort.tier} • ADR: $${resort.adr} • Resonance: ${resort.resonance}</p>
<p class="text-sm text-gray-500">Airport: ${resort.airport} • Flights: ${resort.weeklyFlights}/week</p>
<p class="text-sm text-gray-500">Open Parcels: ${resort.openParcels} • Closed: ${resort.closedParcels}</p>
<div class="flex flex-wrap gap-2 mt-2">
${resort.tags.map(tag => `<span class="text-xs bg-gray-100 px-2 py-1 rounded-full">${tag}</span>`).join("")}
</div>
</div>
`;
});
}

function exportCSV() {
fetch('resorts.json')
.then(res => res.json())
.then(data => {
const csv = convertToCSV(data);
const blob = new Blob([csv], { type: 'text/csv' });
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'conquest_report.csv';
a.click();
});
}

function convertToCSV(data) {
const headers = Object.keys(data[0]);
const rows = data.map(obj => headers.map(header => `"${obj[header]}"`).join(","));
return [headers.join(","), ...rows].join("\n");
}
