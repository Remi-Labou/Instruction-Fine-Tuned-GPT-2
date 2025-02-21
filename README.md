# Instruction-Fine-Tuned-GPT-2

Ce projet vise à affiner un modèle GPT-2 "small" sur un jeu de données d’instructions pour améliorer sa capacité à générer des réponses pertinentes. L’objectif est de transformer le modèle de base en un chatbot performant capable de comprendre et répondre aux questions en langage naturel.

Nous avons suivi une approche méthodique en testant différentes configurations de modèle, techniques de prétraitement des données, stratégies d’entraînement et méthodes d’évaluation afin d’optimiser les performances du modèle.

1. Installation et Importation des Bibliothèques
Nous avons commencé par importer les bibliothèques essentielles telles que PyTorch, Transformers (Hugging Face), Pandas et Datasets pour le traitement des données et l’entraînement du modèle.

Problème rencontré : Initialement, nous avons tenté d’utiliser une autre version de GPT-2 (gpt2-medium) mais avons rencontré des problèmes de mémoire GPU, nous obligeant à revenir à la version "small".

2. Implémentation de GPT-2 Small et Tests Initiaux
Nous avons donc chargé le modèle GPT-2 Small et testé sa génération de texte avec une question basique.

Les réponses étaient souvent répétitives et non structurées.
L’absence d’instruction claire dans les prompts affectait la pertinence des réponses.
Le modèle ne semblait pas bien comprendre les intentions des utilisateurs.
Nous avons donc pensé que l’ajout d’un dataset spécialisé en instruction pourrait améliorer le comportement du modèle.

4. Préparation et Exploration du Dataset
Nous avons utilisé le dataset Stanford Alpaca contenant des paires (instruction, input, output).

Étapes suivies :
Téléchargement du dataset et transformation en DataFrame.
Nettoyage des données (suppression des doublons et gestion des valeurs manquantes).
Réduction du dataset : Pour des raisons de temps d’entraînement, nous avons sélectionné seulement 10% des données.
Fractionnement en ensembles d’entraînement (85%), validation (10%) et test (5%).

Lors de nos premiers test, le modèle générait des réponse relativement cohérente, mais parfois hors contexte. Il fallait donc trouver un moyen d'améliorer la gestion des instructions complexes. Pour cela, nous avons pensé à introduire une mise en forme plus rigoureuse des données en encodant chaque entrée sous unr strucrure plus claire.

4. Fine-Tuning du Modèle GPT-2
Nous avons mis en place un entraînement supervisé en utilisant les instructions formatées comme entrée du modèle.

Optimisations implémentées :
Ajout d’un token de padding pour gérer des séquences de longueurs variables.
Utilisation de batch_size réduit (1) pour éviter les dépassements mémoire GPU.
Introduction de l’AMP (Mixed Precision) pour accélérer l’entraînement et économiser de la mémoire.
Application d’un scheduler linéaire du taux d’apprentissage pour optimiser la convergence.
Implémentation du Gradient Accumulation pour stabiliser l’apprentissage.

5. Évaluation du Modèle et Améliorations
Après l’entraînement, nous avons évalué le modèle avec plusieurs métriques :

Perplexité (PPL) : Permet de mesurer la qualité du modèle sur le texte généré.
Résultat final : 3.37 (correct mais améliorable).

Accuracy Token-Level : Mesure le taux de prédiction correcte des tokens.
Résultat : 40.42 %

BLEU Score : Pour mesurer la similarité entre le texte généré et la réponse attendue.


Problèmes identifiés et solutions :

Problème : Génération encore trop générique.
Solution : Ajuster les hyperparamètres et expérimenter avec différentes stratégies d’augmentation de données.

Problème : Manque de diversité dans les réponses.
Solution : Ajouter un mécanisme de pénalisation des répétitions (Top-k Sampling).

6. Tests et Résultats
Nous avons testé le modèle sur différentes requêtes :

Exemple de question :
"What are the benefits of meditation?"

Réponse générée :
"Meditation helps relax the mind and body, reduces stress, and improves circulation. It is also beneficial for mental clarity and emotional stability."

Comparé à la version initiale de GPT-2, la réponse est plus précise et mieux structurée.

Prochaines améliorations :
Intégrer un meilleur mécanisme de contrôle de la longueur des réponses.
Tester un fine-tuning plus long avec plus de données.
Explorer d’autres architectures comme GPT-3 ou LLaMA pour améliorer la qualité des réponses.
