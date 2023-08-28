---
title: Abilitare Headless Adaptive Forms su AEM Forms as a Cloud Service
seo-title: Step-by-Step Guide for enabling Headless Adaptive Forms on AEM Forms as a Cloud Service
description: Scopri come abilitare i moduli adattivi headless su AEM Forms as a Cloud Service con la nostra guida dettagliata. Il nostro tutorial illustra la procedura per abilitare facilmente questa potente funzione per il tuo ambiente AEM Forms.
seo-description: Learn how to enable headless adaptive forms on AEM Forms as a Cloud Service with our step-by-step guide. Our tutorial walks you through the process, making it easy to enable this powerful feature for your AEM Forms environment.
solution: Experience Manager Forms
feature: Adaptive Forms
topic: Headless
role: Admin
level: Beginner, Intermediate
contentOwner: Khushwant Singh
docset: CloudService
hide: true
hidefromtoc: true
exl-id: 7c545ca6-cb2d-4d28-b9e8-b6efe3faee00
source-git-commit: 47ac7d03c8c4fa18ac3bdcef04352fdd1cad1b16
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 3%

---

# Abilitare Headless Adaptive Forms su AEM Forms as a Cloud Service {#enable-headless-adaptive-forms-on-aem-forms-cloud-service}

L’abilitazione di Headless Adaptive Forms su AEM Forms as a Cloud Service consente di iniziare a creare, pubblicare e distribuire Headless Forms utilizzando le istanze di Cloud Service AEM Forms su più canali. Per utilizzare Headless Adaptive Forms è necessario un ambiente abilitato per i Componenti core Forms adattivi.

## Considerazioni

