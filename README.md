# Rapport d'Analyse Statique APK

## 1\. Informations générales

* Date d’analyse : 26 avril 2026
* Analyste : Mounir Merghich
* APK analysé : app-debug.apk
* Outils : MobSF + VM Mobexler

## 2\. Résumé exécutif

L’analyse statique de l’application Android révèle un niveau de risque global **élevé**. Plusieurs vulnérabilités critiques ont été identifiées, notamment la présence de configurations de débogage, des composants exportés non sécurisés et des communications réseau potentiellement non chiffrées. L’application présente également des faiblesses liées au stockage d’informations sensibles.

## 3\. Préparation de l’environnement

* Création d’un dossier d’analyse sécurisé
* Copie de l’APK
* Calcul du hash SHA-256
* Documentation de la traçabilité

## 4\. Lancement de MobSF

* Démarrage via : `./run.sh 127.0.0.1:8000`
* Accès via navigateur : `http://127.0.0.1:8000`
* Interface opérationnelle

## 5\. Analyse de l’APK

* Import de l’APK dans MobSF
* Analyse automatique réalisée
* Score de sécurité : **\[SCORE\]**

## 6\. Analyse du manifeste

### Configurations sensibles

* android:debuggable = true
* android:allowBackup = true
* usesCleartextTraffic = true

### Permissions dangereuses

* ACCESS\_FINE\_LOCATION
* READ\_CONTACTS
* INTERNET

### Composants exportés

* MainActivity (exported=true)
* Service exposé sans protection

## 7\. Analyse réseau

* Absence de configuration réseau sécurisée
* Autorisation du trafic HTTP
* Endpoints détectés :
   * http://api.example.com
   * https://test.example.com

## 8\. Analyse du code et ressources

### Vulnérabilités détectées

* Clés API hardcodées
* Logs sensibles exposés
* Mauvaise gestion des erreurs

### Secrets détectés

* API\_KEY = "123456"

## 9\. Corrélation OWASP MASVS

* Vulnérabilité : Clé API hardcodée  
 Référence : MSTG-STORAGE-14  
 Impact : fuite de données sensibles
  
* Vulnérabilité : HTTP non sécurisé  
 Référence : MSTG-NETWORK-1  
 Impact : interception des communications

## 10\. Top vulnérabilités

* Clés API en clair dans le code
* Traffic HTTP autorisé
* Mode debug activé
* Composants exportés non sécurisés

## 11\. Recommandations

* Supprimer les clés API du code source
* Désactiver le mode debug en production
* Forcer HTTPS uniquement
* Limiter les composants exportés
* Implémenter Network Security Config

## 12\. Conclusion

L’application présente plusieurs vulnérabilités critiques pouvant compromettre la sécurité des données. Une correction rapide est nécessaire avant toute mise en production.

## 13\. Annexes

### Permissions

* ACCESS\_FINE\_LOCATION
* READ\_CONTACTS

### Endpoints

* http://api.example.com

### Composants exportés

* MainActivity
