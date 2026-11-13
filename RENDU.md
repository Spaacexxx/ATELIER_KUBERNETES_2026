# Rendu — Atelier Kubernetes

> Complétez ce fichier au fil de l'atelier, puis faites un commit et un push (voir la section « Rendu » du README).

**Nom / Prénom :**
**Groupe :**

---

## Exercice 1 — Cluster

1. Combien de nœuds affiche `kubectl get nodes` ? Quel est le rôle de chacun ?
   >Il affiche 3 noeuds. Il y a un controle plane et deux worker.
2. Que montre `docker ps` ? Qu'est-ce qu'un « nœud » dans k3d, concrètement ?
   >docker ps liste les conteneurs Docker qui tournent. Dans k3d un noeud est un conteneur Docker.

## Exercice 2 — Premier déploiement

3. Quels objets Kubernetes ont été créés par `kubectl create deployment` ? (indice : `kubectl get all`)
   >kubectl create deployment a créé trois objets : un Deployment, un ReplicaSet et un ou plusieurs Pods.
4. Vous avez supprimé le pod `web-...` : que s'est-il passé, et pourquoi ?
   >Kubernetes a recréé automatiquement un nouveau pod avec un nom différent. Le ReplicaSet du Deployment surveille en permanence le nombre de pods : il en veut un certain nombre (par exemple 1), et dès qu'il en manque un, il en crée un autre pour revenir à l'état voulu. C'est l'auto-réparation de Kubernetes.

## Exercice 3 — Modifier un conteneur en direct

5. Après suppression du pod, votre message « Bienvenue... » a-t-il survécu ? Pourquoi ?
   >Non, il n'a pas survécu. Le message était stocké dans le système de fichiers du pod supprimé. Le nouveau pod repart de l'image d'origine, donc sans cette modification.
6. Pourquoi le `port-forward` a-t-il cessé de fonctionner après la suppression du pod ?
   >Parce que kubectl port-forward se connecte à un pod précis, et non au Deployment. Quand ce pod a été supprimé, la connexion est tombée, et le nouveau pod a un autre nom, donc il faut relancer la commande.

## Exercice 4 — Déclaratif

7. Quelle différence entre `kubectl create` et `kubectl apply` ?
   >kubectl create crée une ressource et échoue si elle existe déjà (approche impérative). kubectl apply crée la ressource ou la met à jour selon le fichier YAML (approche déclarative).
8. À quoi sert le `selector` d'un Service ? Que se passe-t-il s'il ne correspond à aucun pod ?
   >Le selector indique à quels pods le Service envoie le trafic, grâce à leurs labels. S’il ne correspond à aucun pod, le Service n’a aucun endpoint et les requêtes échouent.

## Exercice 5 — Scaling & mises à jour

9. Sur quels nœuds se trouvent vos 3 réplicas ? Collez la sortie de `kubectl get pods -l app=site -o wide`.
    >Le pod était en ImagePullBackOff (ou ErrImagePull). C’est kubectl describe pod qui a donné la cause, dans la section Events : l’image nginx:9.99 n’existe pas.
10. Avec l'image `nginx:9.99`, quel statut avait le pod ? Quelle commande vous a donné la cause ?
    >Oui, le site restait accessible. Le rolling update garde les anciens pods en fonctionnement tant que le nouveau pod n’est pas prêt, donc le trafic continue d’aller vers l’ancienne version.
11. Pendant la panne, le site restait-il accessible ? Pourquoi ?
    >Oui, ils parlent au même composant : l’API Server de Kubernetes. Portainer et kubectl sont deux clients de cette même API.

## Exercice 6 — Portainer & Mario

12. URL publique de votre Mario :
    >https://special-space-zebra-4jxvrp69rxqgf5qj-8585.app.github.dev
13. Retrouvez le Deployment `mario` en ligne de commande. Portainer et `kubectl` parlent-ils au même composant ? Lequel ?
    >Oui Portainer et kubectl parlent au meme composant, l'API serveur du closter.

## Bonus (facultatif)

14. Bonus réalisés et remarques :
    >

---

**Score obtenu avec `./scripts/check.sh` :** ... / ...
