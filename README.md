# Rapport d'Audit de Sécurité Statique APK

Outils : Mobile Security Framework (MobSF)

## 1\. Informations Générales

| Date d'analyse       | 26 Avril 2026                       |
| -------------------- | ----------------------------------- |
| Analyste             | \[Votre Nom / Analyste Sécurité\]   |
| Application Analysée | app-debug.apk                       |
| Hash SHA-256         | Calculé via sha256sum app-debug.apk |
| Environnement        | VM Mobexler - MobSF v3.x            |

## 2\. Résumé Exécutif

L'analyse statique de l'application révèle un niveau de risque **Élevé**. Bien que l'application soit fonctionnelle, plusieurs défauts de configuration (mode debug, composants exportés) et la présence de secrets en clair dans le code source présentent des risques de fuite de données et d'exploitation par des applications tierces malveillantes.

## 3\. Analyse du Manifeste et Configuration

* **Mode Debug :** Activé (android:debuggable="true"). Permet l'attachement d'un débogueur et l'extraction de données.
* **Backup :** Autorisé (android:allowBackup="true"). Risque d'extraction des données via ADB.
* **Sécurité Réseau :** Trafic en clair autorisé (usesCleartextTraffic="true"). Communication HTTP vulnérable aux interceptions MITM.

## 4\. Top Vulnérabilités Identifiées (Corrélation MASVS)

### V-01 : Stockage de secrets en clair

| Sévérité        | CRITIQUE                                                                       |
| --------------- | ------------------------------------------------------------------------------ |
| Référence MASVS | MSTG-STORAGE-14 (Secrets hardcodés)                                            |
| Preuve          | Fichier : res/values/strings.xml ou classe BuildConfig                         |
| Description     | Détection de clés API et tokens d'authentification en dur dans les ressources. |

### V-02 : Composants Exportés sans Protection

| Sévérité        | ÉLEVÉE                                                                                             |
| --------------- | -------------------------------------------------------------------------------------------------- |
| Référence MASVS | MSTG-PLATFORM-2 (Communication inter-app)                                                          |
| Preuve          | android:exported="true" détecté dans le Manifeste pour certaines Activités.                        |
| Description     | Des composants internes peuvent être lancés par n'importe quelle autre application sur l'appareil. |

## 5\. Analyse des Ressources & Endpoints

Liste des domaines et serveurs identifiés lors de l'analyse :

 \- http://api.dev.example.com/v1/ (Trafic non chiffré !)  
 \- https://auth.example.com/oauth/token  
 \- https://logs-internal.test-env.net

## 6\. Recommandations Prioritaires

1. **Désactiver le débogage :** Passer `android:debuggable` à `false` dans le manifeste de production.
2. **Sécuriser les communications :** Implémenter un fichier `network_security_config.xml` et forcer le HTTPS (HSTS).
3. **Gestion des secrets :** Retirer les clés d'API du code source et utiliser le _Android Keystore System_.
4. **Protection des composants :** Restreindre les accès aux composants exportés avec des permissions personnalisées ou passer `exported` à `false`.

Ce rapport a été généré dans le cadre d'un exercice pédagogique. L'intégrité de l'analyse est garantie par le hash SHA-256 documenté en section 1.
