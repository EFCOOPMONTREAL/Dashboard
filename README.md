
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EFC — Dashboard Complet</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{--bg:#F8F7F4;--card:#fff;--border:#E8E5DF;--text:#1a1a1a;--muted:#888;--accent:#0F6E56;--accent2:#185FA5;--accent3:#533AB7;--accent4:#D4537E;--accent5:#BA7517;--accent6:#A32D2D;--radius:12px;--shadow:0 1px 4px rgba(0,0,0,.06)}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);padding:0}
.header{padding:2rem 2rem 1rem;max-width:1500px;margin:0 auto}
.header h1{font-size:22px;font-weight:700;letter-spacing:-.02em}
.header p{font-size:13px;color:var(--muted);margin-top:2px}
.tabs{display:flex;gap:0;padding:1rem 2rem 0;max-width:1500px;margin:0 auto}
.tab-btn{padding:10px 24px;font-size:13px;font-weight:600;border:1px solid var(--border);background:var(--card);cursor:pointer;font-family:inherit;color:#666;transition:all .15s}
.tab-btn:first-child{border-radius:10px 0 0 10px}
.tab-btn:last-child{border-radius:0 10px 10px 0}
.tab-btn.active{background:var(--text);color:#fff;border-color:var(--text)}
.tab-content{display:none}.tab-content.active{display:block}
.stats-row{display:flex;gap:10px;padding:1rem 2rem;max-width:1500px;margin:0 auto;flex-wrap:wrap}
.stat-card{background:var(--card);border:1px solid var(--border);border-radius:var(--radius);padding:14px 18px;flex:1;min-width:120px;box-shadow:var(--shadow)}
.stat-card .num{font-family:'Space Mono',monospace;font-size:26px;font-weight:700;line-height:1}
.stat-card .label{font-size:10px;color:var(--muted);margin-top:4px;text-transform:uppercase;letter-spacing:.04em}
.controls{display:flex;gap:8px;padding:.5rem 2rem;max-width:1500px;margin:0 auto;flex-wrap:wrap;align-items:center}
.controls label{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.04em}
.pill{padding:5px 14px;font-size:12px;border:1px solid var(--border);border-radius:20px;background:var(--card);color:#666;cursor:pointer;transition:all .15s;font-family:inherit}
.pill:hover{border-color:#aaa}
.pill.active{background:var(--accent);color:#fff;border-color:var(--accent)}
.pill[data-g="france"].active{background:var(--accent2);border-color:var(--accent2)}
.pill[data-g="quebec"].active{background:var(--accent3);border-color:var(--accent3)}
.pill[data-g="autre"].active{background:var(--accent5);border-color:var(--accent5)}
.search-box{padding:6px 14px;font-size:13px;border:1px solid var(--border);border-radius:20px;background:var(--card);font-family:inherit;width:200px;outline:none}
.search-box:focus{border-color:var(--accent)}
.view-toggle{display:flex;gap:0;margin-left:auto}
.view-btn{padding:5px 12px;font-size:12px;border:1px solid var(--border);background:var(--card);cursor:pointer;font-family:inherit;color:#666}
.view-btn:first-child{border-radius:8px 0 0 8px}
.view-btn:last-child{border-radius:0 8px 8px 0}
.view-btn.active{background:var(--text);color:#fff;border-color:var(--text)}
.main{padding:.5rem 2rem 2rem;max-width:1500px;margin:0 auto}
.cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(340px,1fr));gap:12px}
.card{background:var(--card);border:1px solid var(--border);border-radius:var(--radius);padding:16px;box-shadow:var(--shadow);cursor:pointer;transition:box-shadow .15s,transform .15s}
.card:hover{box-shadow:0 4px 16px rgba(0,0,0,.1);transform:translateY(-1px)}
.card-head{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px}
.card-name{font-size:14px;font-weight:600;line-height:1.3;max-width:65%}
.badge{font-size:9px;padding:2px 8px;border-radius:10px;font-weight:600;white-space:nowrap}
.badge-efc{background:#E1F5EE;color:var(--accent)}
.badge-ef{background:#E6F1FB;color:var(--accent2)}
.mat-enplace{background:#D4EDDA;color:#155724}
.mat-dev{background:#FFF3CD;color:#856404}
.mat-nonabouti{background:#F8D7DA;color:#721C24}
.mat-manque{background:#f0f0f0;color:#999}
.card-region{font-size:11px;color:var(--muted);margin-bottom:6px}
.card-row{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:4px}
.tag{font-size:10px;padding:2px 7px;border-radius:8px;background:#f3f2ef;color:#555}
.card-section{margin-top:8px;padding-top:8px;border-top:1px solid var(--border)}
.card-section-title{font-size:10px;text-transform:uppercase;letter-spacing:.05em;color:var(--muted);margin-bottom:4px}
.bar-row{display:flex;align-items:center;gap:6px;margin-bottom:2px}
.bar-label{font-size:10px;color:#666;width:45px;text-align:right;flex-shrink:0}
.bar-track{flex:1;height:5px;background:#f0eeea;border-radius:3px;overflow:hidden}
.bar-fill{height:100%;border-radius:3px}
.bar-val{font-size:10px;color:var(--muted);width:14px;flex-shrink:0}
.table-wrap{overflow-x:auto;border:1px solid var(--border);border-radius:var(--radius);background:var(--card);box-shadow:var(--shadow)}
table{width:100%;border-collapse:collapse;min-width:1100px}
thead th{font-size:10px;font-weight:600;padding:10px 8px;text-align:left;background:#f7f6f3;border-bottom:1px solid var(--border);position:sticky;top:0;cursor:pointer;white-space:nowrap;color:#555}
thead th:hover{background:#eeedea}
tbody td{font-size:11px;padding:8px;border-bottom:1px solid #f0eeea;vertical-align:middle}
tbody tr:hover td{background:#faf9f7}
tbody tr{cursor:pointer}
.modal-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.4);z-index:100;justify-content:center;align-items:center}
.modal-bg.show{display:flex}
.modal{background:var(--card);border-radius:16px;max-width:800px;width:95%;max-height:88vh;overflow-y:auto;padding:28px;position:relative;box-shadow:0 20px 60px rgba(0,0,0,.2)}
.modal-close{position:absolute;top:12px;right:16px;font-size:20px;cursor:pointer;color:var(--muted);background:none;border:none}
.modal h2{font-size:18px;font-weight:700;margin-bottom:4px}
.modal .meta{font-size:12px;color:var(--muted);margin-bottom:14px}
.modal-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-top:14px}
.modal-block{padding:12px;border-radius:10px;background:#faf9f7}
.modal-block h4{font-size:11px;text-transform:uppercase;letter-spacing:.04em;color:var(--muted);margin-bottom:6px}
.offre-row{display:flex;gap:8px;margin-top:8px}
.offre-box{flex:1;padding:8px;border-radius:8px;font-size:10px;line-height:1.4}
.offre-init{background:#fdf6f0;border:1px solid #f0e0d0}
.offre-efc{background:#eef7f3;border:1px solid #c8e6d8}
.offre-label{font-weight:600;font-size:9px;text-transform:uppercase;letter-spacing:.04em;margin-bottom:3px;color:var(--muted)}
.outil-chip{display:inline-flex;align-items:center;gap:4px;padding:4px 10px;border-radius:8px;font-size:10px;margin:2px;background:#E6F1FB;color:var(--accent2);font-weight:500}
.outil-chip.primary{background:#E1F5EE;color:var(--accent)}
.outil-chip a{color:inherit;text-decoration:none}.outil-chip a:hover{text-decoration:underline}
.matrix-controls{display:flex;gap:8px;padding:1rem 0;flex-wrap:wrap;align-items:center}
.matrix-wrap{overflow-x:auto;border:1px solid var(--border);border-radius:var(--radius);background:var(--card);box-shadow:var(--shadow)}
.matrix-wrap table{min-width:1600px}
.matrix-wrap thead th{font-size:9px;min-width:55px;max-width:68px;text-align:center;vertical-align:bottom;line-height:1.2;padding:8px 4px}
.matrix-wrap thead th:first-child{min-width:220px;text-align:left;font-size:11px}
.matrix-wrap tbody td{text-align:center;padding:6px 4px}
.frein-cell{text-align:left!important;font-size:11px;padding:6px 10px!important}
.cat-badge{display:inline-block;font-size:8px;padding:1px 6px;border-radius:8px;margin-bottom:2px;font-weight:600}
.dot{width:12px;height:12px;border-radius:50%;display:inline-block}
.dot.p{background:#1D9E75}.dot.s{background:#85B7EB}.dot.empty{width:5px;height:5px;background:#ddd}
.legend{display:flex;gap:14px;font-size:12px;color:#666;align-items:center;flex-wrap:wrap}
.legend-item{display:flex;align-items:center;gap:5px}
.legend-dot{width:12px;height:12px;border-radius:50%;flex-shrink:0}
.count-badge{font-family:'Space Mono',monospace;font-size:12px;color:var(--muted);text-align:center;padding:8px 2rem}

/* Charts */
.charts-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;padding:1rem 0}
.chart-card{background:var(--card);border:1px solid var(--border);border-radius:var(--radius);padding:20px;box-shadow:var(--shadow)}
.chart-card.full{grid-column:1/-1}
.chart-card h3{font-size:13px;font-weight:600;margin-bottom:12px;color:var(--text)}
.chart-card canvas{width:100%!important}

.hidden{display:none!important}
</style>
</head>
<body>
<div class="header">
<h1>Déployer l'EFC — Dashboard d'Analyse Complet</h1>
<p>105 études de cas · 30 outils · 48 freins identifiés · Projet ENV825 × Synergie Économique Laurentides</p>
</div>
<div class="tabs">
<button class="tab-btn active" data-tab="entreprises">Entreprises</button>
<button class="tab-btn" data-tab="visualisations">Visualisations</button>
<button class="tab-btn" data-tab="matrice">Matrice Outils / Freins</button>
</div>

<!-- TAB 1 -->
<div class="tab-content active" id="tab-entreprises">
<div class="stats-row" id="stats"></div>
<div class="controls" id="ctrl1">
<label>Pays :</label>
<button class="pill active" data-g="all">Tous</button>
<button class="pill" data-g="france">France</button>
<button class="pill" data-g="quebec">Québec</button>
<button class="pill" data-g="autre">Autre</button>
<label style="margin-left:10px">Maturité :</label>
<button class="pill active" data-m="all">Tous</button>
<button class="pill" data-m="En place">En place</button>
<button class="pill" data-m="En développement">En dév.</button>
<button class="pill" data-m="Non Abouti">Non abouti</button>
<input class="search-box" id="search" placeholder="Rechercher…">
<div class="view-toggle">
<button class="view-btn active" data-v="cards">Cartes</button>
<button class="view-btn" data-v="table">Tableau</button>
</div>
</div>
<div class="main">
<div id="cards-view" class="cards"></div>
<div id="table-view" class="table-wrap hidden"></div>
<div class="count-badge"><span id="count"></span> entreprise(s)</div>
</div>
</div>

<!-- TAB 2: VISUALISATIONS -->
<div class="tab-content" id="tab-visualisations">
<div class="main">
<div class="charts-grid">
<div class="chart-card"><h3>Répartition EF / EFC</h3><canvas id="ch-ef-efc"></canvas></div>
<div class="chart-card"><h3>Entreprises par taille</h3><canvas id="ch-taille"></canvas></div>
<div class="chart-card"><h3>Maturité des démarches EFC</h3><canvas id="ch-maturite"></canvas></div>
<div class="chart-card"><h3>Entreprises par secteur d'activité</h3><canvas id="ch-secteur"></canvas></div>
<div class="chart-card full"><h3>Leviers par catégorie — Entreprises « En place »</h3><canvas id="ch-leviers"></canvas></div>
<div class="chart-card full"><h3>Freins par catégorie — Entreprises « Non abouti » et « En développement »</h3><canvas id="ch-freins"></canvas></div>
<div class="chart-card full"><h3>Freins moyens par secteur d'activité</h3><canvas id="ch-secteur-freins"></canvas></div>
</div>
</div>
</div>

<!-- TAB 3: MATRICE -->
<div class="tab-content" id="tab-matrice">
<div class="main">
<div class="matrix-controls">
<label>Catégorie :</label>
<button class="pill active" data-mc="all">Tous</button>
<button class="pill" data-mc="eco">Économique</button>
<button class="pill" data-mc="org">Organisationnel</button>
<button class="pill" data-mc="info">Informationnel</button>
<button class="pill" data-mc="mar">Marché</button>
<button class="pill" data-mc="jur">Juridique</button>
<button class="pill" data-mc="tec">Technique</button>
<button class="pill" data-mc="str">Stratégique</button>
</div>
<div class="legend" style="margin-bottom:12px">
<div class="legend-item"><div class="legend-dot" style="background:#1D9E75"></div>Outil principal</div>
<div class="legend-item"><div class="legend-dot" style="background:#85B7EB"></div>Complémentaire</div>
<div class="legend-item"><div class="legend-dot" style="background:#ddd;width:6px;height:6px"></div>Non pertinent</div>
</div>
<div class="matrix-wrap" id="matrix-wrap"></div>
</div>
</div>

<div class="modal-bg" id="modal-bg"><div class="modal" id="modal"></div></div>

<script>

const COMPANIES=[{"n":"Obione","r":"France (Bourgogne-Franche-Comté)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Distribution & Négoce","Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Vente classique et quantitative de produits","oe":"Offre centrée sur le concept « Happy » : déporter la valeur ajoutée vers l'accompagnement, les compétences et la coopération entre le fournisseur, le vétérinaire et l'éleveur","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Non Abouti","rep":0,"rec":"Stabilisation grâce à la confiance et la fidélisation des acteurs","env":"Moins de consommation de médicaments en élevage et amélioration du modèle d’élevage","soc":"Amélioration du confort de travail, bien-être animal, et restauration de la motivation (raison d'être) pour des professions (éleveurs, vétérinaires) qui souffrent de perte de sens.","fo":0,"fe":1,"fh":1,"ft":0,"fi":0,"fr":0,"lo":0,"le":0,"lh":3,"lt":0,"li":0,"lr":0,"ln":0,"src":"•\tADEME. (2024). Panorama sur l’économie de la fonctionnalité et de la coopérati"},{"n":"Sofraser","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Maintenance & Réparation"],"ty":["SAV & Maintenance","Vente de Performance"],"oi":"Vente classique de matériel (viscosimètres)","oe":"L'entreprise ne vend plus du temps de nettoyage, mais s'engage sur l'amélioration concrète de l'efficacité énergétique","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Sécurisation par des contrats à long terme basés sur la performance","env":"Réduction drastique des consommations de combustibles, baisse des émissions polluantes et des rebuts, et prolongation de la durée de vie des installations thermiques.","soc":"Les salariés sont associés et responsabilisés. Le nouveau modèle modifie leur vision du métier et ils sont fiers de participer directement à un bénéfice environnemental clair.","fo":1,"fe":1,"fh":0,"ft":0,"fi":0,"fr":0,"lo":1,"le":0,"lh":0,"lt":1,"li":0,"lr":0,"ln":0,"src":"•\tADEME. (2024). Panorama sur l’économie de la fonctionnalité et de la coopérati"},{"n":"Serue","r":"France (Grand Est)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Externalisation"],"oi":"Vente classique de prestations d'ingénierie et de pilotage","oe":"Offre \"Lean Chantier\" renforçant le rôle de SERUE dans l'amélioration de la coordination et de la coopération entre les différentes entreprises engagées dans la réalisation d'un projet.","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Sécurisation par une différenciation forte sur le marché et une fidélisation des acteurs grâce à la nouvelle méthode de gestion.","env":"Volonté de réduire les émissions de gaz à effet de serre dans le domaine de la construction et optimisation globale de la logistique du chantier.","soc":"Baisse significative du stress des collaborateurs et amélioration du climat de travail lors des réunions de chantier.","fo":0,"fe":0,"fh":1,"ft":0,"fi":0,"fr":0,"lo":0,"le":0,"lh":1,"lt":1,"li":0,"lr":0,"ln":0,"src":"•\tADEME, & SERUE Ingénierie. (s.d.). Économie de la fonctionnalité et de la coop"},{"n":"Univeira","r":"France (Île-de-France)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Prestation Intellectuelle"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Vente d'outils classique","oe":"Logique d'accompagnement au service des enjeux de travail du client (sécurité, productivité, fiabilité), reposant sur l'observation, l'analyse du travail et la conception de solutions technico-organis","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Sécurisation de l'activité avec une forte croissance ; le chiffre d'affaires a quasiment doublé depuis l'accompagnement EFC.","env":"Réduction de la surproduction et de la consommation de matières premières/énergie.","soc":"Amélioration de la santé et du confort des techniciens","fo":0,"fe":1,"fh":1,"ft":0,"fi":0,"fr":0,"lo":1,"le":0,"lh":1,"lt":1,"li":0,"lr":0,"ln":0,"src":"•\tADEME, & Univeira. (s.d.). Économie de la fonctionnalité et de la coopération "},{"n":"Hebus","r":"France (Normandie)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Maintenance & Réparation","Prestation Intellectuelle","Service Numérique"],"ty":["Conseil & Formation","Externalisation"],"oi":"Vente classique de matériel et prestations d'installateur d’infrastructures IT","oe":"Offre d'accompagnement global rendant visible la valeur immatérielle : montrer toutes les étapes de l'évolution de l'infrastructure pour que le client comprenne la complexité, la valeur ajoutée et l'i","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Stabilisation grâce aux contrats d'infogérance et de gestion de données sur le long terme.","env":"Rendre les systèmes IT plus \"vertueux\", ce qui implique une optimisation du matériel, une lutte contre l'obsolescence prématurée des infrastructures réseau et une meilleure efficacité globale.","soc":"Volonté forte d’offrir aux collaborateurs et aux dirigeants un équilibre de vie satisfaisant (choix d'une croissance maîtrisée).","fo":0,"fe":0,"fh":1,"ft":0,"fi":0,"fr":0,"lo":1,"le":0,"lh":1,"lt":1,"li":0,"lr":0,"ln":0,"src":"•\tADEME, & HÉBUS. (s.d.). Économie de la fonctionnalité et de la coopération Ret"},{"n":"ACTION LOGEMENT","r":"France (Occitanie)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Prestation Intellectuelle","Service à la Personne"],"ty":["Mutualisation / Partage","Externalisation"],"oi":"Offres classiques de financement du logement","oe":"Offre de \"CorpoWorking\" co-conçue : mise à disposition d'espaces de travail partagés proches du domicile des salariés, intégrant des services pour maintenir le lien social à l'entreprise et faciliter ","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Stabilisation via des contrats ou des abonnements B2B récurrents liés à l'usage des espaces par les salariés des entreprises clientes.","env":"Réduction drastique des déplacements automobiles (trajets pendulaires domicile-travail) et des émissions de CO₂ associées, ainsi que la réhabilitation de sites existants en zone rurale pour éviter de nouvelles constructions.","soc":": Amélioration de l'équilibre vie pro/vie perso pour les salariés, diminution de la fatigue liée aux transports, maintien du lien collectif malgré le télétravail, et revitalisation de petits bourgs ruraux.","fo":1,"fe":0,"fh":0,"ft":0,"fi":0,"fr":0,"lo":1,"le":0,"lh":1,"lt":0,"li":1,"lr":0,"ln":0,"src":"•\tactionlogement. (s.d.). Direction Régionale Action Logement Occitanie. Repéré "},{"n":"RELAIS D’ENTREPRISES","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Location / Leasing","Mutualisation / Partage"],"oi":"assurance ","oe":"logique de solution globale et de valeur d’usage","vb":0,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Élevée (abonnements / accompagnement long)","env":"Réduction des déplacements, ancrage local","soc":"Forte (coopération, dynamisation des territoires)","fo":3,"fe":1,"fh":2,"ft":1,"fi":0,"fr":0,"lo":1,"le":1,"lh":0,"lt":1,"li":1,"lr":0,"ln":0,"src":"https://www.relais-entreprises.fr/   IEFC (2022)"},{"n":"SOFAXIS - GROUPE RELYENS","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Conseil","oe":"passage d’une assurance classique à une solution intégrée de sécurisation","vb":0,"vu":0,"vp":1,"ef":1,"efc":0,"mat":"En développement","rep":0,"rec":"Élevée (cotisations / contrats)","env":"Modéré (prévention, optimisation des ressources)","soc":"Forte (sécurisation du service public)","fo":1,"fe":0,"fh":1,"ft":0,"fi":0,"fr":0,"lo":1,"le":1,"lh":1,"lt":1,"li":1,"lr":0,"ln":0,"src":" IEFC (2022)"},{"n":"HD AUTOMATISME","r":"France (Hauts de France)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle"],"ty":["SAV & Maintenance"],"oi":"non mentionné","oe":"Maintenance et optimisation d'équipements industriels, augmentation de la productivité des équipements et réduction des risques de futures pannes","vb":1,"vu":0,"vp":0,"ef":1,"efc":0,"mat":"En place","rep":1,"rec":"Contrats de maintenance et d'optimisation, et vente à la pièce détachée","env":"Reconditionnement et réemploi de pièces détachées, allongement de la durée d'utilisation des équipements industriels","soc":"Contrats auprès d'entreprises locales possédant des équipements industriels","fo":0,"fe":3,"fh":1,"ft":1,"fi":1,"fr":0,"lo":3,"le":2,"lh":1,"lt":2,"li":0,"lr":1,"ln":1,"src":"https://www.hdautomatisme.com/"},{"n":"PROXICOMPOST","r":"France (La Réunion)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Micro","sv":["Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle","Gestion de Déchets"],"ty":["SAV & Maintenance","Conseil & Formation"],"oi":"non mentionné","oe":"Prestations de service pour la gestion des déchets (tri à la source jusqu'au traitement) et vente d'équipements de compostage","vb":1,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Récurrence avec prestations de service et vente d'équipements de compostage, mais association à but non lucratif","env":"Réduction, gestion et valorisation des déchets","soc":"Coopération territoriale auprès d'industriels, de collectivités et de particuliers locaux","fo":1,"fe":0,"fh":3,"ft":0,"fi":1,"fr":0,"lo":3,"le":0,"lh":3,"lt":4,"li":2,"lr":1,"ln":1,"src":"https://www.proxicompost.ch"},{"n":"Transforce Beltal Inc.","r":"Dunham, Brome-Missisquoi","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Conception & Fabrication","Maintenance & Réparation"],"ty":["SAV & Maintenance"],"oi":"Vente de produits","oe":"Accompagnement orienté usages industriels","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En développement","rep":1,"rec":"Contrats performance récurrents","env":"Durée de vie prolongée des équipements","soc":"Coopération locale renforcée","fo":2,"fe":1,"fh":1,"ft":0,"fi":0,"fr":1,"lo":3,"le":2,"lh":1,"lt":2,"li":2,"lr":0,"ln":1,"src":"http://www.transforce-beltal.com/en/"},{"n":"ODIN","r":"Pays-Bas","co":"Autre","ac":"OUI","cl":"Particulier","sz":"Grande","sv":["Distribution & Négoce"],"ty":["Conseil & Formation"],"oi":"Vente de sacs de légumes biologiques à partir de points de collecte de quartier avec système de souscription avec prix fixe","oe":"Commande hebdomadaire de sac de légumes ou de fruits avec tarification à la semaine ou vente des produits à l'unité","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Elevé car système d'abonnement hebdomadaire et de vente de produits en ligne ou en magasin","env":"Circuit court des agriculteurs/ producteurs aux consommateurs donc moins de transport, moins de pollution car aliments biologiques et moins d'emballage","soc":"Elevée car partenariats locaux et réseau local de clients membres","fo":1,"fe":1,"fh":0,"ft":1,"fi":1,"fr":0,"lo":2,"le":2,"lh":2,"lt":2,"li":2,"lr":0,"ln":1,"src":"https://www.odin.nl/"},{"n":"GISPEN","r":"Pays-Bas","co":"Autre","ac":"OUI","cl":"particulier/organisation","sz":"Grande","sv":["Conception & Fabrication","Distribution & Négoce","Maintenance & Réparation","Gestion de Déchets"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing"],"oi":"Production et vente de meubles","oe":"Offre de service de vente ou de location de mobilier de bureau avec aménagement pour des meilleurs environnements de travail pour les clients","vb":1,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Elevé car système de paiment mensuel pour location ou achat de meubles","env":"Faible car éco-conception et réemploi de meubles usagés","soc":"Elevée car partenariats publics et privés locaux","fo":2,"fe":2,"fh":0,"ft":0,"fi":1,"fr":0,"lo":3,"le":4,"lh":3,"lt":4,"li":2,"lr":0,"ln":1,"src":"https://www.gispen.com"},{"n":"REJOUÉ","r":"Ile-de-France, France","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Distribution & Négoce","Maintenance & Réparation","Gestion de Déchets"],"ty":["SAV & Maintenance"],"oi":"non mentionné","oe":"Atelier d'inclusion socioprofessionnelle qui donne une seconde vie aux jouets","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"A chaque vente de jouet","env":"Réemploi favorisant lé réduction des matières premières et de leur transport","soc":"Aide et accompagnement à la réinsertion professionnelle pour l'accès à un emploi durable. Valeur forte de mutualisation et de partage pour le bien des enfants.","fo":2,"fe":2,"fh":2,"ft":1,"fi":1,"fr":0,"lo":2,"le":2,"lh":5,"lt":7,"li":2,"lr":0,"ln":1,"src":"http://www.rejoue.asso.fr/"},{"n":"ALOEN & OPTIM'ISM","r":"France (Bretagne)","co":"France","ac":"OUI","cl":"Particulier","sz":"Petite","sv":["Distribution & Négoce","Prestation Intellectuelle","Logistique & Transport","Gestion de Déchets"],"ty":["Conseil & Formation","Externalisation"],"oi":"Conseil, accompagnement à la rénovation énergétique et activité de maraîchage --> volonté de combiner les expertises et aller plus loin","oe":"EFC : Déconstruction et réemploi des matériaux localement, création d'emploi locaux, entretien des espaces verts du lieu et animation d'ateliers","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"n/a","env":"Positif","soc":"Valeur ajoutée (création d'emplois locaux, sensibilisation et formation, ...)","fo":0,"fe":1,"fh":0,"ft":0,"fi":1,"fr":0,"lo":4,"le":3,"lh":4,"lt":6,"li":2,"lr":3,"ln":1,"src":"ADEME A. (2024). Carnet des projets 2024 - programme COOP'TER. Repéré à https://"},{"n":"TERA - EPICERIE ALVEOLE","r":"France (Nouvelle-Aquitaine)","co":"France","ac":"OUI","cl":"Particulier","sz":"Micro","sv":["Distribution & Négoce","Prestation Intellectuelle"],"ty":[],"oi":"Épicerie distribuant de l'\"alimentation à minimum de 85% locale, respecteuse de l'environnement et rémunérée en monnaie locale.\" (IEFC, 2022) + offre de visite de l'association avec participation fina","oe":"Co-construction d'un écosystème coopératif, gamme de produits élargie, réseau de producteurices locaux.les agrandit, soutient de la monnaie citoyenne locale","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Journalière","env":"Positif","soc":"Valeur ajoutée","fo":0,"fe":3,"fh":0,"ft":0,"fi":1,"fr":0,"lo":3,"le":3,"lh":2,"lt":3,"li":2,"lr":1,"ln":1,"src":"IEFC (2022)\nTERA T. (s.d.). TERA, un écosystème pour le XXIème siècle. Repéré à "},{"n":"ISOVATION","r":"France (Provence)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Distribution & Négoce","Prestation Intellectuelle","Service Numérique"],"ty":["SAV & Maintenance","Conseil & Formation"],"oi":"Vente d'emballages isothermes","oe":"Propose des emballages isothermes 100% recyclés et 100% recyclables (pas de récupération des produits) et des solutions sur mesure, formations, tracking de la localisation et de la température des pro","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Journalière","env":"Positif","soc":"Valeur ajoutée (création d'emplois locaux, formation, ...)","fo":0,"fe":0,"fh":1,"ft":1,"fi":0,"fr":0,"lo":2,"le":3,"lh":1,"lt":6,"li":0,"lr":1,"ln":1,"src":"IEFC (2022)\nLe site Isovation (https://www.isovation.com/)\nIsovation I. (s.d.). "},{"n":"SAPOVAL","r":"France (Occitanie)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Maintenance & Réparation","Prestation Intellectuelle","Service Numérique","Gestion de Déchets"],"ty":["SAV & Maintenance","Conseil & Formation","Externalisation"],"oi":"Étude et mise en oeuvre de \"technologies innovantes pour le management des eaux usées et des sous-produits associés\" + \"simplifier l'exploitation de leurs stations [...] , respecter leurs obligations ","oe":"Propose des solutions sur mesure pour le traitement et la valorisation des eaux usées : conseil, formation, assistance, suivi et maintenance des installations, gestion et valorisation des sous-produit","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Au projet i guess","env":"Positif","soc":"Valeur ajoutée (création d'emplois locaux, formation, ...)","fo":0,"fe":0,"fh":0,"ft":0,"fi":1,"fr":1,"lo":2,"le":4,"lh":4,"lt":7,"li":1,"lr":2,"ln":1,"src":"IEFC (2022)\nLe site Sapoval (https://sapoval.fr/)\nSapoval S. (s.d.). Sapoval : c"},{"n":"Dumont Energies","r":"France","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Maintenance & Réparation"],"ty":["Externalisation","Vente de Performance"],"oi":"Vente équipement","oe":"Solution orientée performance d’usage","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Manque d'information","rep":1,"rec":"Récurrence contrats long terme","env":"Positif","soc":"Lutte contre la précarité en baissant les factures des habitants et en les accompagnant pour mieux gérer leur confort.","fo":1,"fe":1,"fh":1,"ft":1,"fi":0,"fr":0,"lo":1,"le":0,"lh":1,"lt":3,"li":0,"lr":0,"ln":1,"src":"Dumont Nickel. (2025, 18 février). DUMONT NICKEL : un projet structurant… [Commu"},{"n":"TOTEM.mobi","r":"France","co":"France","ac":"NON","cl":"particulier/organisation","sz":"Petite","sv":["Service Numérique","Logistique & Transport"],"ty":["Location / Leasing","Vente de Performance"],"oi":"Vente de produits ","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"Manque d'information","rep":1,"rec":"Récurrence services associés","env":"Positif","soc":"n/a","fo":1,"fe":2,"fh":0,"ft":1,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":2,"li":0,"lr":0,"ln":1,"src":"ADEME. (2020). Expériences d'entreprises en économie de la fonctionnalité : Recu"},{"n":"AMZAIR INDUSTRIE","r":"France (Bretagne)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Conception & Fabrication","Maintenance & Réparation","Service Numérique"],"ty":["SAV & Maintenance","Externalisation"],"oi":"Conception, fabrication avec ressources majoritairement locales et vente de PAC\nInstallateurs tiers pouvant entraîner une baisse de performance à l'installation\nPas de contrat d'entretien","oe":"Co-conception avec les installateurs qui connaissent le besoin et l'usage client, fabrication avec ressources majoritairement locales, installation et entretien-maintenance sur mesure de PAC pendant 1","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Au contrat (long terme)","env":"Positif","soc":"Valeur ajoutée","fo":0,"fe":1,"fh":1,"ft":1,"fi":1,"fr":0,"lo":3,"le":3,"lh":3,"lt":6,"li":1,"lr":2,"ln":1,"src":"ADEME (2020)\nLe site Groupe Airwell (https://groupe-airwell.com/)"},{"n":"La butinerie","r":"France (Île-de-France)","co":"France","ac":"OUI","cl":"Particulier","sz":"Micro","sv":["Distribution & Négoce","Prestation Intellectuelle","Gestion de Déchets"],"ty":["Conseil & Formation","Mutualisation / Partage","Externalisation"],"oi":"Non applicable, créé avec EFC","oe":"N/A","vb":1,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Journalière : vente produits Biocoop ; hebdomadaire : repas à prix libre (4 fois par semaine)","env":"Positif","soc":"Très positive","fo":1,"fe":2,"fh":0,"ft":0,"fi":1,"fr":1,"lo":5,"le":3,"lh":3,"lt":8,"li":2,"lr":1,"ln":1,"src":"TERRES EFC ÎLE-DE-France T. Î. (s.d.). FICHE TRAJECTOIRE LA BUTINERIE.\nLe site L"},{"n":"SPHERATEST ENVIRONNEMENT","r":"Montréal","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Vente de services","oe":"Accompagnement environnemental orienté usages","vb":0,"vu":0,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":0,"rec":"Projets environnementaux récurrents","env":"Décontamination sols","soc":"Coopération locale renforcée","fo":3,"fe":2,"fh":1,"ft":0,"fi":0,"fr":0,"lo":1,"le":1,"lh":2,"lt":4,"li":0,"lr":0,"ln":0,"src":"• EFC QUEBEC. (2021). OFFRIR UN SERVICE DE QUALITÉ ADAPTÉ AUX BESOINS DU CLIENT."},{"n":"ESTER","r":"Occitanie (Toulouse)","co":"France","ac":"OUI","cl":"Particulier","sz":"Moyenne","sv":["Maintenance & Réparation","Prestation Intellectuelle","Gestion de Déchets"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Ingénierie environnementale classique","oe":"Accompagnement territorial durable","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Récurrence services associés","env":"Contribution à la mobilité durable et diminution du recours aux véhicules privés.","soc":"N/A","fo":1,"fe":1,"fh":1,"ft":1,"fi":0,"fr":0,"lo":1,"le":1,"lh":0,"lt":3,"li":0,"lr":0,"ln":0,"src":"ADEME. (2020). Expériences d’entreprises en économie de la fonctionnalité. Agenc"},{"n":"LOGIC INTERIM","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Prestation Intellectuelle","Service à la Personne"],"ty":["Conseil & Formation"],"oi":"Intérim classique (facturation au volume)","oe":"Solution RH intégrée (accompagnement + compétences)","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Contrats d’accompagnement","env":"réduction déplacements inutiles + stabilisation emplois","soc":"insertion, montée en compétences","fo":5,"fe":3,"fh":2,"ft":2,"fi":0,"fr":0,"lo":1,"le":1,"lh":1,"lt":2,"li":0,"lr":0,"ln":0,"src":"ADEME. (2020). Expériences d’entreprises en économie de la fonctionnalité. Agenc"},{"n":"PUBLI RELIEF","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Conception & Fabrication","Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle"],"ty":["Location / Leasing","Vente de Performance"],"oi":"Vente d’enseignes au projet","oe":"Abonnement / performance d’usage","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Vente unique","env":"réduction déchets + meilleure durabilité des supports","soc":"soutien aux commerces / relation durable","fo":3,"fe":2,"fh":1,"ft":1,"fi":0,"fr":1,"lo":1,"le":0,"lh":0,"lt":2,"li":0,"lr":1,"ln":0,"src":"ADEME. (2020). Expériences d’entreprises en économie de la fonctionnalité. Agenc"},{"n":"AGENCE EVEREST","r":"Bretagne","co":"France","ac":"OUI","cl":"Particulier","sz":"Micro","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Vente de services","oe":"abonnement (performance)","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Récurrence contrats long terme","env":"sobriété communication (moins de supports inutiles)","soc":"autonomie des équipes clientes","fo":1,"fe":0,"fh":2,"ft":2,"fi":0,"fr":0,"lo":2,"le":0,"lh":2,"lt":1,"li":0,"lr":1,"ln":0,"src":"ADEME. (2020). Expériences d’entreprises en économie de la fonctionnalité. Agenc"},{"n":"Servi Doryl","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Vente de produits","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"Récurrence contrats long terme","env":"N/A","soc":"N/A","fo":1,"fe":2,"fh":0,"ft":0,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":2,"li":0,"lr":0,"ln":1,"src":"IE-EFC, & ADEME. (2022). Expériences d'EFC : Livret des Universités de l’Économi"},{"n":"GESNORD","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Vente de services","oe":"Solution orientée performance d’usage","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Manque d'information","rep":0,"rec":"N/A","env":"N/A","soc":"N/A","fo":2,"fe":1,"fh":0,"ft":0,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":1,"li":0,"lr":0,"ln":1,"src":"IE-EFC, & ADEME. (2022). Expériences d'EFC : Livret des Universités de l’Économi"},{"n":"RELAIS D’ENTREPRISES","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Location / Leasing","Mutualisation / Partage"],"oi":"Location d’espaces + accompagnement","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En développement","rep":0,"rec":"abonnements","env":"positif (réduction déplacements)","soc":"élevée (territoires, emploi local)","fo":2,"fe":3,"fh":2,"ft":1,"fi":1,"fr":0,"lo":2,"le":2,"lh":2,"lt":2,"li":1,"lr":1,"ln":1,"src":"https://www.relais-entreprises.fr/"},{"n":"SOFAXIS - GROUPE RELYENS","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"assurance + conseil","oe":"Solution orientée performance d’usage","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"contrats à long terme","env":"indirect","soc":"très élevée (santé, secteur public)","fo":2,"fe":2,"fh":1,"ft":1,"fi":0,"fr":0,"lo":2,"le":1,"lh":1,"lt":3,"li":1,"lr":1,"ln":1,"src":"-"},{"n":"ANA BELL GROUP","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Micro","sv":["Prestation Intellectuelle"],"ty":["Mutualisation / Partage"],"oi":"produits industriels (via Sofraser)","oe":"très avancée","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":0,"rec":"Vente unique et abonnements","env":"très positif (réduction déchets, consommation)","soc":"élevée","fo":1,"fe":2,"fh":1,"ft":1,"fi":0,"fr":0,"lo":2,"le":1,"lh":1,"lt":3,"li":1,"lr":1,"ln":0,"src":"https://anabellgroup.com/  IEFC (2022)"},{"n":"ENERCIPA","r":"France (Provence-Alpes-Côte d’Azur – Avignon)","co":"France","ac":"oui","cl":"Organisation privé","sz":"Grande","sv":["Maintenance & Réparation","Prestation Intellectuelle"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"Accompagnement et coordination de projets d’énergie citoyenne et renouvelable","oe":"Solution énergétique territoriale collaborative orientée performance énergétique et transition écologique","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En développement","rep":0,"rec":"Revenus récurrents (service énergétique continu","env":"Réduction des émissions et développement d’énergie renouvelable locale","soc":"élevée","fo":4,"fe":5,"fh":4,"ft":2,"fi":1,"fr":1,"lo":5,"le":3,"lh":5,"lt":6,"li":2,"lr":2,"ln":1,"src":"IE-EFC, & ADEME. (2022). Expériences d'EFC : Livret des Universités de l’Économi"},{"n":"AÉNÉIS","r":"Hauts-de-France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Micro","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"accompagner les structures privées et publiques dans leur développement et projets stratégiques.","oe":"Poursuivre l'offre initiale tout en ayant en ancrage permanent au sein de l'écosystème lillois ","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Manque d'information","rep":0,"rec":"Contrats d'accompagnement ","env":"non applicable ","soc":"Coopération territoriale renforcée","fo":1,"fe":1,"fh":2,"ft":0,"fi":0,"fr":0,"lo":3,"le":0,"lh":1,"lt":4,"li":1,"lr":0,"ln":1,"src":"n/a"},{"n":"BAOBAB","r":"Pays de la Loire (Saint- Nazaire)","co":"Autre","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"non mentionné ","oe":"Accompagnement territorial durable","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Contrats d'accompagnement ","env":"non applicable ","soc":"Coopération territoriale renforcée","fo":0,"fe":1,"fh":0,"ft":0,"fi":0,"fr":0,"lo":3,"le":1,"lh":3,"lt":3,"li":2,"lr":0,"ln":1,"src":"n/a"},{"n":"DeltaGomma","r":"Cowansville,\nBrome-Missisquoi","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Gestion de Déchets"],"ty":["SAV & Maintenance","Externalisation"],"oi":"Vente de produits","oe":"Solution intégrée coopérative","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Contrats récurrents services","env":"Valorisation matières recyclées","soc":"Coopération locale renforcée","fo":2,"fe":4,"fh":3,"ft":2,"fi":0,"fr":0,"lo":0,"le":1,"lh":3,"lt":4,"li":2,"lr":0,"ln":0,"src":"•\tDeltaGomma Inc. (s. d.). quebeccirculaire.org. Consulté 16 février 2026, à l’a"},{"n":"MaltBroue inc.","r":"Témiscouata-sur-le-Lac (Québec)","co":"Québec","ac":"oui","cl":"organisation privé","sz":"Petite","sv":["Transformation de Matière","Distribution & Négoce"],"ty":["Conseil & Formation","Location / Leasing","Externalisation"],"oi":"Vente de produits","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Contrats performance récurrents","env":"positif","soc":"élevée (mobilité inclusive)","fo":2,"fe":6,"fh":0,"ft":1,"fi":2,"fr":1,"lo":3,"le":2,"lh":4,"lt":9,"li":2,"lr":0,"ln":1,"src":"•\tEFC QUEBEC. (2021). FACILITER L’APPROVISIONNEMENT EN GRAINS ET EN MALTS AUPRÈS"},{"n":"ESCAPADE LIBERTE MOBILITE (ELM)","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Maintenance & Réparation","Logistique & Transport"],"ty":["SAV & Maintenance","Location / Leasing","Mutualisation / Partage"],"oi":"mise à disposition de véhicules","oe":"usage","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"location","env":"modéré (mutualisation des véhicules)","soc":"élevée (mobilité inclusive)","fo":1,"fe":1,"fh":1,"ft":1,"fi":0,"fr":0,"lo":1,"le":1,"lh":2,"lt":2,"li":1,"lr":1,"ln":0,"src":" IEFC (2022)"},{"n":"PHENIX","r":"France (Centre-Val de Loire)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Conception & Fabrication","Maintenance & Réparation","Gestion de Déchets"],"ty":["Conseil & Formation","Mutualisation / Partage","Externalisation","Vente de Performance"],"oi":"solution anti-gaspillage","oe":"n/a","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"n/a","env":"positif ","soc":"élevée","fo":1,"fe":1,"fh":2,"ft":0,"fi":1,"fr":0,"lo":1,"le":1,"lh":1,"lt":3,"li":1,"lr":0,"ln":0,"src":"https://www.wearephenix.com/#:~:text=Pourquoi%20revaloriser%20ses%20invendus%20a"},{"n":"FABT - Bien vieillir sur son territoire","r":"France","co":"France","ac":"OUI","cl":"Organisation public","sz":"Micro","sv":["Prestation Intellectuelle","Service à la Personne"],"ty":["Conseil & Formation","Mutualisation / Partage","Externalisation"],"oi":"services aux personnes","oe":"n/a","vb":0,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En développement","rep":0,"rec":"(financement public)","env":"indirect","soc":"extrêmement élevée","fo":1,"fe":1,"fh":1,"ft":1,"fi":1,"fr":0,"lo":1,"le":1,"lh":1,"lt":1,"li":1,"lr":1,"ln":0,"src":"COOP'TER - Auvergne-Rhône-Alpes"},{"n":"VENT D'OUEST - Comm'un quartier en transition","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Micro","sv":["Maintenance & Réparation","Prestation Intellectuelle"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"accompagnement / animation","oe":"n/a","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"n/a","env":"élevé","soc":"élevée","fo":2,"fe":1,"fh":1,"ft":1,"fi":0,"fr":0,"lo":1,"le":1,"lh":2,"lt":1,"li":1,"lr":1,"ln":0,"src":"COOP'TER - Auvergne-Rhône-Alpes"},{"n":"GREENWHEELS","r":"Pays-Bas","co":"Autre","ac":"OUI","cl":"Particulier","sz":"Moyenne","sv":["Maintenance & Réparation"],"ty":["Location / Leasing"],"oi":"location de voiture traditionnel ","oe":"Proposer un service d'autopartage en collaboration avec le gouvernement ","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"à chaque fois qu'un utilisateur loue une voiture","env":"moins de voitures en circulation, plus de transport public,","soc":"Stabilité économique","fo":0,"fe":1,"fh":0,"ft":0,"fi":0,"fr":0,"lo":3,"le":2,"lh":0,"lt":0,"li":2,"lr":0,"ln":0,"src":"n/a"},{"n":"ROLLS-ROYCE","r":"Angleterre","co":"Autre","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Conception & Fabrication","Distribution & Négoce","Maintenance & Réparation","Gestion de Déchets"],"ty":["SAV & Maintenance","Location / Leasing","Vente de Performance"],"oi":" fabrication et vente de moteurs aéronautiques","oe":"Vente de l'utilisation de moteur d'avion ","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":1,"rec":"Au nombre d'heure d'utilisation","env":"Réduction des déchets via la maintenance ","soc":"Stabilité économique","fo":0,"fe":1,"fh":1,"ft":1,"fi":0,"fr":0,"lo":2,"le":3,"lh":1,"lt":2,"li":0,"lr":0,"ln":0,"src":"n/a"},{"n":"MARMOTT ÉNERGIES","r":"Canada, Quebec","co":"Québec","ac":"OUI","cl":"Particulier","sz":"Petite","sv":["Conception & Fabrication","Maintenance & Réparation"],"ty":["SAV & Maintenance","Vente de Performance"],"oi":"Vente thermopompe","oe":"Vente de l'utilisation de la thermopompe","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":1,"rec":"Par abonnement ","env":"Réduction des émissions de CO2","soc":"rendre la géothermie accessible sans coût initial, pour les foyers utilisant encore du mazout ou du gaz","fo":0,"fe":1,"fh":1,"ft":1,"fi":0,"fr":0,"lo":3,"le":3,"lh":1,"lt":2,"li":0,"lr":0,"ln":0,"src":"n/a"},{"n":"COOPÉRATIVE CITOYENNE COMBRAILLES DURABLES","r":"Auvergne Rhône Alpes","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Conception & Fabrication","Maintenance & Réparation","Prestation Intellectuelle"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"non mentionné ","oe":"Produire de l'électricité pour et par les citoyens ","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"mensuel","env":"Réduction des émissions de CO2","soc":"Le projet est réaliser pour les citoyens par les citoyens ","fo":0,"fe":0,"fh":0,"ft":0,"fi":1,"fr":0,"lo":3,"le":2,"lh":3,"lt":5,"li":2,"lr":0,"ln":1,"src":"n/a"},{"n":"SINEO","r":"Occitanie, France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Maintenance & Réparation"],"ty":["Vente de Performance"],"oi":"Nettoyage de véhicule d'occasion","oe":"Réparation de véhicule d'occasion depuis 2021","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Fidélisation clients, services aux entreprises","env":"Produits écologiques, nettoyage sans eau, réparation","soc":"Réinsertion professionnelle","fo":1,"fe":1,"fh":1,"ft":0,"fi":0,"fr":0,"lo":2,"le":2,"lh":3,"lt":2,"li":1,"lr":1,"ln":0,"src":"IEEFC (2022)"},{"n":"Recyclage Vanier","r":"Canada, Quebec","co":"Québec","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Prestation Intellectuelle","Logistique & Transport","Gestion de Déchets"],"ty":["Conseil & Formation","Externalisation"],"oi":"Vente de services","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"Contrats récurrents services","env":"Réduction ressources consommées","soc":"Coopération clients renforcée","fo":3,"fe":5,"fh":3,"ft":3,"fi":2,"fr":1,"lo":4,"le":4,"lh":3,"lt":7,"li":2,"lr":2,"ln":1,"src":"•\tÉtat des renseignements—Revenu Québec. (s. d.)https://www.registreentreprises."},{"n":"ASCOREL","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Conception & Fabrication","Maintenance & Réparation","Service Numérique"],"ty":["SAV & Maintenance"],"oi":"Vente électronique industrielle","oe":"Paiement à l'usage des électroniques","vb":0,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"Manque d'information","rep":1,"rec":"Vente unique et abonnements","env":"n/a","soc":"n/a","fo":1,"fe":3,"fh":1,"ft":0,"fi":0,"fr":0,"lo":2,"le":2,"lh":2,"lt":2,"li":1,"lr":0,"ln":0,"src":"- IEEFC (2022). Expériences d’Économie de la Fonctionnalité et de la Coopération"},{"n":"ARGENTA INTERCOM","r":"France","co":"France","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Transformation de Matière"],"ty":["Conseil & Formation"],"oi":"Évacuer et traiter les volumes produits (plus il y a de volume, plus l'activité est grande)","oe":"On ne cherche plus à gérer des déchets, mais à réduire leur production tout en soutenant l'économie locale","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"élevé","env":"positif","soc":"élevée(sensibilisation, inclusion , territoire)","fo":4,"fe":3,"fh":1,"ft":1,"fi":0,"fr":0,"lo":3,"le":0,"lh":2,"lt":4,"li":1,"lr":0,"ln":0,"src":"https://etsmtl365.sharepoint.com/sites/env82501procdsetprocessuspropresh2026/Doc"},{"n":"Sky computing","r":"France","co":"France","ac":"NON","cl":"Organisation privé","sz":"Petite","sv":["Prestation Intellectuelle","Service Numérique"],"ty":["Location / Leasing","Mutualisation / Partage"],"oi":"Vente de produits","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"n/a","env":"positif","soc":"n/a","fo":1,"fe":0,"fh":1,"ft":1,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":2,"li":0,"lr":0,"ln":1,"src":"Sciences Computers Consultants. (2022). Affranchir le client de la contrainte de"},{"n":"Morpho","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Micro","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Vente de services","oe":"Solution orientée performance d’usage","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Manque d'information","rep":0,"rec":"n/a","env":"positif","soc":"n/a","fo":0,"fe":1,"fh":1,"ft":0,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":2,"li":0,"lr":0,"ln":1,"src":"Morpho Architectes. (2022). Un modèle en phase avec les enjeux environnementaux "},{"n":"Electro calorifique","r":"France","co":"France","ac":"NON","cl":"Organisation public","sz":"Grande","sv":["Conception & Fabrication"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing"],"oi":"Vente d`équipements","oe":"Solution intégrée coopérative","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"Manque d'information","rep":1,"rec":"n/a","env":"n/a","soc":"n/a","fo":1,"fe":1,"fh":0,"ft":1,"fi":0,"fr":0,"lo":0,"le":0,"lh":1,"lt":2,"li":0,"lr":0,"ln":0,"src":"Electro Calorique. (2023). Un modèle économique au service de l'innovation : Off"},{"n":"Je suis reco","r":"France","co":"France","ac":"NON","cl":"particulier/organisation","sz":"Petite","sv":["Service Numérique"],"ty":["SAV & Maintenance"],"oi":"Vente de services","oe":"Solution orientée usage durable","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Manque d'information","rep":1,"rec":"n/a","env":"positif","soc":"n/a","fo":1,"fe":0,"fh":0,"ft":2,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":4,"li":0,"lr":0,"ln":1,"src":"JeSuisReco. (2024). Une assurance simple et éco-responsable destinée aux particu"},{"n":"Archi Possible","r":"Île-de-France","co":"France","ac":"oui","cl":"Particulier","sz":"Petite","sv":["Prestation Intellectuelle","Service à la Personne"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"Accompagnement classique à l’auto-réhabilitation / conseil en habitat","oe":"Offre fondée sur l’accompagnement, la montée en compétence des habitants, la coopération territoriale et la valeur d’usage du logement plutôt que sur la simple vente d’une prestation standard","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"n/a","env":"n/a","soc":"n/a","fo":3,"fe":5,"fh":5,"ft":3,"fi":2,"fr":1,"lo":4,"le":4,"lh":4,"lt":7,"li":2,"lr":1,"ln":1,"src":"IEEFC / COOPTER (2022), étude de cas Archi Possible . https://archipossible.fr"},{"n":"Établissements André CROS","r":"Auvergne-Rhône-Alpes (Isère)","co":"France","ac":"oui","cl":"Organisation public","sz":"Moyenne","sv":["Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing","Externalisation","Vente de Performance"],"oi":"Vente + location + maintenance d’équipements","oe":"Fourniture d’air comprimé avec engagement de performance","vb":1,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"n/a","env":"n/a","soc":"n/a","fo":3,"fe":5,"fh":4,"ft":2,"fi":0,"fr":1,"lo":4,"le":3,"lh":3,"lt":7,"li":2,"lr":1,"ln":1,"src":"ADEME (2020), Étude de cas Établissements André CROS"},{"n":"PARC NATUREL RÉGIONAL DES BALLONS DES VOSGES","r":"Grand Est / Bourgogne-Franche-Comté","co":"France","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Transformation de Matière","Distribution & Négoce","Prestation Intellectuelle"],"ty":["Vente de Performance"],"oi":"Gestion environnementale / forestière classique","oe":"Animation d’un écosystème territorial autour du bois local","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Financement public + projets territoriaux","env":"Gestion durable des forêts + valorisation bois local","soc":"Dynamisation du territoire + emplois locaux + coopération","fo":5,"fe":5,"fh":4,"ft":2,"fi":0,"fr":0,"lo":2,"le":1,"lh":0,"lt":1,"li":0,"lr":0,"ln":0,"src":"Parc naturel régional des Ballons des Vosges. (s.d.). Accueil. Consulté le 10 ma"},{"n":"Les Anges Gardins","r":"Hauts-de-France","co":"France","ac":"oui","cl":"particulier/organisation","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Distribution & Négoce","Prestation Intellectuelle","Service à la Personne","Logistique & Transport","Gestion de Déchets"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"production et vente alimentaire","oe":"système alimentaire multifonctionnel basé sur les services","vb":1,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Subventions publiques + vente de produits + ateliers et formations","env":"Alimentation durable + circuits courts + valorisation des sols et biodiversité","soc":"Insertion professionnelle + éducation à l’alimentation + cohésion sociale territoriale","fo":4,"fe":5,"fh":4,"ft":3,"fi":2,"fr":0,"lo":5,"le":4,"lh":4,"lt":8,"li":2,"lr":1,"ln":1,"src":"n/a"},{"n":"Michelin","r":"Auvergne-Rhône-Alpes","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Conception & Fabrication","Transformation de Matière","Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle","Service Numérique","Logistique & Transport"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing","Externalisation","Vente de Performance"],"oi":"vente de pneumatiques","oe":"vente de kilomètres parcourus / performance","vb":1,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Contrats de performance basés sur les kilomètres parcourus + services associés (maintenance, gestion de flotte)","env":"Réduction de la consommation de matière et d’énergie + allongement de la durée de vie des pneus (rechapage, optimisation usage)","soc":"Amélioration de la sécurité et des conditions de transport + création d’emplois qualifiés et services associés","fo":2,"fe":9,"fh":5,"ft":1,"fi":0,"fr":1,"lo":4,"le":4,"lh":4,"lt":8,"li":0,"lr":2,"ln":1,"src":"ADEME (2020), Michelin"},{"n":"IFPEB (Projet Bureau de demain)","r":"France","co":"France","ac":"oui","cl":"Organisation privé","sz":"N/A","sv":["Conception & Fabrication","Transformation de Matière","Maintenance & Réparation","Prestation Intellectuelle","Gestion de Déchets"],"ty":["SAV & Maintenance","Conseil & Formation","Mutualisation / Partage","Externalisation","Vente de Performance"],"oi":"modèle classique du bâtiment","oe":"modèle basé sur performance d’usage et circularité","vb":1,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"Financements publics  + projets collaboratifs + prestations associées","env":"Réduction des déchets du bâtiment + réutilisation des matériaux + baisse des émissions de GES","soc":"Amélioration du bien-être au travail + coopération entre acteurs + transformation des pratiques","fo":5,"fe":7,"fh":6,"ft":4,"fi":2,"fr":1,"lo":6,"le":5,"lh":4,"lt":11,"li":2,"lr":3,"ln":1,"src":"ADEME (2020), IFPEB – Bureau de demain"},{"n":"Dumoulin Bicyclettes","r":"Montréal","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Distribution & Négoce","Maintenance & Réparation","Service à la Personne"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing","Mutualisation / Partage"],"oi":"Ventes de biens","oe":"Solution orienté usage durable avec partage de vélos","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"Non Abouti","rep":1,"rec":"Contrats performance récurrents","env":"Amélioration de la sécurité et des conditions de transport + création d’emplois qualifiés et services associés","soc":"Coopération locale renforcée","fo":2,"fe":2,"fh":2,"ft":2,"fi":1,"fr":0,"lo":3,"le":1,"lh":2,"lt":4,"li":1,"lr":0,"ln":1,"src":"\tEfc quebec. (2021). OFFRIR DES SOLUTIONS CYCLISTES FAVORISANT L’ADOPTION ET LA "},{"n":"SOLEV","r":"Provence-Alpes-Côte d’Azur","co":"Autre","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Prestation paysagère esthétique classique","oe":"Performance environnementale Eet prévention des risques ferroviaires","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Contrats pluriannuels d’entretien","env":"sobriété communication (moins de supports inutiles)","soc":"autonomie des équipes clientes","fo":2,"fe":8,"fh":3,"ft":1,"fi":0,"fr":1,"lo":3,"le":3,"lh":3,"lt":5,"li":1,"lr":0,"ln":1,"src":"ADEME(2020),SOLEV"},{"n":"SERGIC","r":"Hauts-de-France (Lille)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Grande","sv":["Prestation Intellectuelle","Service Numérique"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Gestion immobilière administrative","oe":"Gestion immobilière administrative","vb":0,"vu":0,"vp":1,"ef":1,"efc":0,"mat":"En développement","rep":0,"rec":"Honoraires récurrents (% loyers / syndic)","env":"Optimisation énergétique du parc immobilier","soc":"Stabilité logement & emploi local","fo":2,"fe":5,"fh":3,"ft":1,"fi":0,"fr":1,"lo":2,"le":2,"lh":3,"lt":4,"li":0,"lr":1,"ln":1,"src":"ADEME(2020),SERGIC"},{"n":"TTEN","r":"Normandie","co":"France","ac":"OUI","cl":"Organisation public","sz":"Moyenne","sv":["Maintenance & Réparation","Prestation Intellectuelle"],"ty":[],"oi":"Ravalement au m²","oe":"Performance esthétique & qualité perçue","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"Récurrence par fidélisation client","env":"Prolongation de la durée de vie des bâtiments","soc":"Autonomie et engagement des équipes","fo":5,"fe":6,"fh":4,"ft":2,"fi":0,"fr":0,"lo":3,"le":1,"lh":4,"lt":4,"li":0,"lr":0,"ln":1,"src":"IEEFC (2022)"},{"n":"LDE","r":"Grand Est( Alsace)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Moyenne","sv":["Distribution & Négoce","Service Numérique"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"\nDistribution de livres scolaires","oe":"Solutions éducatives numériques avec accompagnement","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Contrats annuels récurrents par établissement","env":"Réduction du gaspillage de ressources pédagogiques","soc":"Amélioration de la réussite scolaire et coopération locale","fo":5,"fe":8,"fh":5,"ft":2,"fi":1,"fr":0,"lo":5,"le":4,"lh":3,"lt":8,"li":2,"lr":0,"ln":1,"src":"ADEME. (2020). Expériences d’entreprises en économie de la fonctionnalité. Agenc"},{"n":"RIDEL ENERGY","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Distribution & Négoce","Prestation Intellectuelle","Service Numérique"],"ty":["SAV & Maintenance","Conseil & Formation","Vente de Performance"],"oi":"Vente d’énergie / installation technique","oe":"Vente de performance énergétique locale","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Contrats pluriannuels de performance énergétique","env":"Réduction émissions & production énergie locale","soc":"Transition énergétique & emploi local","fo":5,"fe":8,"fh":7,"ft":2,"fi":2,"fr":1,"lo":6,"le":5,"lh":4,"lt":11,"li":2,"lr":2,"ln":1,"src":"ADEME(2020),RIDEL ENERGY"},{"n":"QUADRA DIFFUSION","r":"Hauts-de-France","co":"France","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Prestation Intellectuelle","Service Numérique"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"Vente de progiciels de gestion des panneaux d’affichage publicitaire urbain (modèle licence)","oe":"Passage d’un modèle licence à un modèle SaaS facturé à l’usage, centré sur la valeur des données et l’utilité pour les acteurs du territoire.","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Revenus récurrents liés à l’usage continu du logiciel","env":"Impact environnemental faible mais positif (service numérique optimisant les flux et équipements).","soc":"Création de valeur sociale par l’amélioration de la gestion urbaine et la coopération avec les acteurs territoriaux","fo":5,"fe":6,"fh":5,"ft":1,"fi":1,"fr":1,"lo":6,"le":4,"lh":3,"lt":6,"li":2,"lr":1,"ln":1,"src":"n/a"},{"n":"MININOO","r":"france","co":"Autre","ac":"OUI","cl":"Organisation privé","sz":"Micro","sv":["Prestation Intellectuelle","Service Numérique"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Accompagnement direct des futures mamans de la grossesse au post-partum","oe":"Service d’accompagnement de la maternité co-construit avec les entreprises pour soutenir leurs salariées","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Revenus récurrents via forfaits d’accompagnement par salariée et par entreprise","env":"Impact environnemental faible (service immatériel)","soc":"Amélioration du bien-être des salariées et facilitation du retour au travail.","fo":4,"fe":5,"fh":4,"ft":2,"fi":1,"fr":0,"lo":2,"le":2,"lh":4,"lt":6,"li":2,"lr":0,"ln":1,"src":"IEEFC (2022). Expériences d’Économie de la Fonctionnalité et de la Coopération ("},{"n":"CULTURE MIEL","r":"Centre Val de Loire","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle","Service Numérique","Service à la Personne","Logistique & Transport"],"ty":["SAV & Maintenance","Conseil & Formation"],"oi":"Vente et distribution de miels et produits de la ruche","oe":"Valorisation du miel local dans une logique coopérative territoriale.","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"oui","env":"Amélioration de la biodiversité et réduction des transpor","soc":"Création de valeur territoriale et soutien aux producteurs locaux","fo":4,"fe":8,"fh":6,"ft":3,"fi":2,"fr":0,"lo":5,"le":3,"lh":3,"lt":8,"li":2,"lr":0,"ln":1,"src":"IEEFC (2022). Expériences d’Économie de la Fonctionnalité et de la Coopération ("},{"n":"ASSOCIATION CARMA - Une alimentation saine et durable pour toutes et tous","r":"Île-de-France","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Micro","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Actions locales et projets d’alimentation durable","oe":"Accompagnement continu des territoires vers la transition écologique","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"Non Abouti","rep":0,"rec":"Faible à moyenne","env":"Positif (réduction impact + alimentation durable)","soc":"Élevée (sensibilisation, inclusion, territoire)","fo":4,"fe":5,"fh":5,"ft":3,"fi":2,"fr":0,"lo":4,"le":2,"lh":4,"lt":7,"li":2,"lr":1,"ln":1,"src":""},{"n":"Un nouveau cycle","r":"France","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Micro","sv":["Maintenance & Réparation","Prestation Intellectuelle","Service à la Personne"],"ty":["SAV & Maintenance","Conseil & Formation","Mutualisation / Partage","Externalisation","Vente de Performance"],"oi":"Maintenance et réparation d’équipements électroménagers","oe":"Offre de service visant la performance d’usage durable des équipements","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"Revenus récurrents basés sur des services réguliers de maintenance préventive et d’accompagnement des usagers.","env":"Réduction des déchets électroménagers et de la consommation de ressources grâce à la prolongation de la durée de vie des équipements","soc":"Création d’emplois locaux, insertion professionnelle et renforcement du lien social autour d’usages plus durables","fo":6,"fe":7,"fh":6,"ft":3,"fi":2,"fr":1,"lo":6,"le":5,"lh":4,"lt":11,"li":2,"lr":2,"ln":1,"src":""},{"n":"POSITIVE DEGREE","r":"Saint-Lazare, Montérégie","co":"Autre","ac":"OUI","cl":"Organisation privé","sz":"Micro","sv":["Conception & Fabrication","Prestation Intellectuelle","Service Numérique"],"ty":["Externalisation","Vente de Performance"],"oi":"vente de services","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Contrats récurrents services","env":"Réduction consommation énergétique","soc":"Coopération locale renforcée","fo":4,"fe":6,"fh":5,"ft":3,"fi":0,"fr":2,"lo":4,"le":4,"lh":2,"lt":7,"li":2,"lr":3,"ln":1,"src":""},{"n":"BIXI","r":"Montréal","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Service à la Personne","Logistique & Transport"],"ty":["SAV & Maintenance","Mutualisation / Partage"],"oi":"Vente de services","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Abonnements","env":"Mobilité douce","soc":"Coopération locale renforcée","fo":2,"fe":2,"fh":2,"ft":1,"fi":1,"fr":0,"lo":2,"le":2,"lh":1,"lt":3,"li":2,"lr":0,"ln":0,"src":"- BIXI: https://www.abcmontreal.com/bixi-l-invention-montrealaise-qui-a-conquis-"},{"n":"CGP EXPAL","r":"Bromont, Canada","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Prestation Intellectuelle"],"ty":["SAV & Maintenance","Externalisation"],"oi":"Vente de produits","oe":"Collaboration à long terme, transparence","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Contrats récurrents services","env":"Réduction des ressources consomées","soc":"Coopération locale renforcée","fo":1,"fe":1,"fh":2,"ft":1,"fi":0,"fr":0,"lo":0,"le":1,"lh":3,"lt":2,"li":1,"lr":0,"ln":0,"src":"•\tÀ propos CGP Expal—CGP COATING INNOVATION - Technicoat Solution. (s. d.). Cons"},{"n":"Feuille d'Érable","r":"Sainte-Christine-d'Auvergne,  Québec, Canada","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Prestation Intellectuelle"],"ty":["SAV & Maintenance"],"oi":"Vente de produits","oe":"Diversification du modèle d'affaire avec le biochar","vb":1,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Récurrence services associés","env":"Valorisation résidus biomasse","soc":"Création valeur local","fo":1,"fe":3,"fh":1,"ft":1,"fi":1,"fr":1,"lo":1,"le":1,"lh":3,"lt":3,"li":1,"lr":1,"ln":0,"src":"•\tEFC QUEBEC. (2021). OFFRIR DES PRODUITS QUÉBÉCOIS INNOVANTS À PARTIR DE RESSOU"},{"n":"ASSOCIATION - Au bout de la rue","r":"Occitanie, France","co":"France","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Service à la Personne"],"ty":["Mutualisation / Partage"],"oi":"n/a","oe":"Mise en place d'un écosystème coopératif territorialisé","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Aucun revenu","env":"Mobilité douce","soc":"Relations humaines","fo":2,"fe":2,"fh":3,"ft":0,"fi":2,"fr":0,"lo":3,"le":1,"lh":2,"lt":2,"li":2,"lr":1,"ln":0,"src":"IEEFC - COOP'TER (2022)"},{"n":"ASSOCIATION - Les jardins de la voie romaine","r":"Centre Val de Loire, France","co":"France","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Transformation de Matière","Service à la Personne"],"ty":["Mutualisation / Partage"],"oi":"n/a","oe":"Produire localement et autrement","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Vente de produits bio de la ferme","env":"Agriculture biologique","soc":"Relations humaines","fo":2,"fe":4,"fh":2,"ft":1,"fi":2,"fr":0,"lo":2,"le":2,"lh":2,"lt":2,"li":2,"lr":2,"ln":0,"src":"IEEFC - COOP'TER (2022)"},{"n":"Enercoop","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Conception & Fabrication"],"ty":["Conseil & Formation","Mutualisation / Partage","Externalisation"],"oi":"Vente produit","oe":"Solution orientée usage durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Contrats performance récurrents","env":"Réduction ressources fossiles","soc":"Coopération locale renforcée/ relation client","fo":1,"fe":1,"fh":2,"ft":0,"fi":0,"fr":1,"lo":1,"le":1,"lh":3,"lt":1,"li":2,"lr":2,"ln":1,"src":"n/a"},{"n":"Promo Plastik","r":"Saint-Jean Port-Joli (Québec)","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Distribution & Négoce"],"ty":["SAV & Maintenance","Conseil & Formation"],"oi":"Vente de services","oe":"Solution intégrée coopérative","vb":0,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En développement","rep":0,"rec":"Récurrence services associés","env":"Production plus durable","soc":"Coopération locale renforcée","fo":1,"fe":4,"fh":3,"ft":3,"fi":2,"fr":0,"lo":4,"le":3,"lh":4,"lt":6,"li":2,"lr":0,"ln":1,"src":"•\tHome. (s. d.). Promo Plastik. https://promoplastik.com/en/\n•\tRessources—EFC Qu"},{"n":"AecopaQ inc.","r":"Quebec","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Prestation Intellectuelle","Logistique & Transport","Gestion de Déchets"],"ty":["SAV & Maintenance"],"oi":"Vente de produits","oe":"Solution orientée usage durable","vb":1,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Récurrence services associés","env":"Moins pollution emballages","soc":"Coopération territoriale renforcée","fo":5,"fe":5,"fh":3,"ft":1,"fi":2,"fr":1,"lo":5,"le":3,"lh":2,"lt":10,"li":2,"lr":3,"ln":1,"src":"•\tAecopaQ Inc. (s. d.). quebeccirculaire.org. Consulté 14 février 2026, à l’adre"},{"n":"ARECO","r":"France (Alpes-Maritimes)","co":"France","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Transformation de Matière","Distribution & Négoce","Maintenance & Réparation"],"ty":["SAV & Maintenance","Vente de Performance"],"oi":"Vente de produits","oe":"Solution orientée performance d’usage","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":1,"rec":"Récurrence contrats long terme","env":"Optimisation des ressources d'eau et réduction du gaspillage alimentaire","soc":"amélioration de la qualité des produits et coopération avec les acteurs locaux.","fo":2,"fe":3,"fh":0,"ft":1,"fi":0,"fr":0,"lo":3,"le":3,"lh":2,"lt":2,"li":0,"lr":1,"ln":0,"src":"ARECO"},{"n":"Groupe ERAM L'atelier BOCAGE","r":"France","co":"France","ac":"OUI","cl":"Particulier","sz":"Grande","sv":["Conception & Fabrication","Distribution & Négoce"],"ty":["SAV & Maintenance","Location / Leasing"],"oi":"Vente de produits et location","oe":"Contrats de location ","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":1,"rec":"Contrats","env":"Optimisation des ressources via utilisation de matériaux recyclés","soc":"Accessibilité et emploi local","fo":2,"fe":1,"fh":2,"ft":0,"fi":0,"fr":0,"lo":2,"le":1,"lh":3,"lt":4,"li":1,"lr":0,"ln":0,"src":"Groupe ERAM\nLa bocage"},{"n":"Groupe C. Laganière","r":"Quebec","co":"Québec","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Transformation de Matière","Prestation Intellectuelle","Gestion de Déchets"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Vente de services","oe":"Solutions de nettoyage écologique + accompagnement d’usage + co-création de produits locaux","vb":0,"vu":0,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":0,"rec":"Forfetaire","env":"Réduit l'enfouissement des matières en revalorisant les sols et décontamine les sols","soc":"Coopération locale renforcée","fo":2,"fe":1,"fh":2,"ft":0,"fi":1,"fr":0,"lo":2,"le":1,"lh":0,"lt":3,"li":1,"lr":1,"ln":0,"src":"Groupe C. Laganière"},{"n":"Mailhot Industrie","r":"Quebec, USA, Mexique","co":"Québec","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Conception & Fabrication","Maintenance & Réparation","Prestation Intellectuelle"],"ty":["SAV & Maintenance"],"oi":"Vente de vérin ","oe":"reconditionnement et réparation","vb":1,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"Non Abouti","rep":1,"rec":"Mixte vente de produits et forfetaire","env":"Réduit la production de vérins neufs","soc":"Savoir-faire industriel valorisé","fo":2,"fe":2,"fh":2,"ft":1,"fi":0,"fr":0,"lo":2,"le":2,"lh":2,"lt":2,"li":0,"lr":0,"ln":0,"src":"Mailhot industrie"},{"n":"Odyssee","r":"France","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Conception & Fabrication"],"ty":["Conseil & Formation"],"oi":"Vente de produits","oe":"Solution orientée performance d’usage","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"-","env":"positif","soc":"-","fo":1,"fe":5,"fh":1,"ft":0,"fi":0,"fr":0,"lo":3,"le":1,"lh":2,"lt":2,"li":1,"lr":0,"ln":1,"src":"Odyssée Environnement. (2024). Un traitement de l'eau qui fait faire des économi"},{"n":"Probiosphère","r":"Trois-rivière - Quebec","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Micro","sv":["Transformation de Matière","Prestation Intellectuelle","Gestion de Déchets"],"ty":["Externalisation","Vente de Performance"],"oi":"Vente de produits","oe":"Traitement de l'eau pour améliorer les conditions de vie et regénération de la biodiversité marine","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Contrats performance récurrents","env":"Réduit la pollution marine et regénère la biodiversité marine","soc":"Responsabilisation entreprises locales","fo":2,"fe":2,"fh":1,"ft":0,"fi":1,"fr":0,"lo":2,"le":0,"lh":2,"lt":1,"li":1,"lr":3,"ln":0,"src":"-"},{"n":"DM Compost","r":"France (Val de marne)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Micro","sv":["Conception & Fabrication","Transformation de Matière","Prestation Intellectuelle","Gestion de Déchets"],"ty":["Conseil & Formation","Location / Leasing"],"oi":"Vente et location de composteurs (silos, lombricomposteurs).","oe":"Accompagne le client pour transformer et revaloriser le déchet sur place","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Via forfait, prestation","env":"Valorise les déchets","soc":"Améliore la qualité de vie via la gestion des déchets","fo":1,"fe":2,"fh":1,"ft":0,"fi":1,"fr":0,"lo":1,"le":2,"lh":1,"lt":4,"li":1,"lr":2,"ln":0,"src":"-"},{"n":"kaeser kompressoren","r":"Allemagne","co":"Autre","ac":"OUI","cl":"particulier/organisation","sz":"Grande","sv":["Conception & Fabrication","Maintenance & Réparation"],"ty":[],"oi":"Vente de compresseurs","oe":"Fourniture d’air comprimé comme service","vb":1,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"-","env":" Réduction consommation énergétique","soc":"Sécurisation budgétaire des industriels","fo":2,"fe":1,"fh":2,"ft":1,"fi":0,"fr":1,"lo":1,"le":1,"lh":0,"lt":2,"li":0,"lr":0,"ln":0,"src":"Kaeser (site officiel)"},{"n":"Vestas","r":"Danemark (international)","co":"Autre","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Conception & Fabrication","Maintenance & Réparation"],"ty":[],"oi":"Vente d’éoliennes","oe":"Disponibilité & performance des parcs","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"contrats 10–20 ans","env":"Énergie renouvelable bas carbone","soc":"Emplois locaux, transition énergétique","fo":2,"fe":2,"fh":0,"ft":0,"fi":0,"fr":0,"lo":1,"le":1,"lh":1,"lt":0,"li":1,"lr":0,"ln":0,"src":"https://pdf.archiexpo.com/pdf/vestas/aom-4000/88087-134439.html"},{"n":"Assur-Ma","r":"France","co":"France","ac":"OUI","cl":"Particulier","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation"],"oi":"Assurance classique","oe":"Sécurisation, accompagnement du risque","vb":0,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"Manque d'information","rep":0,"rec":"NA","env":"-","soc":"-","fo":1,"fe":0,"fh":1,"ft":0,"fi":0,"fr":0,"lo":0,"le":0,"lh":0,"lt":1,"li":0,"lr":0,"ln":0,"src":"https://www.pappers.fr/entreprise/assur-ma-451106140"},{"n":"Quintessens","r":"France","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Moyenne","sv":["Prestation Intellectuelle","Service à la Personne"],"ty":["Conseil & Formation"],"oi":"Produits / conseil nutrition","oe":"Accompagnement alimentaire durable","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"NA","env":"Alimentation durable","soc":"Éducation alimentaire","fo":2,"fe":2,"fh":2,"ft":0,"fi":0,"fr":0,"lo":1,"le":1,"lh":0,"lt":0,"li":0,"lr":0,"ln":0,"src":"https://www.quintessens.fr/economie-de-la-fonctionnalite-et-de-la-cooperation/"},{"n":"Packinov","r":"Auvergne‑Rhône‑Alpes (Beynost, France)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Maintenance & Réparation"],"ty":["SAV & Maintenance","Mutualisation / Partage"],"oi":"Vente de machines de conditionnement","oe":"Solutions globales intégrant machine + service + optimisation de l’usage\n\n","vb":1,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En développement","rep":1,"rec":"maintenance, retrofit, services","env":"Réduction des pertes produit\nÉvitement du surdosage\nAllongement durée de vie machines","soc":"Amélioration des conditions de travail\nMontée en compétences des utilisateurs","fo":1,"fe":2,"fh":0,"ft":1,"fi":0,"fr":0,"lo":2,"le":2,"lh":0,"lt":2,"li":0,"lr":0,"ln":0,"src":"https://www.packinov.fr/"},{"n":"Association FashionGreenHub Grand Paris","r":"Île‑de‑France (Paris & Grand Paris)","co":"France","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Prestation Intellectuelle","Gestion de Déchets"],"ty":["Mutualisation / Partage","Externalisation"],"oi":"Sensibilisation","oe":"Écosystème coopératif territorialisé","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":1,"rec":"Adhésions\nProjets collectifs","env":"Forte réduction des textiles neufs\nAllongement durée de vie des vêtements","soc":"Insertion professionnelle\nFormation métiers de la réparation\nCréation d’emplois locaux\n\n\n","fo":1,"fe":1,"fh":2,"ft":0,"fi":0,"fr":0,"lo":2,"le":0,"lh":2,"lt":3,"li":0,"lr":0,"ln":0,"src":"https://www.fashiongreenhub.org/"},{"n":"TranKilis","r":"France (bourgogne Franche comté)","co":"France","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Maintenance & Réparation","Prestation Intellectuelle"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing"],"oi":"Vente d'équpement","oe":"Location de système, formation et maintenance","vb":0,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En place","rep":1,"rec":"NA","env":"-","soc":"Emplois locaux","fo":0,"fe":0,"fh":0,"ft":0,"fi":0,"fr":0,"lo":3,"le":1,"lh":1,"lt":6,"li":2,"lr":1,"ln":1,"src":"https://www.echamat-kernst.com/ https://www.trankilis.com/contact/"},{"n":"Abièze","r":"Quebec","co":"Québec","ac":"OUI","cl":"Particulier","sz":"Micro","sv":["Conception & Fabrication","Transformation de Matière","Distribution & Négoce"],"ty":["Mutualisation / Partage"],"oi":"NA","oe":"Vente de produits naturels et de proximité","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Adhésions; produits vendu","env":"Proposition de produits ménagers naturel et biodégradables","soc":"Emplois locaux","fo":0,"fe":2,"fh":1,"ft":1,"fi":0,"fr":1,"lo":2,"le":2,"lh":1,"lt":2,"li":2,"lr":1,"ln":0,"src":"abieze – abieze soap https://lesproduitsduquebec.com/entreprises-adherentes/abie"},{"n":"Intellinox","r":"Quebec","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Petite","sv":["Conception & Fabrication","Distribution & Négoce","Maintenance & Réparation","Prestation Intellectuelle","Service Numérique"],"ty":["SAV & Maintenance","Conseil & Formation","Externalisation"],"oi":"NA","oe":"Vente de système de ventillation connecté et personnalisé","vb":1,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En place","rep":0,"rec":"NA","env":"Réduction de la consommation d'énergie et de l'émission de gaz a effet de serre","soc":"NA","fo":1,"fe":1,"fh":1,"ft":0,"fi":1,"fr":1,"lo":3,"le":1,"lh":3,"lt":3,"li":0,"lr":1,"ln":1,"src":"ppe.fd7.mytemp.website/ecoazur.com/fr/ "},{"n":"JARDINS ÉCOUMÈNE","r":"Quebec","co":"Québec","ac":"OUI","cl":"particulier/organisation","sz":"Petite","sv":["Conception & Fabrication","Transformation de Matière","Distribution & Négoce","Prestation Intellectuelle","Service Numérique"],"ty":["Conseil & Formation","Mutualisation / Partage"],"oi":"Vente de semence biologiques","oe":"Vente, conseillez et accompagnement de la mise en tere de semences biologiques","vb":1,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Contrat de vente","env":"Produits locaux et biologique","soc":"Formations et conseils fournis","fo":0,"fe":1,"fh":1,"ft":0,"fi":0,"fr":0,"lo":3,"le":2,"lh":4,"lt":4,"li":2,"lr":1,"ln":1,"src":"https://ecoumene.com/ https://www.registreentreprises.gouv.qc.ca/REQNA/GR/GR03/G"},{"n":"GROUPE SIMONEAU","r":"Quebec","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Grande","sv":["Conception & Fabrication","Maintenance & Réparation","Prestation Intellectuelle"],"ty":["SAV & Maintenance","Conseil & Formation","Location / Leasing"],"oi":"Vente de chaudière","oe":"Vente, consultation, maintenance et location de chaudière","vb":1,"vu":1,"vp":0,"ef":1,"efc":0,"mat":"En place","rep":1,"rec":"Contrats de vente et de location","env":"Réduction de la consommation d'énergie","soc":"Accompagnement des entreprises dans leur projets","fo":1,"fe":1,"fh":0,"ft":0,"fi":0,"fr":1,"lo":2,"le":1,"lh":2,"lt":5,"li":1,"lr":1,"ln":0,"src":"https://www.groupesimoneau.com/fr/ https://www.registreentreprises.gouv.qc.ca/RE"},{"n":"MAIRIE BAGNÈRES-DE-LUCHON - De la terre à l'assiette et au-delà","r":"France","co":"France","ac":"OUI","cl":"Particulier","sz":"Petite","sv":["Service à la Personne"],"ty":["Conseil & Formation","Mutualisation / Partage","Externalisation"],"oi":"services publics","oe":"Services collaboratifs territoriaux","vb":0,"vu":1,"vp":0,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"-","env":"faible","soc":"positif","fo":2,"fe":3,"fh":0,"ft":0,"fi":0,"fr":0,"lo":5,"le":1,"lh":3,"lt":4,"li":2,"lr":0,"ln":1,"src":"ADEME. (2020). Économie de la fonctionnalité et de la coopération. https://www.a"},{"n":"Aquatech‑BM","r":"Quebec","co":"Québec","ac":"OUI","cl":"Organisation privé","sz":"Moyenne","sv":["Conception & Fabrication","Maintenance & Réparation","Prestation Intellectuelle","Gestion de Déchets"],"ty":["SAV & Maintenance","Location / Leasing","Vente de Performance"],"oi":"Vente équipement","oe":"Solution orientée usage durable","vb":0,"vu":0,"vp":1,"ef":1,"efc":0,"mat":"En développement","rep":1,"rec":"Contrats récurrents services","env":"Réduction ressources consommées","soc":"Coopération locale renforcée","fo":6,"fe":8,"fh":7,"ft":3,"fi":2,"fr":1,"lo":6,"le":3,"lh":2,"lt":3,"li":2,"lr":2,"ln":0,"src":"•\tAquatech BM. (s. d.). quebeccirculaire.org. Consulté 16 février 2026, à l’adre"},{"n":"Strateira","r":"France (bourgogne Franche comté)","co":"France","ac":"OUI","cl":"particulier/organisation","sz":"Micro","sv":["Prestation Intellectuelle"],"ty":["Conseil & Formation","Externalisation"],"oi":"vente de service","oe":"accompagnement de l'entreprise, prise en compte des enjeux et objectifs propres à l'entreprise ","vb":0,"vu":0,"vp":1,"ef":0,"efc":1,"mat":"Manque d'information","rep":0,"rec":"Contrats uniques","env":"sensibilisation de leurs clients aux impact environnementaux et développement durable ","soc":"Elevé, accompagnement de l'entreprise pour le bien être de ces employés  ","fo":1,"fe":2,"fh":3,"ft":0,"fi":0,"fr":0,"lo":3,"le":1,"lh":4,"lt":2,"li":2,"lr":0,"ln":0,"src":"https://strateira.com/"},{"n":"LA TAT (LA TÊTE À TOTO) - Mieux habiter son logement, son quartier, sa ville","r":"France (grand est)","co":"France","ac":"OUI","cl":"Organisation public","sz":"Petite","sv":["Prestation Intellectuelle"],"ty":[],"oi":"vente ou location d'un bien immobilié","oe":"pas encore définie","vb":0,"vu":0,"vp":0,"ef":0,"efc":1,"mat":"Non Abouti","rep":0,"rec":"n/a","env":"sensibilisation aux impacts envionnementaux, optique de développement d'une solution durable et respectueuse de l'environnement ","soc":"Elevé, groupe de discution, atelier et éveènements mis en place pour rapprocher les acteurs de l'habitat","fo":5,"fe":5,"fh":6,"ft":2,"fi":1,"fr":0,"lo":5,"le":2,"lh":3,"lt":8,"li":2,"lr":2,"ln":1,"src":""},{"n":"Groupe Alphard","r":"Quebec","co":"Québec","ac":"OUI","cl":"particulier/organisation","sz":"Moyenne","sv":["Prestation Intellectuelle","Gestion de Déchets"],"ty":["Conseil & Formation","Vente de Performance"],"oi":"Vente de services","oe":"Solution orientée performance d’usage","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En développement","rep":0,"rec":"Récurrence contrats long terme","env":"Réduction externalités environnementales","soc":"Création valeur responsable","fo":4,"fe":6,"fh":5,"ft":3,"fi":2,"fr":1,"lo":4,"le":4,"lh":5,"lt":11,"li":2,"lr":3,"ln":1,"src":"•\t(20) Groupe Alphard | Engineering : Overview | LinkedIn. (s. d.). Consulté 14 "},{"n":"Signify","r":"France","co":"France","ac":"OUI","cl":"Organisation public","sz":"Grande","sv":["Conception & Fabrication","Maintenance & Réparation","Prestation Intellectuelle","Service Numérique","Gestion de Déchets"],"ty":["Conseil & Formation","Externalisation","Vente de Performance"],"oi":"Vente de services","oe":"Solution orientée performance d’usage","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En place","rep":0,"rec":"Contrats performance récurrents","env":"Réduction ressources consommées","soc":"Coopération locale renforcée","fo":3,"fe":9,"fh":5,"ft":3,"fi":0,"fr":1,"lo":5,"le":5,"lh":4,"lt":8,"li":0,"lr":3,"ln":0,"src":"•\tHome. (s. d.). Signify. Consulté 16 février 2026, à l’adresse https://www.sign"},{"n":"Compost","r":"Île-de-France","co":"France","ac":"oui ","cl":"Organisation public","sz":"Petite","sv":["Service Numérique","Service à la Personne"],"ty":["Conseil & Formation","Externalisation","Vente de Performance"],"oi":"Collecte et traitement des biodéchets (production de compost)","oe":"Service complet de gestion des déchets","vb":0,"vu":1,"vp":1,"ef":0,"efc":1,"mat":"En place","rep":0,"rec":"Récurrents","env":"Positif","soc":"amélioration de l’environnement et du cadre de vie","fo":4,"fe":4,"fh":6,"ft":3,"fi":2,"fr":1,"lo":5,"le":4,"lh":4,"lt":6,"li":2,"lr":2,"ln":1,"src":"Terres EFC Île-de-France – Plateforme dédiée à l’économie de la fonctionnalité e"},{"n":"SUNCHANEZ","r":"France","co":"France","ac":"","cl":"","sz":"Petite","sv":["Maintenance & Réparation","Gestion de Déchets"],"ty":["SAV & Maintenance","Conseil & Formation","Vente de Performance"],"oi":"Vente et installation d'équipements techniques, suivie de leur maintenance.","oe":"L'offre \"Rose des vents\" : mise à disposition de groupes de froid/climatisation pendant la période d'utilisation réelle (saisonnière)","vb":0,"vu":1,"vp":1,"ef":1,"efc":0,"mat":"En développement","rep":1,"rec":"Revenus basés sur la location saisonnière récurrente et les services associés plutôt que sur une vente unique","env":"Optimisation du taux d'utilisation des équipements (mutualisation) et baisse de l'intensité matière et énergie du poste \"froid\".","soc":"Augmentation de l'autonomie des techniciens en interne et changement de la relation client vers un rapport de cohérence et de convergence.","fo":3,"fe":6,"fh":6,"ft":3,"fi":2,"fr":1,"lo":2,"le":4,"lh":2,"lt":4,"li":1,"lr":2,"ln":1,"src":"ADEME. (2020). Expériences d’entreprises en économie de la fonctionnalité. Agenc"}]
;
const CAT = {
  eco: { label: "Économique & financier", color: "#185FA5", bg: "#E6F1FB" },
  org: { label: "Organisationnel & gouvernance", color: "#533AB7", bg: "#EEEDFE" },
  info: { label: "Informationnel & culturel", color: "#D4537E", bg: "#FBEAF0" },
  mar: { label: "Marché & clients", color: "#BA7517", bg: "#FAEEDA" },
  jur: { label: "Juridique & contractuel", color: "#A32D2D", bg: "#FCEBEB" },
  tec: { label: "Technique & opérationnel", color: "#0F6E56", bg: "#E1F5EE" },
  str: { label: "Stratégique & concurrentiel", color: "#5F5E5A", bg: "#F1EFE8" },
};
const TOOLS = [
  {
    id:"lcc", short:"LCC / ACA", full:"LCC – Life Cycle Costing / ACA élargie",
    desc:"Comptabilise l'ensemble des coûts sur le cycle de vie ; monétarise les externalités pour comparer des scénarios.",
    maturite: 4, facilite: 2,
    link: "https://oneclicklca.com/fr/software/design-construction/life-cycle-costing",
    phases: ["diag","conc","mise","ctrl"]
  },
  {
    id:"afm", short:"AFM", full:"Analyse des flux de matière (AFM)",
    desc:"Quantifie les flux et stocks de matières au sein du système pour optimiser l'utilisation des ressources et réduire les déchets.",
    maturite: 5, facilite: 2,
    link: "https://dei.so/fr/what-are-material-flow-analysis-mfa-and-substance-flow-analysis-sfa/",
    phases: ["diag","conc","mise","ctrl"]
  },
  {
    id:"acv", short:"ACV", full:"Analyse de Cycle de Vie (ACV)",
    desc:"Évalue les impacts environnementaux de l'extraction à la fin de vie ; outil d'aide à l'éco-conception.",
    maturite: 5, facilite: 2,
    link: "https://ciraig.org/index.php/fr/analyse-du-cycle-de-vie/",
    phases: ["diag","conc","mise","ctrl"]
  },
  {
    id:"app", short:"Parties prenantes", full:"Analyse des parties prenantes",
    desc:"Comprend les intérêts et besoins des acteurs ; utile quand les parties ont des intérêts contradictoires.",
    maturite: 5, facilite: 3,
    link: "https://sciencespo.hal.science/hal-04842878v1",
    phases: ["diag","conc","ctrl"]
  },
  {
    id:"amcd", short:"AMCD/MCDA", full:"Analyse Multicritère d'Aide à la Décision",
    desc:"Facilite la prise de décision dans les situations complexes avec plusieurs critères souvent contradictoires.",
    maturite: 5, facilite: 1,
    link: "",
    phases: ["conc","mise","ctrl"]
  },
  {
    id:"conf", short:"Conférence/Atelier", full:"Conférence / Atelier EFC",
    desc:"Informer et permettre la mise en place de l'EFC ; sensibiliser les entreprises et acteurs aux bénéfices.",
    maturite: 5, facilite: 5,
    link: "",
    phases: ["diag","conc"]
  },
  {
    id:"cart", short:"Cartographie acteurs", full:"Cartographie des acteurs",
    desc:"Identifie et hiérarchise les parties prenantes ; évalue leur influence et positionnement pour gérer les résistances.",
    maturite: 4, facilite: 4,
    link: "https://urbanismeparticipatif.ca/outils/cartographie-des-acteurs",
    phases: ["diag"]
  },
  {
    id:"raci", short:"Matrice RACI", full:"Matrice RACI (échelle méso/macro)",
    desc:"Définit clairement les rôles et responsabilités pour chaque tâche ; fluidifie la communication sur les projets complexes.",
    maturite: 4, facilite: 3,
    link: "https://shs.cairn.info/la-boite-a-outils-de-la-conduite-du-changement--9782100776344-page-130?lang=fr",
    phases: ["diag","conc","mise"]
  },
  {
    id:"mat", short:"Grille maturité", full:"Grille de maturité",
    desc:"Auto-évaluation pour mesurer le niveau de sophistication des processus ; diagnostique l'état actuel et définit une feuille de route.",
    maturite: 4, facilite: 4,
    link: "https://cdn-contenu.quebec.ca/cdn-contenu/adm/min/secretariat-du-conseil-du-tresor/sppalap/infolettre/2025/04/echelle_maturite_processus.pdf",
    phases: ["diag"]
  },
  {
    id:"bil", short:"Bilan économique", full:"Bilan économique (ADEME)",
    desc:"Calcule l'amortissement des investissements d'une stratégie de valorisation ; évalue la faisabilité financière.",
    maturite: 3, facilite: 3,
    link: "https://portailcoop.hec.ca/notice?id=h%3A%3A6863d886-1481-49a9-a057-f804c64a3f1d&queryId=N-c87d5224-fa3f-4415-8c57-67dd9a4d75b4&posInSet=1",
    phases: ["diag"]
  },
  {
    id:"gefc", short:"Grille EFC", full:"Grille d'analyse comparative EFC (coût/bénéfice)",
    desc:"Analyse le potentiel de l'EFC pour un projet : comparatif des coûts, analyse coûts-bénéfices et multicritères.",
    maturite: 3, facilite: 3,
    link: "https://constructioncirculaire.com/ressource/grille-danalyse-de-leconomie-de-la-fonctionnalite/",
    phases: ["diag","conc"]
  },
  {
    id:"ges", short:"Calculateur GES", full:"Calculateur de GES",
    desc:"Calcule les gaz à effet de serre liés au transport et aux matières résiduelles.",
    maturite: 3, facilite: 4,
    link: "https://portailcoop.hec.ca/notice?id=h%3A%3A54bf8e1e-14bc-4bdc-a7ee-2ad0ba89061f&queryId=N-c87d5224-fa3f-4415-8c57-67dd9a4d75b4&posInSet=2",
    phases: ["diag","ctrl"]
  },
  {
    id:"mat2", short:"Calc. matières", full:"Calculateur Matières résiduelles",
    desc:"Calcule le poids des matières résiduelles (recyclage, compost, déchets) ; améliore la gestion des déchets.",
    maturite: 3, facilite: 3,
    link: "https://portailcoop.hec.ca/notice?id=h::c0abacbf-67a4-4545-b0ab-f621defc56a2",
    phases: ["diag","ctrl"]
  },
  {
    id:"val", short:"Approche éco. déchets", full:"Approche économique – valorisation des déchets",
    desc:"Calcule les retombées financières de la valorisation des déchets ; visualise les gains potentiels.",
    maturite: 3, facilite: 3,
    link: "https://portailcoop.hec.ca/notice?id=h%3A%3A73d58fa5-bf78-4ada-8633-f807ad48c56a&queryId=N-EXPLORE-ed9e826a-2c37-4419-bf21-86f90d6f4e83&posInSet=3",
    phases: ["diag"]
  },
  {
    id:"four", short:"Éval. fournisseurs", full:"Outils d'évaluation des fournisseurs",
    desc:"Compile les informations sur les fournisseurs et évalue leurs impacts sur l'écoresponsabilité.",
    maturite: 3, facilite: 3,
    link: "https://portailcoop.hec.ca/notice?id=h%3A%3A4b7fc87a-e736-441e-b57a-6c127cae4ad8&queryId=N-EXPLORE-ed9e826a-2c37-4419-bf21-86f90d6f4e83&posInSet=4",
    phases: ["diag","ctrl"]
  },
  {
    id:"can", short:"Canevas durable", full:"Canevas du modèle d'affaires durable (EFC Québec)",
    desc:"Identifie les nouvelles considérations pour un modèle d'affaires durable plutôt que traditionnel.",
    maturite: 2, facilite: 4,
    link: "https://www.efcquebec.com/wp-content/uploads/canevas-du-modele-daffaires-durable-efc-quebec.pdf",
    phases: ["diag"]
  },
  {
    id:"cef", short:"Cartographie effets", full:"Cartographie des effets (EFC Québec)",
    desc:"Représentation graphique des effets des activités sur l'écosystème d'acteurs.",
    maturite: 2, facilite: 5,
    link: "https://www.efcquebec.com/wp-content/uploads/cartographie-des-effets-efc-quebec.pdf",
    phases: ["diag"]
  },
  {
    id:"jdb", short:"Journal de bord", full:"Journal de bord (accompagnateur + entreprise)",
    desc:"Documente l'évolution de la démarche EFC ; sert d'effet miroir pour le dirigeant.",
    maturite: 1, facilite: 5,
    link: "https://www.efcquebec.com/wp-content/uploads/journal-de-bord-accompagnateur-efc-quebec.pdf",
    phases: ["diag","conc","mise","ctrl"]
  },
  {
    id:"fres", short:"Fresque transition", full:"Fresque de la transition économique et écologique",
    desc:"Outil ludo-pédagogique pour acculturer à l'EFC via pédagogie inversée ; exemples concrets de transition.",
    maturite: 4, facilite: 3,
    link: "https://www.ieefc.eu/la-fresque-de-la-transition-economique/",
    phases: ["diag"]
  },
  {
    id:"eco", short:"Écolabel européen", full:"Écolabel européen",
    desc:"Label officiel de la plus-value environnementale ; aide les acteurs à s'améliorer et favorise l'économie circulaire.",
    maturite: 5, facilite: 1,
    link: "",
    phases: ["diag","conc","mise","ctrl"]
  },
  {
    id:"kit", short:"Kit Entreprise", full:"Le Kit Entreprise (ADEME)",
    desc:"Kit pour démarrer la transformation vers un modèle d'affaire différent (économie circulaire, EFC).",
    maturite: 2, facilite: 5,
    link: "https://epargnonsnosressources.gouv.fr/entreprises/boite-a-outils/le-kit-entreprise/",
    phases: ["diag"]
  },
  {
    id:"club", short:"Clubs EFC", full:"Les clubs EFC (IEEFC)",
    desc:"Accompagnement sur 12-18 mois dans le changement de modèle d'affaire vers l'EFC ; communauté de pairs.",
    maturite: 3, facilite: 5,
    link: "https://www.ieefc.eu/clubs-efc/",
    phases: ["diag","conc","mise","ctrl"]
  },
  {
    id:"flux", short:"État des lieux flux", full:"État des lieux des flux entrants/sortants (Diag Éco-Flux)",
    desc:"Identifie les sources d'économies sur énergie, matières, eau, déchets ; plan d'action chiffré personnalisé.",
    maturite: 3, facilite: 2,
    link: "https://diag.bpifrance.fr/diag-eco-flux",
    phases: ["diag"]
  },
  {
    id:"lise", short:"LISE (carbone)", full:"Évaluation carbone simplifiée (LISE – CCI)",
    desc:"Mesure rapidement l'empreinte carbone des PME/TPE ; identifie des leviers et élabore un plan de décarbonation.",
    maturite: 4, facilite: 2,
    link: "https://www.cci.fr/offre/evaluation-carbone-simplifiee-lise-mesurez-et-reduisez-vos-emissions-de-co2",
    phases: ["diag","conc"]
  },
  {
    id:"qges", short:"QuantiGES", full:"Méthode QuantiGES (ADEME)",
    desc:"Quantifie l'impact GES d'une action de réduction ; démarche pratique par étapes.",
    maturite: 3, facilite: null,
    link: "https://librairie.ademe.fr/changement-climatique/4827-methode-quantiges-9791029718236.html",
    phases: ["diag","conc"]
  },
  {
    id:"act", short:"ACT Pas à Pas", full:"Méthode ACT Pas à Pas (ADEME)",
    desc:"Méthode structurante pour identifier un modèle économique compatible avec une trajectoire bas carbone.",
    maturite: 4, facilite: 2,
    link: "https://mission-transition-ecologique.beta.gouv.fr/aides-entreprise/act-pas-a-pas",
    phases: ["diag","conc"]
  },
  {
    id:"for", short:"Outils formation", full:"Outils pour formation/atelier",
    desc:"Instruments permettant aux coopératives de faire progresser leurs stratégies circulaires.",
    maturite: 1, facilite: 5,
    link: "https://portailcoop.hec.ca/search/bfcf44c7-0724-4efd-8ad8-34221a1196c7/N-09820dc9-cbd2-47ce-9d28-9afd7b920b00",
    phases: ["diag"]
  },
  {
    id:"mar2", short:"Matrice MAR", full:"Matrice du modèle d'affaire responsable (Laval)",
    desc:"Réflexion approfondie sur les défis du développement durable ; intègre ces défis dans les modèles d'affaires.",
    maturite: 2, facilite: 5,
    link: "https://portailcoop.hec.ca/in/documentViewer.xhtml?v=2.1.196&id=de102822-f9f9-4789-a78b-f253e2b8f4ce&highlight=profile%3Aaa64feff-51e9-47a7-af49-c47b7dfdb946&file=/in/rest/annotationSVC/DownloadWatermarkedAttachment/attach_upload_4b53711f-b594-497d-8036-d6fd9491441c%3F_%3Dmodele-daffaires-responsable-instructions.pdf&locale=fr&multi=false",
    phases: ["diag","ctrl"]
  },
  {
    id:"bio", short:"Biomimicards", full:"Biomimicards (Circulab)",
    desc:"Favorise la collaboration et l'innovation ; inspire de nouvelles façons de concevoir produits/services depuis la nature.",
    maturite: 2, facilite: 5,
    link: "https://circulab.academy/outils-economie-circulaire/biomimicards-biomimetisme/",
    phases: ["diag"]
  },
  {
    id:"dt", short:"Design thinking", full:"Design thinking (IDEO)",
    desc:"Repenser les offres centrées usage ; approche itérative d'innovation centrée sur l'utilisateur.",
    maturite: 5, facilite: 1,
    link: "",
    phases: ["diag","conc"]
  },
];
const FREINS_MATRIX = [
["eco","Bouleversement du modèle de revenus",        2,0,0,1,1,0,1,0,1,1,2,0,0,0,0,2,1,1,0,0,1,1,0,0,0,1,0,2,0,1],
["eco","Investissements initiaux élevés",             2,0,0,0,1,0,0,0,0,2,1,0,0,0,0,1,1,1,0,0,1,1,1,0,0,1,0,1,0,0],
["eco","Difficulté d'accès au financement",           2,0,0,1,1,0,0,0,0,2,1,0,0,0,0,1,1,1,0,0,1,1,1,0,0,1,0,1,0,0],
["eco","Incertitude sur la rentabilité",              2,0,0,0,2,0,0,0,1,2,2,0,0,1,0,1,1,1,0,0,1,1,0,0,0,1,0,1,0,0],
["eco","Coûts de maintenance et de gestion",         2,1,0,0,1,0,0,0,0,1,2,0,0,0,0,1,1,1,0,0,0,0,1,0,0,0,0,1,0,0],
["eco","Immobilisation financière importante",        2,0,0,0,1,0,0,0,0,2,2,0,0,0,0,1,1,1,0,0,0,0,0,0,0,0,0,1,0,0],
["eco","Forte intensité capitalistique",              2,0,0,0,1,0,0,0,0,2,2,0,0,0,0,1,1,1,0,0,0,0,0,0,0,0,0,1,0,0],
["eco","Arbitrage budgétaire défavorable",            2,0,0,1,2,0,0,0,1,2,1,0,0,0,0,1,1,1,0,0,1,0,0,0,0,1,0,1,0,0],
["eco","Dépendance aux financements publics",         2,0,0,1,1,0,0,0,0,2,1,0,0,0,0,1,1,1,0,0,1,1,1,0,0,1,0,1,0,0],
["eco","Faible valorisation économique des déchets",  1,2,1,0,1,0,0,0,0,1,1,1,1,2,1,1,1,0,0,0,0,0,2,0,1,0,0,0,0,0],
["org","Résistance au changement interne",            0,0,0,2,0,2,1,1,2,0,0,0,0,0,0,1,1,2,2,0,1,2,0,0,0,0,1,1,1,1],
["org","Silos organisationnels",                      0,0,0,1,0,2,1,2,1,0,0,0,0,0,0,1,1,1,1,0,1,2,0,0,0,0,1,1,0,0],
["org","Transformation organisationnelle",            0,0,0,2,1,2,1,2,2,0,1,0,0,0,0,2,1,2,1,0,1,2,0,0,0,1,1,2,0,1],
["org","Complexité organisationnelle",                0,0,0,1,1,1,1,2,2,0,0,0,0,0,0,1,1,1,0,0,1,1,0,0,0,0,0,1,0,0],
["org","Divergence d'objectifs",                      0,0,0,2,2,1,2,1,1,0,1,0,0,0,0,1,2,1,0,0,1,1,0,0,0,0,0,1,0,0],
["org","Manque de temps dédié",                       0,0,0,0,0,1,0,1,1,0,0,0,0,0,0,0,0,2,0,0,0,1,0,0,0,0,1,0,0,0],
["org","Complexité de coordination entre acteurs",    0,0,0,2,1,2,2,2,1,0,0,0,0,0,0,1,2,1,0,0,1,2,0,0,0,0,1,1,0,0],
["org","Manque de communication",                     0,0,0,1,0,2,1,1,0,0,0,0,0,0,0,0,1,1,2,0,0,1,0,0,0,0,1,0,0,0],
["org","Conflits d'intérêts",                         0,0,0,2,1,1,2,1,0,0,0,0,0,0,0,0,2,1,0,0,0,1,0,0,0,0,0,0,0,0],
["info","Culture de la propriété",                    0,0,0,0,0,2,1,0,1,0,2,0,0,0,0,1,1,1,2,0,1,2,0,0,0,0,1,1,0,1],
["info","Habitudes d'achat ancrées",                  0,0,0,0,0,2,1,0,1,0,2,0,0,0,0,1,1,1,2,0,1,2,0,0,0,0,1,1,0,2],
["info","Manque de sensibilisation",                  0,0,0,1,0,2,1,0,1,0,1,0,0,0,0,1,1,1,2,0,1,2,0,0,0,0,2,1,1,0],
["info","Faible culture de la coopération",           0,0,0,2,0,2,2,0,1,0,1,0,0,0,0,1,1,1,2,0,1,2,0,0,0,0,1,1,1,0],
["info","Peur du risque / culture de l'échec",        0,0,0,1,0,2,1,0,2,0,1,0,0,0,0,1,1,1,2,0,1,2,0,0,0,0,1,1,0,0],
["info","Logique court terme",                        2,0,1,1,1,1,1,0,1,1,2,1,0,0,0,1,2,1,2,0,1,1,0,1,1,2,0,1,0,0],
["mar","Difficulté à mesurer le ROI",                 2,0,0,0,1,0,0,0,1,2,2,0,0,0,0,1,2,1,0,0,1,0,0,0,0,0,0,1,0,0],
["mar","Méfiance des clients",                        0,0,0,1,0,1,1,0,1,0,1,0,0,0,0,1,2,1,1,1,1,2,0,0,0,0,1,1,0,0],
["mar","Complexité de la tarification",               2,0,0,0,1,0,0,0,0,2,2,0,0,1,0,2,1,1,0,0,0,0,0,0,0,0,0,1,0,0],
["mar","Risque de dépendance fournisseur",             1,0,0,2,1,0,1,1,0,0,1,0,0,0,2,1,2,0,0,0,0,1,0,0,0,0,0,1,0,0],
["mar","Difficulté à percevoir la valeur globale",    2,1,1,1,1,1,1,0,1,1,2,1,0,0,0,2,2,1,1,1,1,1,0,1,1,1,0,2,0,1],
["mar","Faible visibilité auprès des clients",        0,0,0,1,0,1,1,0,0,1,1,0,0,0,0,1,2,1,0,1,1,1,0,0,0,0,0,1,0,0],
["mar","Résistance au changement des clients",        0,0,0,1,0,2,1,0,1,0,1,0,0,0,0,1,2,1,2,1,1,2,0,0,0,0,1,1,0,2],
["jur","Cadre juridique flou",                        0,0,0,1,0,1,0,0,0,0,1,0,0,0,0,1,1,1,0,0,0,1,0,0,0,0,0,0,0,0],
["jur","Responsabilités élargies",                    0,0,0,1,0,1,0,2,0,0,2,0,0,0,0,1,1,1,0,0,0,1,0,0,0,0,0,1,0,0],
["jur","Propriété des données",                       0,0,0,0,0,0,0,1,0,0,1,0,0,0,0,0,1,1,0,0,0,0,0,0,0,0,0,0,0,0],
["jur","Complexité réglementaire",                    0,0,0,1,0,1,0,1,0,0,1,0,0,0,0,0,1,1,0,0,0,1,0,0,0,0,0,0,0,0],
["jur","Contraintes réglementaires sectorielles",     0,0,0,1,0,1,0,1,0,0,1,0,0,0,0,0,1,1,0,1,0,1,0,0,0,0,0,0,0,0],
["jur","Traçabilité des usages",                      0,1,0,0,0,0,0,1,0,0,1,1,1,0,1,0,1,2,0,0,0,0,0,0,0,0,0,0,0,0],
["tec","Logistique inverse",                          1,2,0,0,0,0,0,0,0,1,1,1,1,1,1,1,1,1,0,0,0,0,2,0,0,0,0,0,0,0],
["tec","Éco-conception",                              1,1,2,0,0,0,0,0,0,0,1,1,0,0,1,1,0,0,0,1,0,0,1,1,1,0,0,0,1,1],
["tec","Besoin de maintenance",                       2,0,0,0,0,0,0,0,0,1,2,0,0,0,0,1,1,1,0,0,0,0,1,0,0,0,0,0,0,0],
["tec","Technologie immature",                        0,0,0,0,1,1,0,0,1,1,1,0,0,0,0,1,1,1,0,0,0,1,0,0,0,0,0,1,0,1],
["tec","Manque de filières locales structurées",      0,1,0,2,1,1,2,0,0,1,1,0,0,1,0,1,2,1,0,0,0,1,1,0,0,0,0,1,0,0],
["tec","Capacité limitée avant modernisation",        1,1,0,0,1,0,0,0,1,1,1,0,0,0,0,1,1,1,0,0,0,1,1,0,0,0,0,1,0,0],
["tec","Dépendance aux matières premières importées", 1,2,1,1,1,0,0,0,0,1,1,1,1,1,1,1,1,0,0,0,0,0,2,0,1,0,0,0,0,0],
["str","Cannibalisation des ventes",                  2,0,0,0,2,0,0,0,1,1,2,0,0,0,0,2,2,1,0,0,0,0,0,0,0,1,0,2,0,1],
["str","Pression concurrentielle",                    1,0,1,0,2,0,0,0,1,1,2,1,0,0,1,2,2,1,0,1,0,0,0,1,1,1,0,2,0,1],
["str","Temps de transition long",                    1,0,0,1,1,1,1,1,2,1,1,0,0,0,0,1,1,2,0,0,1,2,0,0,0,1,1,1,0,0],
["str","Dépendance aux partenaires",                  0,0,0,2,1,1,2,1,0,0,1,0,0,0,1,1,2,1,0,0,1,2,0,0,0,0,0,1,0,0],
["str","Dépendance à l'écosystème territorial",       0,1,0,2,1,1,2,1,0,1,1,0,0,0,0,1,2,1,0,0,0,2,1,0,0,0,0,1,0,0],
];


const FC=["#A32D2D","#D4537E","#BA7517","#0F6E56","#533AB7","#5F5E5A"];
const FL=["Org.","Éco.","Hum.","Tech.","Infra.","Régl."];
const FK=["fo","fe","fh","ft","fi","fr"];
const LC=["#0F6E56","#185FA5","#533AB7","#D4537E","#BA7517","#A32D2D","#5F5E5A"];
const LL=["Org.","Éco.","Hum.","Tech.","Infra.","Régl.","Info."];
const LK=["lo","le","lh","lt","li","lr","ln"];

let fGeo="all",fMat="all",sTerm="",vMode="cards",sCol=null,sDir=1,mCat="all",chartsRendered=false;

function mc(m){if(!m)return"mat-manque";if(m.includes("En place"))return"mat-enplace";if(m.includes("dév")||m.includes("développement"))return"mat-dev";if(m.includes("Non"))return"mat-nonabouti";return"mat-manque"}
function mxF(){return Math.max(...COMPANIES.map(c=>Math.max(...FK.map(k=>c[k]||0))),1)}
function mxL(){return Math.max(...COMPANIES.map(c=>Math.max(...LK.map(k=>c[k]||0))),1)}
function flt(){return COMPANIES.filter(c=>{if(fGeo!=="all"&&c.co.toLowerCase()!==fGeo)return false;if(fMat!=="all"&&c.mat!==fMat)return false;if(sTerm&&!c.n.toLowerCase().includes(sTerm))return false;return true})}
function bars(keys,colors,labels,c,mx){return keys.map((k,i)=>{const v=c[k]||0;return`<div class="bar-row"><span class="bar-label">${labels[i]}</span><div class="bar-track"><div class="bar-fill" style="width:${mx?v/mx*100:0}%;background:${colors[i]}"></div></div><span class="bar-val">${v}</span></div>`}).join("")}

function renderStats(){const f=flt();document.getElementById("stats").innerHTML=[{n:f.length,l:"Entreprises"},{n:f.filter(c=>c.efc).length,l:"EFC"},{n:f.filter(c=>c.ef&&!c.efc).length,l:"EF"},{n:f.filter(c=>c.mat==="En place").length,l:"En place"},{n:f.filter(c=>c.mat==="En développement").length,l:"En dév."},{n:f.filter(c=>c.co==="France").length,l:"France"},{n:f.filter(c=>c.co==="Québec").length,l:"Québec"}].map(s=>`<div class="stat-card"><div class="num">${s.n}</div><div class="label">${s.l}</div></div>`).join("")}

function renderCards(){const f=flt(),mf=mxF(),ml=mxL();document.getElementById("cards-view").innerHTML=f.map(c=>{const b=[];if(c.efc)b.push('<span class="badge badge-efc">EFC</span>');if(c.ef)b.push('<span class="badge badge-ef">EF</span>');if(c.mat)b.push(`<span class="badge ${mc(c.mat)}">${c.mat}</span>`);const tf=FK.reduce((s,k)=>s+(c[k]||0),0),tl=LK.reduce((s,k)=>s+(c[k]||0),0);const ty=c.ty.slice(0,3).map(t=>`<span class="tag">${t}</span>`).join("");return`<div class="card" onclick="showModal(${COMPANIES.indexOf(c)})"><div class="card-head"><span class="card-name">${c.n}</span><div style="display:flex;gap:3px;flex-wrap:wrap">${b.join("")}</div></div><div class="card-region">${c.r} · ${c.sz}</div><div class="card-row">${ty}</div><div class="card-section"><div style="display:flex;gap:14px"><div style="flex:1"><div class="card-section-title">Freins (${tf})</div>${bars(FK,FC,FL,c,mf)}</div><div style="flex:1"><div class="card-section-title">Leviers (${tl})</div>${bars(LK,LC,LL,c,ml)}</div></div></div></div>`}).join("");document.getElementById("count").textContent=f.length}

function renderTable(){const f=flt();if(sCol!==null)f.sort((a,b)=>{let va=a[sCol],vb=b[sCol];if(typeof va==="string")return va.localeCompare(vb)*sDir;return((va||0)-(vb||0))*sDir});const cols=[{k:"n",l:"Nom"},{k:"co",l:"Pays"},{k:"sz",l:"Taille"},{k:"mat",l:"Maturité"},{k:"fo",l:"F.Org"},{k:"fe",l:"F.Éco"},{k:"fh",l:"F.Hum"},{k:"ft",l:"F.Tech"},{k:"lo",l:"L.Org"},{k:"le",l:"L.Éco"},{k:"lh",l:"L.Hum"},{k:"lt",l:"L.Tech"}];let h=`<table><thead><tr>${cols.map(c=>`<th data-sort="${c.k}">${c.l}</th>`).join("")}</tr></thead><tbody>`;f.forEach(c=>{h+=`<tr onclick="showModal(${COMPANIES.indexOf(c)})">`;cols.forEach(col=>{let v=c[col.k]||"";if(col.k==="mat")v=`<span class="badge ${mc(v)}">${v||"-"}</span>`;h+=`<td>${v}</td>`});h+=`</tr>`});h+=`</tbody></table>`;document.getElementById("table-view").innerHTML=h;document.getElementById("count").textContent=f.length;document.querySelectorAll("#table-view th[data-sort]").forEach(th=>{th.onclick=()=>{const k=th.dataset.sort;if(sCol===k)sDir*=-1;else{sCol=k;sDir=1}renderTable()}})}

function getRecoOutils(c){
  const catScores={eco:c.fe||0,org:c.fo||0,info:c.fh||0,tec:(c.ft||0)+(c.fi||0),jur:c.fr||0,mar:0,str:0};
  const activeCats=Object.entries(catScores).filter(([k,v])=>v>0).map(([k])=>k);
  if(!activeCats.length)return[];
  const toolScores=TOOLS.map((t,ti)=>{let score=0;FREINS_MATRIX.forEach(row=>{if(activeCats.includes(row[0])){const val=row[ti+2]||0;if(val===2)score+=3*catScores[row[0]];else if(val===1)score+=1*catScores[row[0]]}});return{tool:t,score,idx:ti}});
  return toolScores.filter(t=>t.score>0).sort((a,b)=>b.score-a.score).slice(0,8);
}

function showModal(idx){
  const c=COMPANIES[idx];const mf=mxF(),ml=mxL();
  const b=[];if(c.efc)b.push('<span class="badge badge-efc">EFC</span>');if(c.ef)b.push('<span class="badge badge-ef">EF</span>');if(c.mat)b.push(`<span class="badge ${mc(c.mat)}">${c.mat}</span>`);
  const vente=[];if(c.vb)vente.push("Bien");if(c.vu)vente.push("Usage");if(c.vp)vente.push("Performance");
  const tf=FK.reduce((s,k)=>s+(c[k]||0),0),tl=LK.reduce((s,k)=>s+(c[k]||0),0);
  const reco=getRecoOutils(c);
  const recoHtml=reco.length?reco.map(r=>{const t=r.tool;const cls=r.score>20?"primary":"";return`<span class="outil-chip ${cls}">${t.link?`<a href="${t.link}" target="_blank">${t.short}</a>`:t.short}</span>`}).join(""):"<span style='font-size:11px;color:var(--muted)'>Aucun frein identifié</span>";
  document.getElementById("modal").innerHTML=`<button class="modal-close" onclick="closeModal()">✕</button><h2>${c.n}</h2><div class="meta">${c.r} · ${c.sz} · ${c.cl} · ${b.join(" ")}</div>
  <div class="offre-row"><div class="offre-box offre-init"><div class="offre-label">Offre initiale</div>${c.oi||"N/A"}</div><div class="offre-box offre-efc"><div class="offre-label">Offre EFC</div>${c.oe||"N/A"}</div></div>
  <div style="margin-top:10px;display:flex;gap:6px;flex-wrap:wrap"><span class="tag" style="background:#E6F1FB">Vente: ${vente.join(", ")||"N/A"}</span>${c.ty.map(t=>`<span class="tag">${t}</span>`).join("")}</div>
  <div class="modal-grid"><div class="modal-block"><h4>Freins (${tf})</h4>${bars(FK,FC,FL,c,mf)}</div><div class="modal-block"><h4>Leviers (${tl})</h4>${bars(LK,LC,LL,c,ml)}</div></div>
  <div class="modal-block" style="margin-top:14px"><h4>Outils recommandés (basés sur le profil de freins)</h4><div style="margin-top:6px">${recoHtml}</div></div>
  <div class="modal-grid" style="margin-top:12px"><div class="modal-block"><h4>Impact environnemental</h4><p style="font-size:12px">${c.env||"N/A"}</p></div><div class="modal-block"><h4>Valeur sociale</h4><p style="font-size:12px">${c.soc||"N/A"}</p></div></div>
  <div class="modal-block" style="margin-top:12px"><h4>Récurrence des revenus</h4><p style="font-size:12px">${c.rec||"N/A"}</p></div>
  ${c.src?`<div style="margin-top:10px;font-size:10px;color:var(--muted)">Source: ${c.src}</div>`:""}`;
  document.getElementById("modal-bg").classList.add("show");
}
function closeModal(){document.getElementById("modal-bg").classList.remove("show")}
document.getElementById("modal-bg").onclick=e=>{if(e.target===e.currentTarget)closeModal()};
document.onkeydown=e=>{if(e.key==="Escape")closeModal()};

// MATRIX
function renderMatrix(){
  const rows=mCat==="all"?FREINS_MATRIX:FREINS_MATRIX.filter(r=>r[0]===mCat);
  const catColors={eco:"#185FA5",org:"#533AB7",info:"#D4537E",mar:"#BA7517",jur:"#A32D2D",tec:"#0F6E56",str:"#5F5E5A"};
  const catNames={eco:"Économique",org:"Organisationnel",info:"Informationnel",mar:"Marché",jur:"Juridique",tec:"Technique",str:"Stratégique"};
  let h=`<table><thead><tr><th>Frein</th>${TOOLS.map(t=>`<th style="font-size:9px">${t.link?`<a href="${t.link}" target="_blank" style="color:inherit;text-decoration:none">${t.short}</a>`:t.short}</th>`).join("")}</tr></thead><tbody>`;
  rows.forEach(r=>{const cat=r[0];const col=catColors[cat]||"#888";h+=`<tr><td class="frein-cell"><span class="cat-badge" style="background:${col}20;color:${col}">${catNames[cat]}</span><br>${r[1]}</td>`;for(let i=2;i<r.length;i++){const v=r[i];if(v===2)h+=`<td><span class="dot p"></span></td>`;else if(v===1)h+=`<td><span class="dot s"></span></td>`;else h+=`<td><span class="dot empty"></span></td>`}h+=`</tr>`});
  h+=`</tbody></table>`;document.getElementById("matrix-wrap").innerHTML=h;
}

// CHARTS
const CHART_DEFAULTS={
  font:{family:"'DM Sans',sans-serif"},
  plugins:{legend:{labels:{font:{family:"'DM Sans',sans-serif",size:11},padding:14}},tooltip:{titleFont:{family:"'DM Sans'"},bodyFont:{family:"'DM Sans'"}}},
  scales:{x:{ticks:{font:{family:"'DM Sans',sans-serif",size:10}},grid:{display:false}},y:{ticks:{font:{family:"'DM Sans',sans-serif",size:10}},grid:{color:"#f0eeea"}}}
};
Chart.defaults.font.family="'DM Sans',sans-serif";

function renderCharts(){
  if(chartsRendered)return;chartsRendered=true;
  const C=COMPANIES;

  // 1. EF vs EFC
  const efOnly=C.filter(c=>c.ef&&!c.efc).length;
  const efcOnly=C.filter(c=>c.efc&&!c.ef).length;
  const both=C.filter(c=>c.ef&&c.efc).length;
  new Chart(document.getElementById("ch-ef-efc"),{type:"doughnut",data:{labels:["EFC uniquement","EF uniquement","EF + EFC"],datasets:[{data:[efcOnly,efOnly,both],backgroundColor:["#0F6E56","#185FA5","#533AB7"],borderWidth:2,borderColor:"#fff"}]},options:{responsive:true,plugins:{legend:{position:"bottom"}}}});

  // 2. Taille
  const sizes=["Micro","Petite","Moyenne","Grande","N/A"];
  const sizeCounts=sizes.map(s=>C.filter(c=>c.sz===s).length);
  new Chart(document.getElementById("ch-taille"),{type:"bar",data:{labels:sizes,datasets:[{label:"Entreprises",data:sizeCounts,backgroundColor:["#BA7517","#D4537E","#533AB7","#185FA5","#ccc"],borderRadius:6}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,grid:{color:"#f0eeea"}},x:{grid:{display:false}}}}});

  // 3. Maturité
  const mats=["En place","En développement","Non Abouti","Manque d'information"];
  const matCounts=mats.map(m=>C.filter(c=>c.mat===m).length);
  new Chart(document.getElementById("ch-maturite"),{type:"doughnut",data:{labels:mats,datasets:[{data:matCounts,backgroundColor:["#155724","#BA7517","#A32D2D","#aaa"],borderWidth:2,borderColor:"#fff"}]},options:{responsive:true,plugins:{legend:{position:"bottom"}}}});

  // 4. Secteur d'activité
  const svcAll=["Prestation Intellectuelle","Conception & Fabrication","Maintenance & Réparation","Distribution & Négoce","Service Numérique","Gestion de Déchets","Service à la Personne","Transformation de Matière","Logistique & Transport"];
  const svcCounts=svcAll.map(s=>C.filter(c=>c.sv.some(x=>x.includes(s.split(" ")[0]))).length);
  const svcShort=svcAll.map(s=>s.length>22?s.substring(0,20)+"…":s);
  new Chart(document.getElementById("ch-secteur"),{type:"bar",data:{labels:svcShort,datasets:[{label:"Entreprises",data:svcCounts,backgroundColor:"#185FA5",borderRadius:4}]},options:{indexAxis:"y",responsive:true,plugins:{legend:{display:false}},scales:{x:{beginAtZero:true,grid:{color:"#f0eeea"}},y:{grid:{display:false},ticks:{font:{size:10}}}}}});

  // 5. Leviers par catégorie — En place
  const enPlace=C.filter(c=>c.mat==="En place");
  const levLabels=["Organisationnel","Économique","Humain","Technique","Infrastructure","Réglementaire","Informationnel"];
  const levSums=LK.map(k=>enPlace.reduce((s,c)=>s+(c[k]||0),0));
  new Chart(document.getElementById("ch-leviers"),{type:"bar",data:{labels:levLabels,datasets:[{label:"Total leviers (entreprises « En place »)",data:levSums,backgroundColor:LC,borderRadius:6}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,grid:{color:"#f0eeea"}},x:{grid:{display:false}}}}});

  // 6. Freins — Non abouti + En développement
  const troubled=C.filter(c=>c.mat==="Non Abouti"||c.mat==="En développement");
  const freinLabels=["Organisationnel","Économique","Humain","Technique","Infrastructure","Réglementaire"];
  const freinSumsNA=FK.map(k=>C.filter(c=>c.mat==="Non Abouti").reduce((s,c)=>s+(c[k]||0),0));
  const freinSumsDev=FK.map(k=>C.filter(c=>c.mat==="En développement").reduce((s,c)=>s+(c[k]||0),0));
  new Chart(document.getElementById("ch-freins"),{type:"bar",data:{labels:freinLabels,datasets:[{label:"Non abouti",data:freinSumsNA,backgroundColor:"#A32D2D",borderRadius:4},{label:"En développement",data:freinSumsDev,backgroundColor:"#BA7517",borderRadius:4}]},options:{responsive:true,plugins:{legend:{position:"top"}},scales:{y:{beginAtZero:true,stacked:false,grid:{color:"#f0eeea"}},x:{grid:{display:false}}}}});

  // 7. Secteur × Freins moyens
  const secLabels=["Prestation Intell.","Conception & Fab.","Maintenance","Distribution","Service Num.","Gestion Déchets","Service Personne","Transformation","Logistique"];
  const secKeys=["Prestation","Conception","Maintenance","Distribution","Numérique","Déchets","Personne","Transformation","Logistique"];
  const secFreinAvg=secKeys.map(sk=>{
    const group=C.filter(c=>c.sv.some(s=>s.includes(sk)));
    if(!group.length)return{org:0,eco:0,hum:0,tech:0};
    return{
      org:+(group.reduce((s,c)=>s+(c.fo||0),0)/group.length).toFixed(1),
      eco:+(group.reduce((s,c)=>s+(c.fe||0),0)/group.length).toFixed(1),
      hum:+(group.reduce((s,c)=>s+(c.fh||0),0)/group.length).toFixed(1),
      tech:+(group.reduce((s,c)=>s+(c.ft||0),0)/group.length).toFixed(1),
    };
  });
  new Chart(document.getElementById("ch-secteur-freins"),{type:"bar",data:{labels:secLabels,datasets:[
    {label:"Org.",data:secFreinAvg.map(d=>d.org),backgroundColor:"#A32D2D",borderRadius:3},
    {label:"Éco.",data:secFreinAvg.map(d=>d.eco),backgroundColor:"#D4537E",borderRadius:3},
    {label:"Hum.",data:secFreinAvg.map(d=>d.hum),backgroundColor:"#BA7517",borderRadius:3},
    {label:"Tech.",data:secFreinAvg.map(d=>d.tech),backgroundColor:"#0F6E56",borderRadius:3},
  ]},options:{responsive:true,plugins:{legend:{position:"top"}},scales:{y:{beginAtZero:true,title:{display:true,text:"Moyenne de freins par entreprise",font:{size:11}},grid:{color:"#f0eeea"}},x:{grid:{display:false},ticks:{font:{size:9}}}}}});
}

function render(){renderStats();if(vMode==="cards"){document.getElementById("cards-view").classList.remove("hidden");document.getElementById("table-view").classList.add("hidden");renderCards()}else{document.getElementById("cards-view").classList.add("hidden");document.getElementById("table-view").classList.remove("hidden");renderTable()}}

// Events
document.querySelectorAll(".pill[data-g]").forEach(b=>b.onclick=()=>{document.querySelectorAll(".pill[data-g]").forEach(x=>x.classList.remove("active"));b.classList.add("active");fGeo=b.dataset.g;render()});
document.querySelectorAll(".pill[data-m]").forEach(b=>b.onclick=()=>{document.querySelectorAll(".pill[data-m]").forEach(x=>x.classList.remove("active"));b.classList.add("active");fMat=b.dataset.m;render()});
document.getElementById("search").oninput=e=>{sTerm=e.target.value.toLowerCase();render()};
document.querySelectorAll(".view-btn").forEach(b=>b.onclick=()=>{document.querySelectorAll(".view-btn").forEach(x=>x.classList.remove("active"));b.classList.add("active");vMode=b.dataset.v;render()});
document.querySelectorAll(".pill[data-mc]").forEach(b=>b.onclick=()=>{document.querySelectorAll(".pill[data-mc]").forEach(x=>x.classList.remove("active"));b.classList.add("active");mCat=b.dataset.mc;renderMatrix()});

document.querySelectorAll(".tab-btn").forEach(b=>b.onclick=()=>{
  document.querySelectorAll(".tab-btn").forEach(x=>x.classList.remove("active"));b.classList.add("active");
  document.querySelectorAll(".tab-content").forEach(x=>x.classList.remove("active"));
  document.getElementById("tab-"+b.dataset.tab).classList.add("active");
  if(b.dataset.tab==="matrice")renderMatrix();
  if(b.dataset.tab==="visualisations")renderCharts();
});

render();
</script>
</body>
</html>