* Quando crei un nuovo programma AEM Forms as a Cloud Service, [I Forms headless adattivi sono già abilitati per i tuoi ambienti](#are-adaptive-forms-core-components-enabled-for-my-environment).

* Se hai un programma Forms as a Cloud Service precedente in cui i Componenti core sono [non abilitato](#enable-components), è possibile [aggiungere dipendenze dei componenti core di Forms adattivi](#enable-headless-adaptive-forms-for-an-aem-forms-as-a-cloud-service-environment) nell’archivio as a Cloud Service per AEM e implementa l’archivio negli ambienti di Cloud Service per abilitare Headless Adaptive Forms.

* Se l&#39;ambiente di Cloud Service esistente fornisce l&#39;opzione [creazione di un Forms adattivo basato su Componenti core](create-a-headless-adaptive-form.md), i Forms headless adattivi sono già abilitati per il tuo ambiente e puoi distribuire Forms adattivo basato su Componenti core come moduli headless a canali quali dispositivi mobili, web, app native e servizi che richiedono una rappresentazione headless di Forms adattivo.


>[!NOTE]
>
>
> Adobe fornisce Forms adattivo [kit di avvio (app React)](create-and-publish-a-headless-form.md) per aiutare gli sviluppatori a iniziare rapidamente con lo sviluppo di Forms adattivo headless, senza abilitare Forms adattivo headless nell’ambiente AEM Forms as a Cloud Service. Puoi abilitare il Forms adattivo headless in un ambiente Forms as a Cloud Service in un secondo momento dopo un [sviluppo di moduli headless pratico e veloce](create-and-publish-a-headless-form.md).

## Abilitare Headless Adaptive Forms per un ambiente AEM Forms as a Cloud Service

Per abilitare Headless Adaptive Forms per un ambiente AEM Forms as a Cloud Service, effettua le seguenti operazioni, nell’ordine elencato


![](/help/assets/enable-headless-adaptive-forms-on-aem-forms-cloud-service.png)


## 1. Clona l’archivio Git as a Cloud Service di AEM Forms {#clone-git-repository}

1. Accedi a [Cloud Manager](https://my.cloudmanager.adobe.com/) e seleziona la tua organizzazione e il tuo programma.

1. Accedi a **Pipeline** scheda dal tuo **Panoramica del programma** , fare clic su **Accedi a dati archivio** per accedere e gestire l’archivio Git. La pagina include le seguenti informazioni:

   * URL dell’archivio Git di Cloud Manager.
   * Credenziali dell’archivio Git (nome utente e password), nome utente Git.

   Clic **Genera password** per visualizzare o generare la password.

1. Aprire il terminale o il prompt dei comandi sul computer locale ed eseguire il comando seguente:

   ```Shell
   git clone [Git Repository URL]
   ```

   Quando richiesto, immettere le credenziali. L&#39;archivio viene clonato nel computer locale.


## 2. Aggiungere all’archivio Git le dipendenze dei Componenti core adattivi di Forms {#add-adaptive-forms-core-components-dependencies}

1. Apri la cartella dell’archivio Git in un editor di codice di testo normale. Ad esempio, Codice VS.
1. Apri `[AEM Repository Folder]\pom.xml` file per la modifica.
1. Sostituisci versioni di `core.forms.components.version`, `core.forms.components.af.version` e `core.wcm.components.version` componenti con versioni specificate in [documentazione dei componenti core](https://github.com/adobe/aem-core-forms-components). Se il componente non esiste, aggiungi questi componenti.

   ```XML
   <!-- Replace the version with the latest released version at https://github.com/adobe/aem-core-forms-components/tags -->
   
   <properties>
       <core.forms.components.version>2.0.14</core.formscomponents.version>
       <core.forms.components.af.version>2.0.14</core.forms.components.af.version>  
       <core.wcm.components.version>2.21.2</core.wcmcomponents.version>
   </properties>
   ```

   ![Menzionare la versione più recente dei componenti core di Forms](/help/assets/latest-forms-component-version.png)

1. Nella sezione dipendenze della sezione `[AEM Repository Folder]\pom.xml` , aggiungere le dipendenze seguenti e salvare il file.

   ```XML
       <!-- WCM Core Component Examples Dependencies -->
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.apps</artifactId>
               <type>zip</type>
               <version>${core.wcm.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.content</artifactId>
               <type>zip</type>
               <version>${core.wcm.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.config</artifactId>
               <version>${core.wcm.components.version}</version>
               <type>zip</type>
           </dependency>    
           <!-- End of WCM Core Component Examples Dependencies -->
           <!-- Forms Core Component Dependencies -->
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-core</artifactId>
               <version>${core.forms.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-apps</artifactId>
               <version>${core.forms.components.version}</version>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-af-core</artifactId>
               <version>${core.forms.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-af-apps</artifactId>
               <version>${core.forms.components.version}</version>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-apps</artifactId>
               <type>zip</type>
               <version>${core.forms.components.version}</version>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-content</artifactId>
               <type>zip</type>
               <version>${core.forms.components.version}</version>
           </dependency>
   <!-- End of AEM Forms Core Component Dependencies -->
   ```

1. Apri `[AEM Repository Folder]/all/pom.xml` file per la modifica. Aggiungi le dipendenze seguenti nel `<embeddeds>` e salva il file.

   ```XML
   <!-- WCM Core Component Examples Dependencies -->
   
   <!-- inside plugin config of filevault-package-maven-plugin -->  
   <!-- embed wcm core components examples artifacts -->
   
   <embedded>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples.ui.apps</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples.ui.content</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples.ui.config</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <!-- embed forms core components artifacts -->
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-af-apps</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/application/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-af-core</artifactId>
       <target>/apps/${appId}-vendor-packages/application/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-examples-apps</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   <embedded>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-examples-content</artifactId>
       <type>zip</type>
       <target>/apps/${appId}-vendor-packages/content/install</target>
   </embedded>
   ```

   >[!NOTE]
   >
   >
   >  Sostituisci `${appId}` con il tuo appId.
   >
   >  Per trovare `${appId}`, nella `[AEM Repository Folder]/all/pom.xml` file, cerca nel `-packages/application/install` termine. Il testo prima del `-packages/application/install` il termine è il tuo `${appId}`. Ad esempio, il codice seguente: `myheadlessform` è `${appId}`.
   >
   >   ```
   >             <embedded>
   >                     <groupId>com.myheadlessform</groupId>
   >                     <artifactId>myheadlessform.ui.apps<artifactId>
   >                     <type>zip</type>
   >                   <target>/apps/myheadlessform-packages/application install</target>
   >             </embedded>
   >   ```

1. In `<dependencies>` sezione del `[AEM Repository Folder]/all/pom.xml` , aggiungere le dipendenze seguenti e salvare il file:

   ```XML
           <!-- Other existing dependencies -->
           <!-- wcm core components examples dependencies -->
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.apps</artifactId>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.config</artifactId>
               <type>zip</type>
               </dependency>
           <dependency>
               <groupId>com.adobe.cq</groupId>
               <artifactId>core.wcm.components.examples.ui.content</artifactId>
               <type>zip</type>
           </dependency>
               <!-- forms core components dependencies -->
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-af-apps</artifactId>
               <type>zip</type>
           </dependency>
           <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-apps</artifactId>
               <type>zip</type>
           </dependency>
               <dependency>
               <groupId>com.adobe.aem</groupId>
               <artifactId>core-forms-components-examples-content</artifactId>
               <type>zip</type>
           </dependency>
   ```

1. Apri `[AEM Repository Folder]/ui.apps/pom.xml` per la modifica. Aggiungi il `af-core bundle` e salvare il file.

   ```XML
       <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>core-forms-components-af-core</artifactId>
       </dependency>
   ```

   >[!NOTE]
   >
   >Assicurati che i seguenti artefatti dei Componenti core adattivi di Forms non siano inclusi nel progetto.
   >
   > `<dependency>`
   >
   >   `<groupId>com.adobe.aem</groupId>`
   >   `<artifactId>core-forms-components-apps</artifactId>`
   >
   > `</dependency>`
   >
   > e
   >
   > `<dependency>`
   >
   >   `<groupId>com.adobe.aem</groupId>`
   >   `<artifactId>core-forms-components-core</artifactId>`
   >
   > `</dependency>`


1. Salva e chiudi il file 

## 3. Aggiorna il progetto per includere la versione più recente dei Componenti core di Forms:

1. Apri [Cartella progetto Archetipo AEM]/pom.xml per la modifica.


1. Salva e chiudi il file 

## 4. Eseguire il commit degli aggiornamenti nel repository Git ed eseguire la pipeline per distribuire il repository {#Commit-the-updates-to-your-git-repository}

1. Per eseguire il commit del codice nell’archivio Git:
   1. Aprire il terminale o il prompt dei comandi.
   1. Accedi al tuo `[AEM Repository Folder]` ed esegui i seguenti comandi nell’ordine elencato

      ```Shell
      git add pom.xml
      git add all/pom.xml
      git add ui.apps/pom.xml
      git commit -m "Added dependencies for Adaptive Forms Core Components"
      git push origin
      ```

1. Dopo il commit dei file nell’archivio Git, [Eseguire la pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html?lang=it).

   Una volta completata l’esecuzione della pipeline, i componenti core Adaptive Forms vengono abilitati per l’ambiente corrispondente. All’ambiente Forms as a Cloud Service vengono inoltre aggiunti un modello Forms adattivo (Componenti core) e un tema Canvas 3.0, che offrono opzioni per personalizzare e creare componenti core basati su Adaptive Forms.


## Domande frequenti {#faq}

### Cosa sono i Componenti core? {#core-components}

Il [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it) sono un set di componenti WCM (Web Content Management) standardizzati per l’AEM che consentono di velocizzare i tempi di sviluppo e ridurre i costi di manutenzione dei siti web.

### Quali sono tutte le funzionalità aggiunte all’abilitazione dei componenti core? {#core-components-capabilities}

Quando i componenti core Adaptive Forms sono abilitati per il tuo ambiente, all’ambiente vengono aggiunti un modello di modulo adattivo basato su Componenti core vuoto e un tema Canvas 3.0. Dopo aver abilitato i componenti core Forms adattivi per il tuo ambiente, puoi:

* Creazione di componenti core basati su Adaptive Forms.
* Creare modelli di moduli adattivi basati su Componenti core.
* Crea temi personalizzati per i modelli di moduli adattivi basati su Componenti core.
* Distribuisci le rappresentazioni JSON del modulo adattivo basato su componenti core a canali quali dispositivi mobili, web, app native e servizi che richiedono la rappresentazione headless di un modulo.

### I componenti core Forms adattivi sono abilitati per il mio ambiente? {#enable-components}

Per verificare che i componenti core Adaptive Forms siano abilitati per il tuo ambiente:

1. [Clonare l’archivio as a Cloud Service di AEM Forms](#1-clone-your-aem-forms-as-a-cloud-service-git-repository).

1. Apri `[AEM Repository Folder]/all/pom.xml` del tuo archivio Git di Cloud Service AEM Forms.

1. Cerca le dipendenze seguenti:

   * core-forms-components-af-core
   * core-forms-components-core
   * core-forms-components-apps
   * core-forms-components-af-apps
   * core-forms-components-examples-apps
   * core-forms-components-examples-content


   ![individua l’artefatto core-forms-components-af-core in all/pom.xml](/help/assets/enable-headless-adaptive-forms-on-aem-forms-cloud-service-locate-core-af-artifact.png)

   Se le dipendenze esistono, i componenti core Adaptive Forms sono abilitati per il tuo ambiente.
