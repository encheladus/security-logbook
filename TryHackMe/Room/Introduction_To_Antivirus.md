# Introduction to Antivirus — TryHackMe - Red Team  
**Date:** 2025-12-01  
**Type:** TryHackMe  
**Scope:** Authorized Lab

---

## 1) Contexte
L’objectif du lab : comprendre *comment fonctionne un antivirus*, quelles techniques de détection il utilise, comment tester ses payloads sans les “burn”, et comment fingerprint l’AV/EDR présent sur une machine cible.

Le room couvre :

- Les moteurs et composants d’un AV moderne  
- Les types de détection : signatures, heuristique, comportement, sandbox  
- La création de signatures (ClamAV, Yara)  
- L’anti-sandboxing  
- Les plateformes de test multi-AV (VirusTotal vs alternatives)  
- Le fingerprinting d’AV/EDR en post-exploitation

---

## 2) Hypothèses de départ
- Un pentester/red-team doit comprendre les mécanismes des AV pour ensuite mieux les contourner.  
- Les détections modernes sont hybrides : statique + dynamique + heuristique.  
- On doit savoir **tester** un payload sans l’exposer aux vendors.  
- On doit savoir **identifier** l’AV/EDR sur un hôte avant de lancer quoi que ce soit.

---

## 3) Méthode / outils

### ✔ Comprendre comment un AV fonctionne
Étude de la structure d’un moteur AV :

- **Scanners** (temps réel, on-demand, mémoire, registre…)
- **Détection statique** (signatures, hashes, motifs)
- **Détection heuristique** (patterns de code, structures PE suspectes)
- **Analyse dynamique** (API hooking, sandboxing, comportement)
- **Unpackers & décompression** (UPX, ASPack, Themida…)
- **Émulateurs** pour exécuter le malware en environnement contrôlé

### ✔ Créer des signatures
- Création d’une signature ClamAV via `sigtool` (MD5 → `.hdb`)
- Écriture d’une règle Yara basique → puis améliorée (check du header “MZ”)  
  → apprentissage des **false positives** et de leur réduction.

### ✔ Comportement dynamique & heuristique
- API monitoring (VirtualAlloc, WriteProcessMemory, CreateRemoteThread, registry ops…)
- Sandbox detection & anti-VM
- Détection comportementale (ransomware patterns, injection, réseau)

### ✔ Tester un payload sans le brûler
- VirusTotal = OK pour PoC, *interdit* pour payload réel (partage avec vendors)
- Plateformes no-share :
  - **Antiscan.me**
  - **JottiScan**
- Construire **son propre lab AV** pour les tests sensibles

### ✔ Fingerprinting AV/EDR
- Identifier l’AV via :
  - Processus (`MsMpEng.exe`, `avp.exe`, `bdagent.exe`, …)
  - Services (`WinDefend`, `AVPxx`, `TMBMSRV`, …)
  - Clés registre
  - Dossiers d’installation
  - DLLs chargées
  - Patterns réseau (télémétrie)

### ✔ Analyse d’éditeurs connus
Tableau étudié : Defender, Kaspersky, Avast, Bitdefender, Norton, AVG, Trend Micro, etc.

### ✔ Outils
- **SharpEDRChecker** (enumeration signatured)
- Petit **fingerprinter C#** basé sur les process WMI :
  - WMI → `select * from win32_process`
  - Liste de process AV connus
  - Match → impression

Limites : WMI peut être monitoré → bruyant selon EDR.

---

## 4) Issues rencontrées

### **Issue 1 — Comment tester mes payloads sans les cramer ?**
- VirusTotal partage tout → transformation immédiate en signature → payload mort.
- Solution :
  - VT uniquement pour tests très génériques / non sensibles
  - Pour les “vrais” payloads : Antiscan.me, Jotti, ou lab personnel isolé

### **Issue 2 — Comment savoir quel AV/EDR tourne réellement sur la cible ?**
Sans fingerprinting, je suis aveugle :
- Je ne sais pas si c’est Defender, CrowdStrike, SentinelOne, Sophos, Bitdefender…
- Impossible d’adapter la stratégie d’évasion ou de reproduire l’environnement exactement en lab.

---

## 5) Solutions / workflow clair

### **1) Tester proprement sans se brûler**
- Étape 1 : tests rapides → VT (uniquement sur code non confidentiel)
- Étape 2 : tests “réels” → no-share (Antiscan / Jotti)
- Étape 3 : validation finale → lab AV privé

### **2) Fingerprinting AV/EDR sur la cible**
Techniques discrètes :
- Process enumeration (idéalement sans WMI)
- Services
- Registry keys (produits installés)
- Chemins connus (ProgramData, Program Files, drivers)
- DLL signatures (ex : `msmpeng.dll`)

### **3) Utiliser les bons outils**
- SharpEDRChecker → parfait en environnement lab
- Code C# custom → plus discret, contrôle du bruit
  
### **4) Connaitre par cœur les AV processus/services**
Exemple :
- Defender = `MsMpEng.exe` / service `WinDefend`
- Kaspersky = `avp.exe`
- Avast = `AvastSvc.exe`
- Bitdefender = `vsserv.exe`, `bdagent.exe`

→ Permet de décider rapidement si un bypass Defender suffit, ou si l’endpoint a une solution lourde type EDR.

---

## 6) Ce que j’ai appris

- **VirusTotal ≠ environnement de test pour payloads d’engagement**  
  Car il publie les samples aux éditeurs → signature immédiate.

- Les AV modernes mélangent :
  - signatures
  - heuristique
  - détection comportementale
  - sandbox (éventuellement cloud)

- La **static detection seule est triviale à bypass** (packing, chiffrement, randomisation)

- La **dynamic detection** est bien plus dangereuse :
  - API hookées
  - injection détectée
  - comportement suspect monitoré
  - sandbox + ML

- Le **fingerprinting AV/EDR** doit toujours être une étape PRE-payload :
  - pour avoir un environnement identique en lab
  - pour choisir les bonnes techniques d’évasion
  - pour éviter de déclencher des alertes bêtes (WMI spam, etc.)

- Un bon red teamer ne lance jamais un payload “à l’aveugle”.

---

## 7) Takeaways

- Ne jamais charger un payload opérationnel sur VirusTotal.  
- Toujours fingerprint la machine avant toute phase offensive.  
- Maîtriser les signatures AV courantes → gain de vitesse énorme.  
- Construire un lab AV réaliste et tester avec discipline.  
- Minimalisme > outils “all-in-one” quand la discrétion compte.  
- Connaître comment un AV voit ton payload = comprendre comment le contourner.

---