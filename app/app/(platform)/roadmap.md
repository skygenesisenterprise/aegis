# Roadmap - Console de Gestion Aegis

## Contexte
Aegis est un système de gestion d'identité d'entreprise (Enterprise Identity Management) avec un serveur Go (voir `server/main.go`). Cette console interne est l'interface d'administration pour gérer utilisateurs, accès, domaines et paramètres de sécurité.

La structure s'inspire des consoles modernes (AWS Management Console, Azure Portal, Okta Admin Dashboard) avec Next.js App Router.

## Principes Architecturaux
- Conventions Next.js App Router (groupes de routes, layouts imbriqués)
- TypeScript strict, alias `@/*` (AGENTS.md)
- Tailwind CSS + utilitaire `cn()`
- Nommage : composants PascalCase, fichiers kebab-case
- Contextes avec suffixe `Provider`, hooks avec préfixe `use`
- Interfaces > types pour API publiques (AGENTS.md)
- Error boundaries pour opérations asynchrones (AGENTS.md)

## Structure Globale
Console sous `app/(platform)/`, layout parent `layout.tsx` :
- **Sidebar** : Navigation latérale gauche persistante
- **Header** : Barre supérieure (profil, notifications, recherche)
- **Contenu principal** : Routes enfants (dashboard, modules)

## Structure du Module Dashboard (`app/(platform)/dashboard/`)
Entrée principale de la console, sous-routes correspondant aux modules de gestion :

### 1. Accueil Dashboard (`dashboard/page.tsx`)
- Widgets stats (utilisateurs, sessions actives, connexions récentes, santé système)
- Actions rapides (ajouter utilisateur, créer rôle, audit logs)
- Fil d'activité récente

### 2. Gestion Utilisateurs (`dashboard/users/`)
Aligné avec le système d'identité serveur :
- `page.tsx` : Liste utilisateurs (filtres, recherche, actions masse)
- `new/page.tsx` : Création utilisateur
- `[id]/page.tsx` : Détail/modification utilisateur
- Composants : `user-table.tsx`, `user-filters.tsx`, `user-form.tsx`

### 3. Gestion Rôles & Permissions (`dashboard/roles/`)
- `page.tsx` : Liste rôles + permissions associées
- `new/page.tsx` : Création rôle
- `[id]/page.tsx` : Détail rôle, attribution permissions
- Composants : `role-table.tsx`, `permission-matrix.tsx`

### 4. Gestion Domaines (`dashboard/domains/`)
Aligné avec `DomainService` serveur (`server/src/services/domain.go`) :
- `page.tsx` : Liste domaines + statut vérification
- `new/page.tsx` : Ajout domaine
- `[id]/page.tsx` : Détail domaine, vérification DNS

### 5. Journaux d'Audit (`dashboard/audit-logs/`)
- `page.tsx` : Logs filtrables (actions utilisateur, événements système)
- Composants : `log-table.tsx`, `log-filters.tsx`

### 6. Clés de Service (`dashboard/service-keys/`)
Aligné avec `ServiceKeyService` serveur :
- `page.tsx` : Liste clés API/service, révocation
- `new/page.tsx` : Génération nouvelle clé

### 7. Paramètres (`dashboard/settings/`)
- `page.tsx` : Paramètres généraux console
- `security/page.tsx` : OAuth, sessions, JWT
- `billing/page.tsx` : (Optionnel) Abonnement, usage

### 8. Notifications (`dashboard/notifications/`)
- `page.tsx` : Centre notifications in-app, marquage lu

## Composants Partagés (`app/(platform)/components/`)
- `sidebar.tsx` : Sidebar dépliable
- `header.tsx` : Barre supérieure globale
- `stat-card.tsx` : Widgets stats dashboard
- `data-table.tsx` : Tableau réutilisable (tri, filtrage, pagination)
- `confirm-modal.tsx` : Modales confirmation actions
- `empty-state.tsx` : États vides listes

## Utilitaires & Hooks
- `lib/api.ts` : Client API endpoints Aegis
- `hooks/use-auth.ts` : Hook état authentification
- `hooks/use-toast.ts` : Hook notifications toast
- `lib/cn.ts` : Utilitaire nom de classe (AGENTS.md)

## Phases d'Implémentation
### Phase 1 : Layout & Dashboard
- `app/(platform)/layout.tsx` (sidebar + header)
- `dashboard/page.tsx` (widgets overview)

### Phase 2 : Utilisateurs & Rôles
- Modules `dashboard/users/` et `dashboard/roles/`

### Phase 3 : Audit & Clés de Service
- Journaux d'audit et gestion clés de service

### Phase 4 : Paramètres & Polish
- Paramètres, notifications, finalisation UI
