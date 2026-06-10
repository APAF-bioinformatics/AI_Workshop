---
name: emoticon_mapping
description: Emoticon mappings and logic primitives tailored for the Immunity Protocol game workshop and debugging.
---
<!-- APAF Metadata
last_verified_date: 2026-06-10
apaf_approved: true
apaf_version: 1.3
apaf_org: APAF Bioinformatics
-->

# r_emoticon_mapping: Logic Primitives for Immunity Protocol Workshop

This skill defines standardized emojis, text emoticons, and debugging terms for the **Immunity Protocol - Biome Defense** game development and diagnostics.

## 1. Core Mappings

### A. General Emotion & SDLC Mappings
`EMO: {Joy:[😊,:),"Success"];Grief:[😢,:(,"Fail"];Fear:[😨,:-O,"Risk"];Disgust:[🤢,:-P,"Smell"];Rage:[😠,>:(,"Err"];Surprise:[😮,:O,"Disc"];Trust:[🤝,:),"Agree"];Antic:[⏳,:-),"Wait"]}`
`SDLC: {Plan:[🗺️,[],"Arch"];Analys:[🔬,< >,"Expl"];Spec:[📐,/ \,"Spec"];Impl:[🛠️,>-<,"Code"];Test:[🧪,(),"Valid"];Deploy:[🚀,^^,"Merge"];Maint:[🩹,++,"Refac"]}`
`EQ: {Explorer:[🕵️,"?_?","Disc"];Troubleshooter:[🩹,"o_o","Fix"];Collaborator:[🎉,"*_*","Celeb"];Analytic:[📊,"- -","Data"]}`

### B. Workshop Game Cell Mappings (Lineage Selection)
`GAME_CELLS: {Macrophage:[🛡️,"[M]","Defense"];NKCell:[⚡,"[NK]","Target"];Neutrophil:[💥,"[N]","Blast"]}`

### C. Pathogen & Boss Mappings
`GAME_PATHOGENS: {Virus:[👾,"(V)","Infect"];Bacteria:[🦠,"(B)","Divide"];Boss:[👹,"{BOSS}","Encounter"]}`

### D. Physics & Collision Debugging Mappings
`PHYSICS_DIAGNOSTICS: {Tunneling:[🎯,"x->o","Raycast"];FPS_Independent:[⏱️,"dt","Physics"];Friction:[💨,"mu","Damping"]}`

### E. Shield & HUD Mappings
`HUD_HUD: {Complement:[🔋,"[===]","HUD"];Shield:[🧱,"###","Barrier"]}`

## 2. Directives
`MATCH: Phase|Emotion | SYNC: Agents | LIMIT: Msg Start/End`

<!-- APAF Bioinformatics | SKILL.md | Approved | 2026-06-10 -->
