# Arjun-repertory-
Homeopathic 
<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HomeoPro - Advanced Report Tree</title>
    <style>
        :root { --primary: #2d6a4f; --secondary: #40916c; --bg: #f8f9fa; }
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background: var(--bg); margin: 0; padding: 20px; }
        .container { max-width: 800px; margin: auto; background: white; padding: 30px; border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); }
        .header { text-align: center; border-bottom: 2px solid var(--primary); margin-bottom: 25px; padding-bottom: 10px; }
        .search-box { margin-bottom: 30px; }
        input[type="text"] { width: 100%; padding: 15px; border: 2px solid #ddd; border-radius: 10px; font-size: 16px; box-sizing: border-box; }
        .tree-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
        select { width: 100%; padding: 12px; border-radius: 8px; border: 1px solid #ccc; background: white; }
        .report-card { margin-top: 30px; display: none; padding: 25px; border-radius: 15px; background: #fff; border-left: 10px solid var(--primary); box-shadow: 0 5px 15px rgba(0,0,0,0.1); }
        .miasm-tag { background: #e63946; color: white; padding: 3px 10px; border-radius: 5px; font-size: 0.8em; }
        .btn-wa { display: block; background: #25D366; color: white; text-align: center; padding: 15px; border-radius: 10px; text-decoration: none; font-weight: bold; margin-top: 20px; transition: 0.3s; }
        .btn-wa:hover { background: #128C7E; }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>🌿 HomeoPro Repertory</h1>
        <p>Advance Symptom & Miasm Finder</p>
    </div>

    <div class="search-box">
        <label>🔍 Symptom Search (Type karein):</label>
        <input type="text" id="searchInput" onkeyup="instantSearch()" placeholder="Jaise: Headache, Warts, Anxiety...">
        <div id="searchSuggestions"></div>
    </div>

    <div class="tree-grid">
        <div>
            <label>1. Body Region (Ang):</label>
            <select id="region" onchange="loadSymptoms()">
                <option value="">-- Select --</option>
                <option value="Mind">Mind (Mann)</option>
                <option value="Head">Head (Sir)</option>
                <option value="Stomach">Stomach (Pet)</option>
                <option value="Skin">Skin (Tavacha)</option>
                <option value="Extremities">Extremities (Hath-Paon)</option>
            </select>
        </div>
        <div>
            <label>2. Specific Symptom:</label>
            <select id="symptom" onchange="generateReport()">
                <option value="">-- Pehle Ang Chunein --</option>
            </select>
        </div>
    </div>

    <div id="reportArea" class="report-card">
        <h3 style="margin-top:0;">📋 Professional Report</h3>
        <p><strong>Symptom:</strong> <span id="resSymp"></span></p>
        <p><strong>Medicine:</strong> <span id="resMed" style="color:var(--primary); font-size:1.2em; font-weight:bold;"></span></p>
        <p><strong>Miasm:</strong> <span id="resMiasm" class="miasm-tag"></span></p>
        <p><strong>Common Potency:</strong> 30CH or 200CH</p>
        <hr>
        <small style="color:gray;">*Note: Please consult a Homeopathic Physician.</small>
        <a id="waLink" href="#" target="_blank" class="btn-wa">WhatsApp par Share karein 📱</a>
    </div>
</div>

<script>
    // Bada Database (Ismein aap hazaron entries add kar sakte hain)
    const repo = {
        "Mind": {
            "Anxiety about health": { med: "Arsenic Album", miasm: "Psora" },
            "Fear of public speaking": { med: "Gelsemium", miasm: "Psora" },
            "Very Irritable/Angry": { med: "Nux Vomica", miasm: "Psora/Sycosis" }
        },
        "Head": {
            "Right sided Migraine": { med: "Sanguinaria", miasm: "Psora" },
            "Left sided Migraine": { med: "Spigelia", miasm: "Psora" },
            "Hair fall in patches": { med: "Phosphorus", miasm: "Syphilitic" }
        },
        "Stomach": {
            "Burning in chest": { med: "Iris Vers", miasm: "Psora" },
            "Stone pain (Kidney)": { med: "Berberis Vulgaris", miasm: "Sycosis" },
            "Piles with bleeding": { med: "Hamamelis", miasm: "Syphilitic" }
        },
        "Skin": {
            "Warts on hands": { med: "Thuja", miasm: "Sycosis" },
            "Psoriasis dry": { med: "Arsenic Iod", miasm: "Psora/Syphilis" },
            "Urticaria/Hives": { med: "Apis Mellifica", miasm: "Psora" }
        },
        "Extremities": {
            "Heel pain (morning)": { med: "Rhus Tox", miasm: "Psora" },
            "Sciatica pain": { med: "Colocynth", miasm: "Psora/Sycosis" }
        }
    };

    function loadSymptoms() {
        const region = document.getElementById("region").value;
        const sympSelect = document.getElementById("symptom");
        sympSelect.innerHTML = '<option value="">-- Select Symptom --</option>';
        if (region) {
            for (let s in repo[region]) {
                let opt = document.createElement("option");
                opt.value = s; opt.innerHTML = s;
                sympSelect.appendChild(opt);
            }
        }
    }

    function generateReport() {
        const region = document.getElementById("region").value;
        const symp = document.getElementById("symptom").value;
        if (symp) {
            const data = repo[region][symp];
            document.getElementById("resSymp").innerText = symp;
            document.getElementById("resMed").innerText = data.med;
            document.getElementById("resMiasm").innerText = data.miasm;

            let msg = `*-- HOMEOPATHY REPORT --*%0A*Symptom:* ${symp}%0A*Medicine:* ${data.med}%0A*Miasm:* ${data.miasm}%0A%0A_Get well soon!_`;
            document.getElementById("waLink").href = "https://wa.me/?text=" + msg;
            document.getElementById("reportArea").style.display = "block";
        }
    }

    // Quick Search Logic
    function instantSearch() {
        let input = document.getElementById("searchInput").value.toLowerCase();
        if(input.length > 2) {
            for(let reg in repo) {
                for(let s in repo[reg]) {
                    if(s.toLowerCase().includes(input)) {
                        document.getElementById("region").value = reg;
                        loadSymptoms();
                        document.getElementById("symptom").value = s;
                        generateReport();
                        return;
                    }
                }
            }
        }
    }
</script>
</body>
</html>
