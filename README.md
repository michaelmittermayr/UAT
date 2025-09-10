<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digitaler Power BI UAT</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #e5e7eb;
        }
        .form-container {
            max-width: 900px;
            margin: 2rem auto;
            background-color: #ffffff;
            border-radius: 1rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 2rem;
        }
        .section-heading {
            border-bottom: 2px solid #e5e7eb;
            padding-bottom: 0.5rem;
            margin-bottom: 1.5rem;
        }
        .form-row {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
        }
        @media (min-width: 640px) {
            .form-row {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        .test-table, .bug-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
        }
        .test-table th, .test-table td, .bug-table th, .bug-table td {
            padding: 0.75rem;
            text-align: left;
            border-bottom: 1px solid #e5e7eb;
        }
        .test-table th, .bug-table th {
            background-color: #f9fafb;
            font-weight: 600;
        }
        .button-group {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-top: 1rem;
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4">

<div id="app" class="form-container">
    <!-- User ID and Loading Indicator -->
    <div id="status-area" class="text-sm text-gray-600 mb-4 flex justify-between items-center">
        <span id="loading-indicator" class="hidden text-blue-500 font-medium">Lädt...</span>
        <span id="user-info"></span>
    </div>

    <!-- Main UAT Form -->
    <h1 class="text-3xl font-bold text-gray-800 mb-6 text-center">Digitaler Power BI UAT</h1>
    <form id="uat-form">

        <!-- 1. Berichtsinformationen -->
        <div class="mb-8">
            <h2 class="text-xl font-semibold text-gray-700 section-heading">1. Berichtsinformationen</h2>
            <div class="form-row">
                <div class="flex flex-col">
                    <label for="report-name" class="text-gray-600 mb-1">Berichtsname</label>
                    <input type="text" id="report-name" name="report-name" required class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="report-url" class="text-gray-600 mb-1">Berichts-URL</label>
                    <input type="url" id="report-url" name="report-url" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="report-variant" class="text-gray-600 mb-1">Berichtsvariante</label>
                    <select id="report-variant" name="report-variant" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="Power BI">Power BI</option>
                        <option value="Excel">Excel</option>
                        <option value="Tableau">Tableau</option>
                        <option value="SAP BO">SAP BO</option>
                        <option value="SAP Analytics Cloud">SAP Analytics Cloud</option>
                    </select>
                </div>
                <div class="flex flex-col">
                    <label for="use-case" class="text-gray-600 mb-1">Anwendungsszenario</label>
                    <select id="use-case" name="use-case" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="Ist Werte">Ist-Werte</option>
                        <option value="Planung">Planung</option>
                        <option value="Simulation">Simulation</option>
                        <option value="Reporting">Reporting</option>
                    </select>
                </div>
                <div class="flex flex-col">
                    <label for="version" class="text-gray-600 mb-1">Version</label>
                    <input type="text" id="version" name="version" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="bi-developer" class="text-gray-600 mb-1">Verantwortlicher BI Entwickler</label>
                    <input type="text" id="bi-developer" name="bi-developer" required class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="uat-tester" class="text-gray-600 mb-1">UAT-Tester</label>
                    <input type="text" id="uat-tester" name="uat-tester" required class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="report-owner" class="text-gray-600 mb-1">Berichtsbesitzer (Fachbereich)</label>
                    <input type="text" id="report-owner" name="report-owner" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="data-source" class="text-gray-600 mb-1">Datenquelle(n)</label>
                    <input type="text" id="data-source" name="data-source" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
            </div>
        </div>

        <!-- 2. UAT-Checkliste -->
        <div id="test-scenarios" class="mb-8">
            <h2 class="text-xl font-semibold text-gray-700 section-heading">2. UAT-Checkliste</h2>
            <p class="text-sm text-gray-500 mb-4">Bitte überprüfen Sie jedes Szenario und geben Sie ein Testergebnis und ggf. Kommentare ab.</p>

            <!-- Test Table -->
            <div class="overflow-x-auto rounded-lg shadow-sm">
                <table class="test-table bg-white">
                    <thead>
                        <tr>
                            <th class="w-1/12">Nr.</th>
                            <th class="w-6/12">Test-Szenario</th>
                            <th class="w-2/12 text-center">Ergebnis</th>
                            <th class="w-3/12">Kommentare</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Visual and Layout -->
                        <tr>
                            <td>2.1</td>
                            <td>Alle Visualisierungen und Textfelder werden korrekt und vollständig angezeigt.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="2.1" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="2.1" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>2.2</td>
                            <td>Das Layout ist konsistent über alle Seiten hinweg.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="2.2" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="2.2" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>2.3</td>
                            <td>Die verwendeten Farben und Schriften sind einheitlich und lesbar.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="2.3" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="2.3" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>2.4</td>
                            <td>Titel und Beschriftungen sind klar und verständlich.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="2.4" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="2.4" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>

                        <!-- Data Validation -->
                        <tr>
                            <td>3.1</td>
                            <td>Die Daten im Bericht entsprechen der Referenzquelle.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="3.1" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="3.1" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>3.2</td>
                            <td>Kennzahlen (KPIs) und Summenwerte sind korrekt berechnet.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="3.2" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="3.2" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>3.3</td>
                            <td>Drill-down und Drill-through Funktionen zeigen die korrekten Detaildaten an.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="3.3" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="3.3" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>3.4</td>
                            <td>Filter oder Slicer funktionieren wie erwartet und liefern korrekte Ergebnisse.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="3.4" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="3.4" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>3.5</td>
                            <td>Die Daten werden aktuell und ohne Verzögerung geladen.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="3.5" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="3.5" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>

                        <!-- Functionality -->
                        <tr>
                            <td>4.1</td>
                            <td>Filter und Slicer beeinflussen die entsprechenden Visualisierungen korrekt.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="4.1" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="4.1" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>4.2</td>
                            <td>Interaktionen zwischen Visualisierungen funktionieren wie erwartet.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="4.2" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="4.2" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>4.3</td>
                            <td>Lesezeichen (Bookmarks) leiten zu den korrekten Ansichten.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="4.3" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="4.3" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>4.4</td>
                            <td>Tooltips werden korrekt angezeigt und enthalten nützliche Informationen.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="4.4" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="4.4" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>4.5</td>
                            <td>Die Navigation zwischen den Seiten ist intuitiv und fehlerfrei.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="4.5" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="4.5" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        
                        <!-- Performance -->
                        <tr>
                            <td>5.1</td>
                            <td>Der Bericht lädt innerhalb einer akzeptablen Zeit.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="5.1" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="5.1" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                        <tr>
                            <td>5.2</td>
                            <td>Visualisierungen reagieren zügig auf Filter und Interaktionen.</td>
                            <td class="text-center">
                                <input type="checkbox" data-test="5.2" class="form-checkbox text-blue-600 rounded">
                            </td>
                            <td><textarea data-test-comment="5.2" rows="1" class="w-full p-1 border rounded-lg resize-none"></textarea></td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- 3. Mängel- und Fehlerdokumentation -->
        <div class="mb-8">
            <h2 class="text-xl font-semibold text-gray-700 section-heading">3. Mängel- und Fehlerdokumentation</h2>
            <p class="text-sm text-gray-500 mb-4">Erfassen Sie alle gefundenen Fehler mit Details.</p>
            <div class="overflow-x-auto rounded-lg shadow-sm">
                <table id="bug-table" class="bug-table bg-white">
                    <thead>
                        <tr>
                            <th class="w-1/12">Nr.</th>
                            <th class="w-5/12">Beschreibung des Mangels</th>
                            <th class="w-2/12">Schweregrad</th>
                            <th class="w-2/12">Status</th>
                            <th class="w-2/12"></th> <!-- For remove button -->
                        </tr>
                    </thead>
                    <tbody id="bug-table-body">
                        <!-- Bug rows will be added here dynamically -->
                    </tbody>
                </table>
            </div>
            <button type="button" id="add-bug-btn" class="mt-4 bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-full transition duration-300">
                + Mangel hinzufügen
            </button>
        </div>

        <!-- 4. Zusammenfassendes Feedback -->
        <div class="mb-8">
            <h2 class="text-xl font-semibold text-gray-700 section-heading">4. Zusammenfassendes Feedback</h2>
            <div class="flex flex-col mb-4">
                <label for="general-comments" class="text-gray-600 mb-1">Allgemeine Kommentare/Vorschläge für Verbesserungen</label>
                <textarea id="general-comments" name="general-comments" rows="3" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"></textarea>
            </div>
        </div>

        <!-- 5. Gesamturteil -->
        <div class="mb-8">
            <h2 class="text-xl font-semibold text-gray-700 section-heading">5. Gesamturteil</h2>
            <div class="flex flex-col space-y-2">
                <label class="inline-flex items-center">
                    <input type="radio" name="verdict" value="bestanden" class="form-radio text-green-500 rounded-full" required>
                    <span class="ml-2">Bestanden: Der Bericht ist bereit für die Veröffentlichung.</span>
                </label>
                <label class="inline-flex items-center">
                    <input type="radio" name="verdict" value="bestanden-maengel" class="form-radio text-yellow-500 rounded-full">
                    <span class="ml-2">Bestanden (mit kleineren Mängeln): Der Bericht ist nutzbar, kleinere Mängel müssen vor der Veröffentlichung behoben werden.</span>
                </label>
                <label class="inline-flex items-center">
                    <input type="radio" name="verdict" value="nicht-bestanden" class="form-radio text-red-500 rounded-full">
                    <span class="ml-2">Nicht bestanden: Der Bericht enthält kritische Fehler und benötigt eine Überarbeitung.</span>
                </label>
            </div>
        </div>

        <!-- 6. Formelle Abnahme -->
        <div class="mb-8">
            <h2 class="text-xl font-semibold text-gray-700 section-heading">6. Formelle Abnahme</h2>
            <div class="form-row">
                <div class="flex flex-col">
                    <label for="signer-name" class="text-gray-600 mb-1">Name des Unterzeichners</label>
                    <input type="text" id="signer-name" name="signer-name" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex flex-col">
                    <label for="acceptance-date" class="text-gray-600 mb-1">Datum der Abnahme</label>
                    <input type="date" id="acceptance-date" name="acceptance-date" class="p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
            </div>
        </div>

        <!-- Submit Buttons -->
        <div class="button-group">
            <button type="button" id="save-pdf-btn" class="bg-purple-600 hover:bg-purple-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition duration-300 transform hover:scale-105">
                Als PDF speichern
            </button>
        </div>
        <div id="submit-message" class="mt-4 text-center hidden text-sm font-medium"></div>

    </form>

    <hr class="my-8 border-gray-300">

    <!-- List of saved reports -->
    <div class="mt-8">
        <h2 class="text-xl font-semibold text-gray-700 section-heading">Gespeicherte Berichte</h2>
        <div id="reports-list" class="space-y-4">
            <!-- Reports will be injected here -->
        </div>
    </div>
</div>

<!-- PDF Libraries -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, collection, onSnapshot, query, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    setLogLevel('debug'); // Enable debug logging

    const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
    const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

    // Initialize Firebase
    let app, auth, db, userId;
    const loadingIndicator = document.getElementById('loading-indicator');
    const userInfoSpan = document.getElementById('user-info');
    let bugCounter = 0;

    // Authentication and DB Setup
    window.onload = async function() {
        loadingIndicator.classList.remove('hidden');
        try {
            app = initializeApp(firebaseConfig);
            auth = getAuth(app);
            db = getFirestore(app);

            const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
            if (initialAuthToken) {
                await signInWithCustomToken(auth, initialAuthToken);
            } else {
                await signInAnonymously(auth);
            }

            onAuthStateChanged(auth, (user) => {
                loadingIndicator.classList.add('hidden');
                if (user) {
                    userId = user.uid;
                    userInfoSpan.textContent = `Angemeldet als: ${userId}`;
                    listenForReports();
                } else {
                    userId = crypto.randomUUID();
                    userInfoSpan.textContent = `Angemeldet als anonymer Nutzer.`;
                }
            });
        } catch (error) {
            console.error("Firebase Initialization or Auth Error:", error);
            loadingIndicator.classList.add('hidden');
        }
    };

    // Listen for real-time updates to the reports list
    function listenForReports() {
        if (!db) {
            console.error("Firestore is not initialized.");
            return;
        }
        const reportsCollectionRef = collection(db, `artifacts/${appId}/public/data/uat_reports`);
        
        // Note: orderBy is commented out to avoid index errors, as per instructions. Data will be sorted in the client.
        const q = query(reportsCollectionRef); 

        onSnapshot(q, (snapshot) => {
            let reports = [];
            snapshot.forEach(doc => {
                reports.push({ id: doc.id, ...doc.data() });
            });
            
            // Client-side sorting by timestamp, since orderBy is avoided.
            reports.sort((a, b) => (b.timestamp?.seconds || 0) - (a.timestamp?.seconds || 0));

            renderReports(reports);
        }, (error) => {
            console.error("Error listening to reports:", error);
        });
    }

    // Render the list of reports
    function renderReports(reports) {
        const reportsListContainer = document.getElementById('reports-list');
        reportsListContainer.innerHTML = '';
        if (reports.length === 0) {
            reportsListContainer.innerHTML = '<p class="text-gray-500 text-center">Noch keine Berichte gespeichert.</p>';
            return;
        }

        reports.forEach(report => {
            const reportElement = document.createElement('div');
            reportElement.className = 'p-4 border border-gray-200 rounded-lg shadow-sm bg-white';
            reportElement.innerHTML = `
                <div class="flex items-center justify-between mb-2">
                    <h3 class="font-bold text-lg text-blue-600">${report.reportName}</h3>
                    <span class="text-sm text-gray-500">Tester: ${report.uatTester}</span>
                </div>
                <p class="text-sm text-gray-700"><strong>Variante:</strong> ${report.reportVariant}</p>
                <p class="text-sm text-gray-700"><strong>Szenario:</strong> ${report.useCase}</p>
                <p class="text-sm text-gray-700"><strong>Version:</strong> ${report.version}</p>
                <p class="text-sm text-gray-700"><strong>Ergebnis:</strong> ${getVerdictLabel(report.verdict)}</p>
                <p class="text-xs text-gray-400 mt-2">Gespeichert am: ${new Date(report.timestamp.seconds * 1000).toLocaleString()}</p>
            `;
            reportsListContainer.appendChild(reportElement);
        });
    }

    function getVerdictLabel(verdict) {
        switch(verdict) {
            case 'bestanden':
                return '<span class="text-green-600 font-bold">Bestanden</span>';
            case 'bestanden-maengel':
                return '<span class="text-yellow-600 font-bold">Bestanden (mit Mängeln)</span>';
            case 'nicht-bestanden':
                return '<span class="text-red-600 font-bold">Nicht bestanden</span>';
            default:
                return 'Unbekannt';
        }
    }

    // Dynamic Bug Table Logic
    document.getElementById('add-bug-btn').addEventListener('click', () => {
        bugCounter++;
        const tableBody = document.getElementById('bug-table-body');
        const newRow = document.createElement('tr');
        newRow.innerHTML = `
            <td>${bugCounter}</td>
            <td><textarea rows="1" class="w-full p-1 border rounded-lg resize-none" data-bug-field="description"></textarea></td>
            <td>
                <select class="w-full p-1 border rounded-lg" data-bug-field="severity">
                    <option value="Kritisch">Kritisch</option>
                    <option value="Hoch">Hoch</option>
                    <option value="Mittel">Mittel</option>
                    <option value="Niedrig">Niedrig</option>
                </select>
            </td>
            <td>
                <select class="w-full p-1 border rounded-lg" data-bug-field="status">
                    <option value="Offen">Offen</option>
                    <option value="In Bearbeitung">In Bearbeitung</option>
                    <option value="Gelöst">Gelöst</option>
                </select>
            </td>
            <td>
                <button type="button" class="text-red-500 hover:text-red-700 font-bold text-lg" onclick="this.closest('tr').remove(); updateBugNumbers();">&times;</button>
            </td>
        `;
        tableBody.appendChild(newRow);
    });

    // Function to update the row numbers after a bug is removed
    window.updateBugNumbers = () => {
        const rows = document.getElementById('bug-table-body').getElementsByTagName('tr');
        for (let i = 0; i < rows.length; i++) {
            rows[i].getElementsByTagName('td')[0].textContent = i + 1;
        }
        bugCounter = rows.length;
    };

    // Event listener for PDF generation
    const savePdfBtn = document.getElementById('save-pdf-btn');
    savePdfBtn.addEventListener('click', async () => {
        const reportName = document.getElementById('report-name').value;
        if (!reportName) {
            const message = document.getElementById('submit-message');
            message.textContent = 'Bitte geben Sie einen Berichtsnamen ein, bevor Sie das PDF speichern.';
            message.classList.remove('hidden', 'text-green-500');
            message.classList.add('text-red-500');
            return;
        }

        const appContainer = document.getElementById('app');
        const originalWidth = appContainer.style.width;
        const originalMargin = appContainer.style.margin;

        // Temporarily adjust styles for better PDF rendering
        appContainer.style.width = '210mm';
        appContainer.style.margin = '0 auto';

        await new Promise(resolve => setTimeout(resolve, 500));

        try {
            const canvas = await html2canvas(appContainer, {
                scale: 2,
                logging: false,
                useCORS: true
            });
            const imgData = canvas.toDataURL('image/jpeg', 1.0);
            const pdf = new window.jsPDF({
                orientation: 'portrait',
                unit: 'mm',
                format: 'a4'
            });

            const imgProps = pdf.getImageProperties(imgData);
            const pdfWidth = pdf.internal.pageSize.getWidth();
            const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;

            pdf.addImage(imgData, 'JPEG', 0, 0, pdfWidth, pdfHeight);
            
            const date = new Date().toISOString().slice(0, 10);
            const filename = `${reportName.replace(/[^a-z0-9]/gi, '_')}_UAT_${date}.pdf`;
            pdf.save(filename);

        } catch (error) {
            console.error('Error generating PDF:', error);
            const message = document.getElementById('submit-message');
            message.textContent = 'Fehler beim Generieren des PDF.';
            message.classList.remove('hidden', 'text-green-500');
            message.classList.add('text-red-500');
        } finally {
            // Restore original styles
            appContainer.style.width = originalWidth;
            appContainer.style.margin = originalMargin;
        }
    });
</script>

</body>
</html>
