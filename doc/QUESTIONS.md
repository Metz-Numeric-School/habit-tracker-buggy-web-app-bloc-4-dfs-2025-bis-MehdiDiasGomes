# Questions

Répondez ici aux questions théoriques en détaillant un maxium vos réponses :

1) Expliquer la procédure pour réserver un nom de domaine chez OVH avec des captures d'écran (arrêtez-vous au paiement) :

Pour réserver un nom de domaine chez OVH, je procède comme suit :

- Je me rends sur le site ovh.com et je clique sur "Domaines"
- J'utilise la barre de recherche pour vérifier la disponibilité du nom de domaine souhaité
- Je sélectionne l'extension appropriée (.fr, .com, .org, etc.)
- J'ajoute le domaine au panier
- Je choisis la durée d'enregistrement (1 an minimum)
- Je configure les options DNS selon mes besoins
- Je procède à la commande en créant un compte OVH si nécessaire
- J'arrive à l'étape de paiement où je m'arrête

2. Comment faire pour qu'un nom de domaine pointe vers une adresse IP spécifique ?

Pour faire pointer un nom de domaine vers une adresse IP, je dois configurer les enregistrements DNS :

- Je me connecte à l'interface de gestion DNS de mon registraire (OVH dans ce cas)
- Je crée un enregistrement de type "A" pour le domaine principal
- Je renseigne l'adresse IP de destination (par exemple 172.17.4.8)
- J'ajoute un enregistrement "CNAME" pour le sous-domaine www pointant vers le domaine principal
- Je sauvegarde les modifications
- J'attends la propagation DNS (entre 24h et 48h maximum)

3. Comment mettre en place un certificat SSL ?

Pour mettre en place un certificat SSL :

- Je me connecte à aaPanel
- Je vais dans la section "Website" puis je clique sur mon site
- Dans l'onglet "SSL", je sélectionne "Let's Encrypt"
- Je génère automatiquement le certificat gratuit
- J'active la redirection HTTPS automatique
