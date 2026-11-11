<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChantierChrono - Pointage BTP Netlify</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen text-slate-800">

    <!-- Modal de Connexion Ouvrier (Sécurisé par code d'accès) -->
    <div id="ouvrier-login-modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl p-8 shadow-xl max-w-md w-full text-center">
            <div class="w-14 h-14 bg-amber-500 text-slate-950 rounded-2xl flex items-center justify-center text-2xl mx-auto mb-4 shadow-md font-bold">🔒</div>
            <h2 class="text-xl font-extrabold text-slate-900 mb-2">Accès Sécurisé ChantierChrono</h2>
            <p class="text-xs text-slate-500 mb-6">Veuillez entrer le code d'accès de l'équipe pour accéder à la plateforme.</p>
            <form onsubmit="verifyOuvrierLogin(event)" class="space-y-4">
                <input type="password" id="ouvrier-pass-input" placeholder="Code d'accès (ex: travauxgroupe)" required class="w-full text-center text-base bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 focus:outline-none focus:ring-2 focus:ring-amber-500 font-semibold">
                <button type="submit" class="w-full bg-amber-500 hover:bg-amber-600 text-slate-950 font-extrabold py-3.5 rounded-xl transition shadow-md">
                    Entrer sur le site
                </button>
            </form>
        </div>
    </div>

    <!-- Top Navbar Sombre -->
    <header class="bg-slate-900 border-b border-slate-800 px-6 py-4 shadow-md sticky top-0 z-40">
        <div class="max-w-5xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-amber-500 rounded-xl flex items-center justify-center text-white text-xl shadow-md">
                    🏗️
                </div>
                <div>
                    <h1 class="text-lg font-extrabold text-white tracking-wide">Chantier<span class="text-amber-400">Chrono</span></h1>
                    <p class="text-[10px] uppercase tracking-wider text-slate-400 font-semibold">POINTAGE BTP NETLIFY</p>
                </div>
            </div>
            
            <div class="flex items-center gap-2">
                <button onclick="switchTab('ouvrier')" id="btn-ouvrier" class="px-4 py-2 rounded-xl text-xs font-bold transition flex items-center gap-2 bg-amber-500 text-slate-950 shadow-sm">
                    👷‍♂️ Espace Ouvrier
                </button>
                <button onclick="switchTab('patron')" id="btn-patron" class="px-4 py-2 rounded-xl text-xs font-bold transition flex items-center gap-2 bg-slate-800 text-slate-300 hover:bg-slate-700">
                    👔 Espace Patron
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-5xl mx-auto p-4 sm:p-6 space-y-6">

        <!-- ONGLET OUVRIER -->
        <div id="tab-ouvrier" class="space-y-6">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 overflow-hidden">
                <div class="bg-slate-900 px-6 py-4 flex justify-between items-center text-white">
                    <div class="flex items-center gap-2.5">
                        <span class="text-amber-400 text-lg">📁</span>
                        <h2 class="font-bold text-base sm:text-lg">Nouveau Pointage de Chantier</h2>
                    </div>
                    <div id="date-badge" class="bg-slate-800 text-amber-400 border border-slate-700 px-3 py-1 rounded-xl text-xs font-bold"></div>
                </div>

                <form id="pointage-form" onsubmit="handlePointage(event)" class="p-6 sm:p-8 space-y-6">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                                <span class="text-amber-600">👤</span> Sélectionner votre Nom <span class="text-rose-500">*</span>
                            </label>
                            <select id="ouvrier-select" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-amber-500">
                                <option value="">-- Sélectionner votre nom --</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                                <span class="text-amber-600">📅</span> Date du chantier <span class="text-rose-500">*</span>
                            </label>
                            <input type="date" id="date-chantier" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-amber-500">
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                            <span class="text-amber-600">🏢</span> Nom du Chantier / Client <span class="text-rose-500">*</span>
                        </label>
                        <input type="text" id="chantier-nom" required placeholder="ex: Immeuble Le Grand Sud" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 pt-2">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                                <span class="text-emerald-600">➡️</span> Heure d'arrivée sur le chantier <span class="text-rose-500">*</span>
                            </label>
                            <div class="flex gap-2">
                                <input type="time" id="heure-arrivee" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-amber-500">
                                <button type="button" onclick="setNow('heure-arrivee')" class="px-4 py-2.5 bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold rounded-xl transition shadow-sm whitespace-nowrap">🕒 Maintenant</button>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                                <span class="text-rose-600">⬅️</span> Heure de départ du chantier <span class="text-rose-500">*</span>
                            </label>
                            <div class="flex gap-2">
                                <input type="time" id="heure-depart" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-amber-500">
                                <button type="button" onclick="setNow('heure-depart')" class="px-4 py-2.5 bg-rose-600 hover:bg-rose-700 text-white text-xs font-bold rounded-xl transition shadow-sm whitespace-nowrap">🕒 Maintenant</button>
                            </div>
                        </div>
                    </div>

                    <div class="bg-amber-50 border border-amber-200/80 rounded-xl p-3 text-amber-900 text-xs font-medium flex items-center gap-2">
                        <span>ℹ️</span> 30 minutes de pause repas sont automatiquement déduites du total des heures.
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                                <span class="text-amber-600">👥</span> Collègues travaillant avec vous
                            </label>
                            <input type="text" id="collegues" placeholder="ex: Paul, Thomas, Karim" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 mb-2 flex items-center gap-1.5">
                                <span class="text-amber-600">💬</span> Remarques / Matériel manquant / Avancement
                            </label>
                            <textarea id="remarques" rows="2" placeholder="ex: Coffrage terminé..." class="w-full bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500"></textarea>
                        </div>
                    </div>

                    <div class="pt-4">
                        <button type="submit" class="w-full bg-amber-500 hover:bg-amber-600 text-slate-950 font-extrabold py-4 px-6 rounded-2xl transition shadow-lg shadow-amber-500/20 text-base flex items-center justify-center gap-2">
                            <span>🚀</span> Valider et enregistrer mon pointage
                        </button>
                    </div>
                </form>
            </div>
        </div>

        <!-- ONGLET PATRON -->
        <div id="tab-patron" class="hidden space-y-6">
            <!-- Écran PIN -->
            <div id="patron-login" class="bg-white rounded-2xl p-8 shadow-sm border border-slate-200 max-w-md mx-auto text-center">
                <div class="w-14 h-14 bg-slate-900 text-amber-400 rounded-2xl flex items-center justify-center text-2xl mx-auto mb-4 shadow-md">🔐</div>
                <h3 class="text-lg font-bold text-slate-900 mb-2">Espace Sécurisé Patron</h3>
                <p class="text-xs text-slate-500 mb-6">Veuillez entrer votre code PIN pour accéder au suivi</p>
                <form onsubmit="verifyPin(event)" class="space-y-4">
                    <input type="password" id="pin-code" maxlength="4" placeholder="••••" required class="w-40 text-center text-2xl tracking-widest bg-slate-50 border border-slate-200 rounded-xl px-4 py-3 focus:outline-none focus:ring-2 focus:ring-slate-900 mx-auto block font-mono">
                    <button type="submit" class="w-full bg-slate-900 hover:bg-slate-800 text-white font-bold py-3.5 rounded-xl transition shadow-md">
                        Se connecter
                    </button>
                </form>
            </div>

            <!-- Dashboard Patron (Protégé) -->
            <div id="patron-dashboard" class="hidden space-y-6">
                
                <!-- Widgets Globaux en haut de page -->
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <div class="bg-white border border-slate-200/80 rounded-2xl p-5 shadow-sm flex items-center gap-4">
                        <div class="w-12 h-12 bg-amber-100 text-amber-700 rounded-xl flex items-center justify-center text-xl font-bold">⏱️</div>
                        <div>
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Total Heures (Mois)</span>
                            <div id="widget-total-heures" class="text-xl font-black text-slate-900 mt-0.5">0h 00</div>
                        </div>
                    </div>
                    <div class="bg-white border border-slate-200/80 rounded-2xl p-5 shadow-sm flex items-center gap-4">
                        <div class="w-12 h-12 bg-blue-100 text-blue-700 rounded-xl flex items-center justify-center text-xl font-bold">📋</div>
                        <div>
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Total Pointages</span>
                            <div id="widget-total-pointages" class="text-xl font-black text-slate-900 mt-0.5">0</div>
                        </div>
                    </div>
                    <div class="bg-white border border-slate-200/80 rounded-2xl p-5 shadow-sm flex items-center gap-4">
                        <div class="w-12 h-12 bg-emerald-100 text-emerald-700 rounded-xl flex items-center justify-center text-xl font-bold">🏢</div>
                        <div>
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-400">Chantiers Actifs</span>
                            <div id="widget-total-chantiers" class="text-xl font-black text-slate-900 mt-0.5">0</div>
                        </div>
                    </div>
                </div>

                <!-- Gestion des Ouvriers & Paramètres de Sécurité -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-4">👥 Gestion de l'équipe</h3>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="nouveau-ouvrier-nom" placeholder="Prénom et Nom du nouvel ouvrier" class="flex-1 bg-slate-50 border border-slate-200 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                            <button onclick="addOuvrier()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-bold px-5 py-2.5 rounded-xl text-sm transition shadow-sm">Ajouter</button>
                        </div>
                        <div id="liste-ouvriers-tags" class="flex flex-wrap gap-2"></div>
                    </div>

                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200 space-y-4">
                        <div>
                            <h3 class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-2">🔒 Modifier mon code PIN Patron</h3>
                            <div class="flex gap-2">
                                <input type="password" id="nouveau-pin" maxlength="4" placeholder="Nouveau code (4 chiffres)" class="flex-1 bg-slate-50 border border-slate-200 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-slate-900 font-mono">
                                <button onclick="changePin()" class="bg-slate-900 hover:bg-slate-800 text-white font-bold px-4 py-2.5 rounded-xl text-sm transition shadow-sm">Modifier</button>
                            </div>
                        </div>

                        <div class="pt-2 border-t border-slate-100">
                            <h3 class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-2">🔑 Modifier le code d'accès Ouvrier du site</h3>
                            <div class="flex gap-2">
                                <input type="text" id="nouveau-site-pass" placeholder="Nouveau code d'accès" class="flex-1 bg-slate-50 border border-slate-200 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500 font-semibold">
                                <button onclick="changeSitePass()" class="bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold px-4 py-2.5 rounded-xl text-sm transition shadow-sm">Changer</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Historique & Filtres -->
                <div class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200">
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-6">
                        <div>
                            <h3 class="text-lg font-bold text-slate-900">📊 Suivi des Heures (Lecture seule)</h3>
                            <p class="text-xs text-slate-500">Historique complet des fiches de chantier transmises par les ouvriers.</p>
                        </div>
                        <div class="flex flex-wrap gap-2 w-full sm:w-auto">
                            <select id="filtre-ouvrier" onchange="renderTable()" class="bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-sm font-medium focus:outline-none">
                                <option value="">Tous les ouvriers</option>
                            </select>
                            <button onclick="exportCSV()" class="bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold px-4 py-2 rounded-xl text-sm transition">Export CSV</button>
                            <button onclick="window.print()" class="bg-slate-900 hover:bg-slate-800 text-white font-semibold px-4 py-2 rounded-xl text-sm transition">Imprimer / PDF</button>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="border-b border-slate-200 text-[11px] font-bold uppercase tracking-wider text-slate-400">
                                    <th class="py-3 px-4">Date</th>
                                    <th class="py-3 px-4">Ouvrier</th>
                                    <th class="py-3 px-4">Chantier</th>
                                    <th class="py-3 px-4">Horaires</th>
                                    <th class="py-3 px-4">Durée (Nette)</th>
                                    <th class="py-3 px-4">Collègues</th>
                                    <th class="py-3 px-4">Remarques</th>
                                </tr>
                            </thead>
                            <tbody id="table-pointages" class="text-sm divide-y divide-slate-100"></tbody>
                        </table>
                    </div>
                </div>

            </div>
        </div>

    </main>

    <script>
        let ouvriers = JSON.parse(localStorage.getItem('cc_ouvriers')) || ['Jean Dupont', 'Marc Martin', 'Lucas Leroy'];
        let pointages = JSON.parse(localStorage.getItem('cc_pointages')) || [];
        let patronPin = localStorage.getItem('cc_pin') || '1234';
        let sitePass = localStorage.getItem('cc_site_pass') || 'travauxgroupe';

        function verifyOuvrierLogin(e) {
            e.preventDefault();
            const pass = document.getElementById('ouvrier-pass-input').value.trim();
            if (pass === sitePass) {
                document.getElementById('ouvrier-login-modal').classList.add('hidden');
            } else {
                alert('Code d\'accès incorrect.');
            }
        }

        function init() {
            const today = new Date().toISOString().split('T')[0];
            const dateInput = document.getElementById('date-chantier');
            if (dateInput) dateInput.value = today;

            const badge = document.getElementById('date-badge');
            if (badge) {
                const options = { weekday: 'short', day: 'numeric', month: 'short' };
                badge.innerText = new Date().toLocaleDateString('fr-FR', options);
            }

            updateOuvrierSelects();
            renderTable();
        }

        function switchTab(tab) {
            const isOuvrier = tab === 'ouvrier';
            document.getElementById('tab-ouvrier').classList.toggle('hidden', !isOuvrier);
            document.getElementById('tab-patron').classList.toggle('hidden', isOuvrier);
            
            const btnOuvrier = document.getElementById('btn-ouvrier');
            const btnPatron = document.getElementById('btn-patron');

            if (isOuvrier) {
                btnOuvrier.className = "px-4 py-2 rounded-xl text-xs font-bold transition flex items-center gap-2 bg-amber-500 text-slate-950 shadow-sm";
                btnPatron.className = "px-4 py-2 rounded-xl text-xs font-bold transition flex items-center gap-2 bg-slate-800 text-slate-300 hover:bg-slate-700";
            } else {
                btnPatron.className = "px-4 py-2 rounded-xl text-xs font-bold transition flex items-center gap-2 bg-amber-500 text-slate-950 shadow-sm";
                btnOuvrier.className = "px-4 py-2 rounded-xl text-xs font-bold transition flex items-center gap-2 bg-slate-800 text-slate-300 hover:bg-slate-700";
            }
        }

        function setNow(id) {
            const now = new Date();
            document.getElementById(id).value = now.toTimeString().substring(0, 5);
        }

        function updateOuvrierSelects() {
            const select = document.getElementById('ouvrier-select');
            const filtre = document.getElementById('filtre-ouvrier');
            const tags = document.getElementById('liste-ouvriers-tags');

            const currentSelected = select ? select.value : '';

            if (select) select.innerHTML = '<option value="">-- Sélectionner votre nom --</option>';
            if (filtre) filtre.innerHTML = '<option value="">Tous les ouvriers</option>';
            if (tags) tags.innerHTML = '';

            ouvriers.forEach((o, index) => {
                if (select) select.innerHTML += `<option value="${o}" ${currentSelected === o ? 'selected' : ''}>${o}</option>`;
                if (filtre) filtre.innerHTML += `<option value="${o}">${o}</option>`;
                if (tags) {
                    tags.innerHTML += `<span class="inline-flex items-center gap-2 bg-slate-100 border border-slate-200 px-3 py-1.5 rounded-xl text-xs font-bold text-slate-700">${o} <button onclick="removeOuvrier(${index})" class="text-rose-500 hover:text-rose-700 font-extrabold text-sm">&times;</button></span>`;
                }
            });

            localStorage.setItem('cc_ouvriers', JSON.stringify(ouvriers));
        }

        function addOuvrier() {
            const input = document.getElementById('nouveau-ouvrier-nom');
            const nom = input.value.trim();
            if (nom && !ouvriers.includes(nom)) {
                ouvriers.push(nom);
                input.value = '';
                updateOuvrierSelects();
                window.dispatchEvent(new Event('storage'));
            }
        }

        function removeOuvrier(index) {
            ouvriers.splice(index, 1);
            updateOuvrierSelects();
            window.dispatchEvent(new Event('storage'));
        }

        window.addEventListener('storage', (e) => {
            if (e.key === 'cc_ouvriers' || e.key === null) {
                ouvriers = JSON.parse(localStorage.getItem('cc_ouvriers')) || ouvriers;
                updateOuvrierSelects();
            }
            if (e.key === 'cc_pointages' || e.key === null) {
                pointages = JSON.parse(localStorage.getItem('cc_pointages')) || pointages;
                renderTable();
            }
        });

        function verifyPin(e) {
            e.preventDefault();
            const pin = document.getElementById('pin-code').value;
            if (pin === patronPin) {
                document.getElementById('patron-login').classList.add('hidden');
                document.getElementById('patron-dashboard').classList.remove('hidden');
                document.getElementById('pin-code').value = '';
                renderTable();
            } else {
                alert('Code PIN incorrect.');
            }
        }

        function changePin() {
            const newPin = document.getElementById('nouveau-pin').value.trim();
            if (newPin.length === 4 && !isNaN(newPin)) {
                patronPin = newPin;
                localStorage.setItem('cc_pin', patronPin);
                document.getElementById('nouveau-pin').value = '';
                alert('Code PIN patron mis à jour avec succès !');
            } else {
                alert('Veuillez entrer un code PIN valide à 4 chiffres.');
            }
        }

        function changeSitePass() {
            const newPass = document.getElementById('nouveau-site-pass').value.trim();
            if (newPass.length > 0) {
                sitePass = newPass;
                localStorage.setItem('cc_site_pass', sitePass);
                document.getElementById('nouveau-site-pass').value = '';
                alert('Le code d\'accès du site pour les ouvriers a été mis à jour avec succès !');
            } else {
                alert('Veuillez entrer un code valide.');
            }
        }

        function handlePointage(e) {
            e.preventDefault();
            const dateStr = document.getElementById('date-chantier').value;
            const arrivee = document.getElementById('heure-arrivee').value;
            const depart = document.getElementById('heure-depart').value;

            const [hArr, mArr] = arrivee.split(':').map(Number);
            const [hDep, mDep] = depart.split(':').map(Number);
            let totalMins = (hDep * 60 + mDep) - (hArr * 60 + mArr) - 30;
            if (totalMins < 0) totalMins = 0;

            const netH = Math.floor(totalMins / 60);
            const netM = totalMins % 60;
            const dureeStr = `${netH}h ${netM < 10 ? '0' : ''}${netM}`;

            const pointage = {
                id: Date.now(),
                date: dateStr,
                ouvrier: document.getElementById('ouvrier-select').value,
                chantier: document.getElementById('chantier-nom').value,
                arrivee,
                depart,
                dureeMinutes: totalMins,
                duree: dureeStr,
                collegues: document.getElementById('collegues').value || '-',
                remarques: document.getElementById('remarques').value || '-'
            };

            pointages.unshift(pointage);
            localStorage.setItem('cc_pointages', JSON.stringify(pointages));

            alert('Pointage validé et enregistré avec succès !');
            document.getElementById('pointage-form').reset();
            document.getElementById('date-chantier').value = new Date().toISOString().split('T')[0];
            renderTable();
        }

        function renderTable() {
            const tbody = document.getElementById('table-pointages');
            const filtreOuvrierEl = document.getElementById('filtre-ouvrier');
            const filtreOuvrier = filtreOuvrierEl ? filtreOuvrierEl.value : '';
            if (!tbody) return;
            tbody.innerHTML = '';

            const currentMonth = new Date().toISOString().substring(0, 7);
            let totalMinsMois = 0;
            let countPointages = 0;
            let chantiersSet = new Set();
            let tableCount = 0;

            pointages.forEach(p => {
                const match = !filtreOuvrier || p.ouvrier === filtreOuvrier;

                if (match && p.date.startsWith(currentMonth)) {
                    totalMinsMois += (p.dureeMinutes || 0);
                    countPointages++;
                    chantiersSet.add(p.chantier);
                }

                if (match) {
                    tableCount++;
                    tbody.innerHTML += `
                        <tr class="hover:bg-slate-50 transition">
                            <td class="py-3 px-4 font-semibold text-slate-700">${p.date}</td>
                            <td class="py-3 px-4 font-bold text-slate-900">${p.ouvrier}</td>
                            <td class="py-3 px-4 text-slate-800 font-medium">${p.chantier}</td>
                            <td class="py-3 px-4"><span class="bg-slate-100 text-slate-700 px-2.5 py-1 rounded-lg text-xs font-mono">${p.arrivee} → ${p.depart}</span></td>
                            <td class="py-3 px-4 font-bold text-emerald-600">${p.duree}</td>
                            <td class="py-3 px-4 text-slate-500">${p.collegues}</td>
                            <td class="py-3 px-4 text-slate-400 italic text-xs">${p.remarques}</td>
                        </tr>
                    `;
                }
            });

            if (tableCount === 0) {
                tbody.innerHTML = `<tr><td colspan="7" class="py-8 text-center text-slate-400 font-medium">Aucun pointage trouvé.</td></tr>`;
            }

            const h = Math.floor(totalMinsMois / 60);
            const m = totalMinsMois % 60;
            const wHeures = document.getElementById('widget-total-heures');
            const wPointages = document.getElementById('widget-total-pointages');
            const wChantiers = document.getElementById('widget-total-chantiers');

            if (wHeures) wHeures.innerText = `${h}h ${m < 10 ? '0' : ''}${m}`;
            if (wPointages) wPointages.innerText = countPointages;
            if (wChantiers) wChantiers.innerText = chantiersSet.size;
        }

        function exportCSV() {
            let csv = 'Date,Ouvrier,Chantier,Arrivee,Depart,Duree Nette,Collegues,Remarques\n';
            pointages.forEach(p => {
                csv += `"${p.date}","${p.ouvrier}","${p.chantier}","${p.arrivee}","${p.depart}","${p.duree}","${p.collegues}","${p.remarques}"\n`;
            });
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = `pointages_${new Date().toISOString().split('T')[0]}.csv`;
            link.click();
        }

        init();
    </script>
</body>
</html>
