# DIGITALES Academy v2 — Design Spec
**Post-audit UbicMotion · 29/05/2026**
**Date :** 2026-05-31

---

## Contexte

Prototype HTML/CSS/JS tout-en-un (pas de framework, pas de build).
Base : `digitales_mobile_first.html` (1 653 lignes, 5 écrans).
Sortie : `digitales_academy_v2.html`.

Un audit externe (UbicMotion) a identifié que l'interface donnait une impression de prototype généré par IA : trop d'emojis, badges décoratifs, couleurs expressives, ton marketing. La v2 doit être sobre, institutionnelle et crédible.

---

## Contraintes techniques

- Tout CSS et JS restent inline dans le fichier HTML
- Pas de librairies supplémentaires — JS vanilla uniquement
- CDN maintenus : Tabler Icons + Google Fonts (Inter + IBM Plex Sans en remplacement)
- Navigation `go(id)` conservée
- Layout `max-width: 560px`, centré, sans chrome téléphone
- Bottom nav en `position: fixed`
- Tous les écrans suivent le pattern `.page` avec `id="p-xxx"`

---

## Bloc 1 — 12 corrections UI globales (priorité absolue)

Ces corrections s'appliquent à l'ensemble du fichier avant tout ajout.

| # | Correction | Scope |
|---|---|---|
| 1 | Supprimer tous les emojis — remplacer par icônes Tabler ou texte | Global |
| 2 | Remplacer polices : Syne→IBM Plex Sans, DM Sans→Inter | `<head>` + CSS |
| 3 | Refonte palette `:root` — bleu institutionnel `#1E3A5F` | CSS |
| 4 | Gradients sobres — hero, bandeaux, cthumb, promo | CSS |
| 5 | Badges : supprimer décoratifs, reformater utiles | HTML + CSS |
| 6 | Cards cours allégées (cthumb 110px, cbody 12px, ctitle 15px, corner→cmeta) | CSS + HTML |
| 7 | Ton éditorial neutre (textes visibles) | HTML |
| 8 | Icônes : cohérence Tabler, parcimonieux pour IA | HTML |
| 9 | Barre de recherche discrète (filter-btn sobre) | CSS |
| 10 | Appbar sobre (icon-btn opacité réduite, border-radius 8px) | CSS |
| 11 | Bottom nav sans backdrop-filter, ombre discrète, indicateur 20px | CSS |
| 12 | Supprimer tous les blocs `.status-bar` | HTML |

---

## Bloc 2 — Modifications écrans existants

### Écran 1 — Home `#p-home`
- Hero mis à jour : tag sans .pulse, h1 "Accédez aux savoirs universitaires africains", stats corrigées, bouton "Voir le catalogue" sans flèche animée
- Section universités partenaires (4 pills : UO, CS, IS, UA) entre hero et search
- 4 cartes cours mises à jour : label université, inscrits dans .cmeta, corner supprimé
- 5ème carte : Cybersécurité sponsorisée (thème red, badge Sponsorisé·Gratuit)
- Bottom sheet : filtre "Université source" ajouté
- Bottom nav : "Mes cours" → "Assistant" (go('p-chatbot'))

### Écran 2 — Course `#p-course`
- `.cd-tag` : tag université
- `.buy-bar` : bouton "S'inscrire à ce cours" (sans flèche animée)
- Tab Aperçu : item attestation co-signée, encadré validation académique, gbox "Accès garanti 7 jours"
- Onglet "Assistant" ajouté avec chat fonctionnel (sendChatMsg)
- Tab Programme : leçon "Quiz de validation" linkée vers `#p-quiz`

### Écran 3 — Apprenant `#p-apprenant`
- "Mes certificats" → "Mes attestations"
- Section "Recommandé pour vous" avec badge IA
- Item IA dans activité récente (premier)
- Bouton flottant Assistant IA (fixed, bottom:106px)
- Bottom nav : "Catalogue" → "Assistant" (go('p-chatbot'))

### Écran 4 — Admin `#p-admin`
- Bandeau : h2 "DIGITALES Academy", sub "4 universités partenaires actives"
- 2 KPIs supplémentaires : Contenus traités + Universités
- Section "Universités partenaires" (4 cartes + bouton ajout)
- Items gestion : Content Factory + Universités partenaires

### Écran 5 — Profil `#p-profile`
- Groupe Administration : lien "Espace université partenaire" (go('p-universite'))

---

## Bloc 3 — Nouveaux écrans

### Écran 6 — `#p-universite` (Espace université)
Bandeau vert institutionnel, 3 KPIs (cours, apprenants, note).
Section "Mes contenus" : 4 états (Publié, En cours + barre progression, En attente, Brouillon).
Section "Déposer un contenu" : zone upload simulée (simulateUpload), toast confirmation.
Section "Revenus du mois" : 145 000 FCFA, split 75%/25%, bouton virement.
Bottom nav propre à cet écran (5 boutons).

### Écran 7 — `#p-chatbot` (Assistant IA)
Appbar avec statut "En ligne".
Messages pré-chargés (bienvenue + 4 suggestions).
Suggestions → réponses IA contextualisées + 2 cartes cours recommandées.
Input texte avec sendChatbotMsg() (réponse générique).
`const chatbotReplies` avec 4 types : career, skills, project, update.

### Écran 8 — `#p-quiz` (Quiz IA)
Appbar light : titre module, badge IA, timer, barre de progression.
Question affichée avec 4 options (A/B/C/D).
selectAnswer() : correct (vert) / incorrect (rouge + révèle bonne réponse).
nextQuestion() : incrémente compteur, réinitialise options.
Score final : résultat %, message contextuel, encadré "Module validé".

---

## Données mockées

- 4 universités : UO (Ouagadougou), CS (CESAG Dakar), IS (ISTIC Cameroun), UA (Abomey-Calavi)
- 5 cours avec formateurs, durées, prix, inscrits
- Apprenante : Aminata Koné · AK · Ministère du Numérique BF

---

## JS global

```javascript
const pages = ['p-home','p-course','p-apprenant','p-admin','p-profile',
               'p-universite','p-chatbot','p-quiz'];
```

Nouvelles fonctions : `sendChatMsg()`, `sendChatbotMsg()`, `selectSuggestion()`,
`selectAnswer()`, `nextQuestion()`, `simulateUpload()`.

---

## Critères de succès

- Aucun emoji dans l'interface
- Polices Inter + IBM Plex Sans
- Palette sobre `#1E3A5F` comme brand
- 8 écrans navigables avec données cohérentes
- Toutes les interactions JS fonctionnelles
- Design fidèle aux recommandations UbicMotion : neutre, lisible, crédible
