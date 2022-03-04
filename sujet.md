# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers


### 1) Log4Shell 

Log4Shell est le nom donné à une faille de la bibliothèque open Source d'Apache Log4J qui permet la journalisation(logs).  

Révélée par le chercheur en sécurité Chen Zhaojun d’Alibaba Cloud Security, la faille Log4Shell permet de provoquer une connexion non souhaitée vers une ressource potentiellement malveillante par le biais de l’interface JDNI (Java Naming and Directory Interface). Il s’agit là d’un protocole qui permet de se connecter à des ressources locales ou distantes  

Également connu sous le numéro CVE-2021-45105, ce bug permettait d’injecter lors des requêtes LDAP et JNDI codes qui permettaient aux attaquants d’exécuter du code java arbitraire sur un server ou un ordinateur. Il est lieu de rappeler que cette bibliothèque est utilisée par beaucoup de service, entreprises privées comme publiques, des organisations. Cette faille est considérée comme la plus importante et la plus critique de la dernière décennie. Elle a eu une note CVSS de 10. 

Presque tout internet est soumis à ce bug, qui permettait a des malveillants de se connecter en local ou distance en y injectant un code malveillant. Vu le niveau d’utilisation de bibliothèque en question, tout le monde de l’internet y était affecté les géants comme les petits. 

Ce bug est considère local déjà qu’il survient lors de la saisie de l’utilisateur. Pour palier je pense qu’un test aurait pu aider à découvrir cette faille bien avant. 

https://nakedsecurity.sophos.com/2021/12/13/log4shell-explained-how-it-works-why-you-need-to-know-and-how-to-fix-it/ 

https://www.01net.com/actualites/la-faillelog4shell-extremement-critique-met-la-toile-en-ebullition-2052523.html 

https://avandeursen.com/2014/08/29/think-twice-before-using-the-maintainability-index/ 

https://www.lemondeinformatique.fr/actualites/lire-faille-dans-log4j-encore-un-bug-encore-un-patch-85171.html 


### 2) https://issues.apache.org/jira/browse/COLLECTIONS-796  

Bug *SetUniqueList.createSetBasedOnList doesn't add list elements to return value*. 

Le bug est de type local.  

Explication du bug : La fonction `SetUniqueList.createSetBasedOnList` retourne la liste après son appel. Mais un commit modifiant la classe `SetUniqueList` a également supprimé la ligne 344 `subSet.addAll(list);`. Commit : https://github.com/apache/commons-collections/commit/b1c45ac691d46a8c609f2534d2adfa59c0599527?diff=split#diff-8e53271d5d8299a76d43b0e3c81740fbe660083ae71c5bf2be63846d52156f23L344  

La suppression est sûrement involontaire. 

Solution du bug : rajout de la ligne `subSet.addAll(list);` dans la méthode `createSetBasedOnList`.

Commit de correction : https://github.com/apache/commons-collections/pull/255/files/e2564ab6e2ef22f212e09f4c7db33d1b18d803d6#diff-8e53271d5d8299a76d43b0e3c81740fbe660083ae71c5bf2be63846d52156f23R355  

Le commit corrigeant l’erreur n’a pas ajouté de nouveaux tests pour détecter le bug s’il se reproduit. 

### 3) 

Pratique consistant à éprouver des environnements IT en créant des événements perturbateurs, l’ingénierie du chaos est aujourd’hui expérimentée par une majorité d'entreprises 

Plus précisément, 
l'ingénierie du chaos est une discipline qui consiste à expérimenter sur un système distribué 
afin de renforcer la confiance dans sa capacité à résister à des conditions turbulentes en production. 

 Les variables métriques suivies 

 - Les  SPS, 

   - Basculement territorial 
   - Le taux de connexion  
- Metrique de grain fin 
   - Le taux de latence 
   - La charge des requetes  
   - L’utilisation du CPU du système 
   - La charge du processeur 

Les expériences concrètes 
- Construire des hypothèses autour d’un comportement stable 
- Varier les expériences du monde réel 
- Les résultats des expériences en production 
- Automatiser les expériences pour les exécuter en continues 

Nous avons également remarqué que des organisations telles qu'Amazon, Google, Microsoft et Facebook appliquaient des techniques similaires pour tester la résilience de leurs propres systèmes 

Cette technique peut être appliquée dans d’autres domaines autre le streaming, il faudrait définir les principes suivants :  

- L’adoption : Quelles sont les approches qui réussissent à faire adhérer une organisation à cette et pour que les équipes d'ingénieurs de l'organisation l'adoptent ? 
- L’outillage : définir ses métriques qu’il faudrait suivre, ses infrastructures internes et ses outils internes permettant de faire ces suivis 
- Modèle d’injection d’évènement :  Définir les évènements à injecter et aussi la combinaison des évènements  

 

### 4) WebAssembly  

Les principaux avantages à avoir une spécification formelle sont :  
- WebAssembly est langage bas niveau qui est indépendant de la plateforme, du langage haut niveau et du navigateur qui l'utilise. Cela le rend plus flexible. 
- WebAssembly est rapide, compact, protable et sécurisé.

WebAssembly est fondé sur 7 concepts principaux : Modules, Functions, Instructions, Trap, Machine Types, Local Variables, Global Variables.

WebAssembly propose un graphe de flow de contrôle permettant notamment de détecter les boucles infinies. 

Cela ne signifie pas que WebAssembly ne devrait pas être testé.

### 5) Isabelle et WebAssembly

#### What are the main advantages of the mechanized specification? 

Les principaux avantages de la spécification mécanisée sont : 
- l'apport d'une preuve de la solidité du system WebAssembly
- la détection de certains problèmes, notamment avec l'opération Trap ou Return

#### Did it help improving the original formal specification of the language? 

La détection de ces erreurs a permis d'améliorer la spécification du langage. Par exemple,  après avoir trouvé un contre exemple à l'opération Trap, l'équipe a fourni une solution à ce problème qui a été ajoutée à la spécification du langage.

#### What other artifacts were derived from this mechanized specification?

En plus de la spécification mécanisée, l'équipe a développé un Type Checker et un Interpréteur en Isabelle.

#### How did the author verify the specification? 

L'auteur a testé la spécification en la soumettant aux tests, en utilisant les outils CSmith et Binaryen pour convertir les tests C en WebAssembly. Aucune erreur n'a été détectée.

#### Does this new specification removes the need for testing?

Cette nouvelle spécification n'enlève pas le besoin de tester.
