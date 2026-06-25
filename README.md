# 🚆 TransN Checker - version web

Vérificateur d'**attestations mensuelles de temps de travail** pour les mécaniciens **voie normale (VN)** de transN, directement dans le navigateur.

Glissez votre PDF d'attestation → la page le lit, recalcule chaque jour selon la **LDT**, l'**OLDT** et la **CCT transN**, et affiche un tableau coloré avec les anomalies, les indemnités dues et le détail des calculs.

> **Outil indépendant**, non affilié à transN. Votre attestation officielle transN fait foi — cet outil sert à **repérer les écarts** et à **comprendre vos droits**.

---

## ✨ Fonctionnalités

- **Lecture du PDF dans le navigateur** (via [pdf.js](https://mozilla.github.io/pdf.js/)) — analyse par position exacte des colonnes.
- **Moteur de vérification complet** : amplitude, indemnité nuit, SBN, indemnité km, repas, TR/TS/DTA vs GANTT, tours de repos, dimanche/fériés, et contrôles sur le mois (15 nuits/28 j, 13 jours consécutifs, 7 nuits consécutives, temps effectif > 10 h, amplitude > 12 h…).
- **Indemnités km conformes et hors-ligne** : calcul depuis votre **commune de domicile** (table de distances intégrée), formule CCT Annexe 6 / Annexe 3 §2.9.
- **Tableau coloré** par statut (Conforme / Info / Attention / Erreur) + **panneau de détail** par ligne avec l'article CCT/LDT et l'étiquette *avantageux* / *défavorable*.
- **Compteurs du mois** recalculés (TR, DTA, TS, SBN, nuits, repos, indemnités en CHF).
- **Aide / FAQ intégrée** : explication des compteurs, des indemnités, du code couleur, des fériés et des questions fréquentes.
- **Un seul fichier HTML**, aucune installation, fonctionne sur PC / Mac / Linux / tablette / téléphone.

---

## 🔒 Confidentialité

Tout est traité **localement dans votre navigateur**. **Votre PDF ne quitte jamais votre appareil** — il n'est envoyé à aucun serveur. Vos réglages (dépôt, commune) sont mémorisés uniquement dans le stockage local de votre navigateur.

La seule connexion réseau possible est **optionnelle** : si vous géocodez une adresse précise (OpenStreetMap) ou affinez les distances (OSRM). Le calcul par commune, lui, est **100 % hors-ligne**.

---

## 🌐 Mettre en ligne sur GitHub Pages

1. Créez un dépôt et ajoutez-y le fichier **`index.html`** (renommez `transn-checker.html` en `index.html` si besoin).
2. Dans le dépôt : **Settings → Pages**.
3. *Source* : **Deploy from a branch** → branche `main`, dossier `/ (root)` → **Save**.
4. Après ~1 minute, votre page est en ligne à l'adresse :
   ```
   https://<votre-pseudo>.github.io/<nom-du-depot>/
   ```

> La page fonctionne aussi en l'ouvrant simplement en local (double-clic), mais le **géocodage d'adresse** et le **calcul routier OSRM** ne marchent **que sur la version hébergée**. Le calcul des km par commune fonctionne partout.

---

## 🛠️ Utilisation

1. Ouvrez la page → **Paramètres (F2)**.
2. Choisissez votre **dépôt** (Neuchâtel, Fleurier ou La Chaux-de-Fonds).
3. Choisissez votre **commune de domicile** — indispensable pour les indemnités km (calcul conforme et hors-ligne).
4. **Enregistrez**, puis **glissez votre attestation PDF** sur la page (ou *Ouvrir un PDF*).
5. Lisez les résultats : statut coloré par jour, **clic sur une ligne** pour le détail des calculs, bande de compteurs en bas.

Le PDF doit être l'**original transN non modifié**.

---

## ⚙️ Comment ça marche

| Étape | Détail |
|-------|--------|
| **Lecture** | pdf.js extrait le texte avec ses coordonnées ; les colonnes (Date, Tour, Début, Fin, TR, DTA, TS, Ind.nuit, SBN…) sont reconnues par leur position en points PDF. |
| **Calcul** | Portage fidèle du moteur du programme C# : amplitude, nuit 20h–06h (¼h sup.), SBN (formule empirique confirmée), fériés NE (Pâques + Jeûne fédéral), repos par paliers LDT/OLDT, etc. |
| **GANTT** | Tous les tours VN officiels (NE / FLE / CDF, semaine et week-end) sont intégrés comme référence. |
| **Km** | `km A/R = (domicile→prise A/R) − (trajet normal domicile→dépôt A/R)`, plancher 0, × 0.70 CHF/km. Distances issues de la table par commune. |

### Ce qui est vérifié

- TR inférieur/supérieur au GANTT (sous-payé / favorable)
- Indemnité nuit, samedi, dimanche/fériés (manquante ou incorrecte)
- SBN (manquant, sous-calculé, ou présent à tort)
- **Indemnité km** cross-dépôt (manquante / présente / prise proche du domicile)
- **Indemnité repas** (collation hors dépôt, en fenêtre 11h–13h ou 18h–21h, avec vraie pause ≥ 30 min)
- Variants `/1` : recalcul TR / TS / DTA depuis les heures réelles
- Tours de repos : < 8 h (illégal), 8–9 h, 9–11 h, 11–12 h (LDT Art. 8, OLDT Art. 18)
- Travail après jour de repos (consentement, CCT Annexe 4 §1.7)
- Limites mensuelles : 15 nuits / 28 j, 13 jours consécutifs, 7 nuits consécutives, temps effectif > 10 h, amplitude > 11 h / 12 h, solde DTA +72 h / −41 h

---

## ⚠️ Limites connues

- **Géocodage d'adresse / OSRM** : nécessitent la version hébergée et une connexion. Le calcul par commune les remplace hors-ligne.
- Les distances par commune sont des valeurs de centre de commune (suffisantes et conformes pour la quasi-totalité des cas)

---

## 📚 Base légale

- **LDT** — RS 822.21 (Loi fédérale sur la durée du travail)
- **OLDT** — RS 822.211 (Ordonnance d'exécution)
- **CCT transN** — Annexes 1 à 11 (principalement 3, 4 et 6)
- **GANTT VN** transN valides depuis le 14.12.2025

---

## 🧰 Technique

- Un fichier `index.html` autonome (HTML + CSS + JavaScript).
- Dépendance unique chargée depuis un CDN au runtime : **pdf.js** (lecture PDF).
- Services optionnels (version en ligne) : **Nominatim** (géocodage) et **OSRM** (distances routières).
- Aucun *build*, aucun framework, aucune base de données.

---

## 📝 Avertissement

Cet outil est fourni **à titre indicatif**. Il n'est ni développé ni validé par transN. En cas d'écart, référez-vous à votre attestation officielle et au service RH de transN, ou au syndicat (SEV) pour les questions de durée du travail et de repos.

---

## 👤 Crédits

Développé par **SN@KE SWISS**
Portage web du programme TransN Checker (C# / .NET).
