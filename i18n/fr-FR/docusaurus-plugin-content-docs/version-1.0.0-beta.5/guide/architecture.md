---
slug: /architecture
sidebar_position: 3
---

# Architecture

Assurez-vous d'avoir consulté le [glossaire](/fr-FR/glossary) afin d'avoir une meilleure compréhension de cette section.

## Schéma d'architecture de haut niveau

![Schéma d'architecture de haut niveau de Léon](/img/guide/high-level_architecture_schema.svg "Schéma d'architecture de haut niveau de Léon")

## Scénario

Ce scénario décrit les étapes du schéma ci-dessus. Merci de prendre conscience que la plupart des intéractions sont effectuées via WebSockets.

1. Client (app web, etc.) fait une requête HTTP pour récupérer quelques informations au sujet de Léon.

2. [API HTTP](/fr-FR/glossary#api) retourne les informations au client.

3. Utilisateur parle avec son microphone.

4. <i style={{ opacity: 0 }}>.</i>
   
   a. Si serveur [hotword](/fr-FR/offline#hotword) est lancé, Léon écoute (hors ligne) si utilisateur l'appel en prononçant `Léon`. 
  
   b. Si Léon comprend que utilisateur l'appel, Léon émet un message au serveur principal via une WebSocket. Maintenant Léon écoute (hors ligne) utilisateur.
  
   c. Utilisateur a dit `Bonjour !` à Léon, client transforme l'entrée audio en blob audio.
5. [ASR](/fr-FR/glossary#asr) transforme blob audio en fichier wave.

6. Parseur [STT](/fr-FR/glossary#stt) transforme fichier wave en chaîne de caractères (`Bonjour`).

7. <i style={{ opacity: 0 }}>.</i>
   
   a. Utilisateur reçoit chaîne de caractères et chaîne de caractères est envoyée à [NLU](/fr-FR/glossary#nlu).
   
   b. Ou alors, utilisateur écrit `Bonjour !` avec son clavier (et ignore les étapes 1. à 7.a.). Chaîne de caractères `Bonjour ` est envoyée à NLU.

8. NLU classifie chaîne de caractères et sélectionne classification.

9. Si [journal collaboratif](/fr-FR/collaborative-logger) est activé, classification est envoyée au journal collaboratif.

10. [Cerveau](/fr-FR/glossary#cerveau) crée un processus enfant et exécute le [module](/fr-FR/glossary#modules) choisi.

11. Si [synchroniseur](/fr-FR/glossary#synchroniseur) est activé et module contient cette option, le contenu se synchronise.

12. Cerveau crée une [réponse](/fr-FR/glossary#reponses) et la transfère au synthétiseur [TTS](/fr-FR/glossary#tts).

13. Synthétiseur TTS transforme réponse texte (et l'envoie à utilisateur en tant que texte) en buffeur audio qui est joué par client.
