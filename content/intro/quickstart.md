---
linkTitle: त्वरित प्रारंभ
title: Brigade(ब्रिगेड) के साथ त्वरित प्रारंभ
description: Brigade(ब्रिगेड) के साथ त्वरित प्रारंभ
section: परिचय
weight: 3
aliases:
  - /quickstart.md
  - /intro/quickstart.md
---

यह त्वरित प्रारंभ Brigade(ब्रिगेड) का व्यापक परिचय प्रस्तुत करता है. आप ब्रिगेड को डिफ़ॉल्ट कॉन्फिगरेशन के साथ लोकल, डेवलपमेंट-ग्रेड cluster(क्लस्टर) पे इंस्टाल करेंगे, प्रोजेक्ट और इवेंट बनानांगे, ब्रिगेड को इवेंट हैंडल करते हुए देखेंगे, और फिर वाह चिज रिमूव करेंगे जो आपको नहीं चाहिए।

अगर आपको वीडियो देखें सीखना पसंद है, तो यहां क्लिक करें
[video adaptation](https://www.youtube.com/watch?v=VFyvYOjm6zc). याह हमारे YouTube चैनल पर है |

* [पहली आवश्यकता](#prerequisites)
* [इंस्टॉल Brigade(ब्रिगेड)](#install-Brigade(ब्रिगेड))
  * [Brigade(ब्रिगेड) CLI(सीएलआई) की इंस्टालेशन](#install-the-Brigade(ब्रिगेड)-CLI(सीएलआई))
  * [Server-Side Components की इंस्टालेशन](#install-server-side-components)
  * [पोर्ट फॉरवार्डिंग](#port-forwarding)
* [परीक्षा लेना](#trying-it-out)
  * [Brigade(ब्रिगेड) में लॉगिन](#log-into-Brigade(ब्रिगेड))
  * [प्रोजेक्ट बनाना](#create-a-project)
  * [इवेंट बनाने के लिए](#create-an-event)
* [सफ़ाई](#cleanup)
* [अगले कदम](#next-steps)
* [समस्या निवारण](#troubleshooting)

## आवश्यकताएँ

> ⚠️ इस गाइड में जो डिफ़ॉल्ट कॉन्फ़िगरेशन है, वो केवल Brigade(ब्रिगेड) को लोकल, 
> डेवलपमेंट-ग्रेड cluster(क्लस्टर) पर देखने के लिए है, ना की कोई _any_ shared 
> cluster(क्लस्टर) के लिए-- _विशेष रूप से प्रोडक्शन वाले क्लस्टर्स के लिए_।
> हमारा [डेवलपमेंट गाइड](/topics/operators/deploy/) देखे shared या 
>production cluster(क्लस्टर)s के लिए। हमने ये निर्देश स्थानीय cluster(क्लस्टर) पे टेस्ट 
>किया है।
> [KinD](https://kind.sigs.k8s.io/)

* एक local, _development-grade_ Kubernetes v1.16.0+ cluster(क्लस्टर)
* [Helm v3.7.0+](https://helm.sh/docs/intro/install/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* खाली डिस्क स्पेस। स्थापना के लिए पर्याप्त खाली डिस्क स्थान की आवश्यकता होती है|
   यदि आपकी डिस्क लगभग भर चुकी है, तो विफल हो जाएगा।

## इंस्टॉल Brigade(ब्रिगेड)

### Brigade(ब्रिगेड) CLI(सीएलआई) की इंस्टालेशन

Brigade(ब्रिगेड) CLI(सीएलआई), `brig`,को इंस्टाल करने के लिए उपयुक्त प्री-बिल्ट बाइनरी को डाउनलोड करे हमारे [रिलीज पेज](https://github.com/brigadecore/brigade/releases) से आपके मशीन के एक डायरेक्टरी पर जो आपके `PATH` एनवायरनमेंट वेरिएबल्स से जुड़ा है। कुछ सिस्टम पर, ये इससे भी आसान है। नीचे दिए गए कुछ निर्देश है जान पहचान एनवायरनमेंट के लिए:

**Linux(लिनक्स)**

```shell
$ curl -Lo /usr/local/bin/brig https://github.com/brigadecore/brigade/releases/download/v2.3.1/brig-linux-amd64
$ chmod +x /usr/local/bin/brig
```

**MacOS(मैक ओएस)**

प्रसिद्ध [Homebrew(होमब्रू)](https://brew.sh/) पैकेज मैनेजर सबसे सुविधाजनक विधि प्रदान करता है जिसे Brigade(ब्रिगेड) CLI(सीएलआई) Mac पर स्थापित हो सके:

```shell
$ brew install brigade-cli
```

वैकल्पिक रूप से आप सीधा खुद से प्री बिल्ट बाइनरी डाउनलोड कर सकते हैं:

```shell
$ curl -Lo /usr/local/bin/brig https://github.com/brigadecore/brigade/releases/download/v2.3.1/brig-darwin-amd64
$ chmod +x /usr/local/bin/brig
```

**Windows(विंडोज़)**

```powershell
> mkdir -force $env:USERPROFILE\bin
> (New-Object Net.WebClient).DownloadFile("https://github.com/brigadecore/brigade/releases/download/v2.3.1/brig-windows-amd64.exe", "$ENV:USERPROFILE\bin\brig.exe")
> $env:PATH+=";$env:USERPROFILE\bin"
```
ऊपर दी गई स्क्रिप्ट `brig.exe` डाउनलोड करती है और इसे वर्तमान सत्र के लिए आपके `PATH` में जोड़ती है। निम्नलिखित पंक्ति को अपने [पावरशेल प्रोफाइल](https://www.howtogeek.com/126469/how-to-create-a-powershell-profile/) में जोड़ें।

यदि आप परिवर्तन को स्थायी बनाना चाहते हैं:


```powershell
> $env:PATH+=";$env:USERPROFILE\bin"
```

### Server-Side Components(सर्वर-साइड कंपोनेंट्स) की इंस्टालेशन

अपने स्थानीय, डेवलपमेंट-ग्रेड cluster(क्लस्टर) पर सर्वर-साइड कंपोनेंट्स को स्थापित करने के लिए:

1. हेल्म का प्रयोगात्मक OCI(ओसीआई) समर्थन सक्षम करें:

    **POSIX(पॉज़िक्स)**
    ```shell
    $ export HELM_EXPERIMENTAL_OCI=1
    ```

    **PowerShell(पावरशेल)**
    ```powershell
    > $env:HELM_EXPERIMENTAL_OCI=1
    ```

1.Brigade(ब्रिगेड) को डिफ़ॉल्ट कॉन्फ़िगरेशन के साथ स्थापित करने के लिए निम्नलिखित कमांड चलाएँ:

    ```shell
    $ helm install brigade \
        oci://ghcr.io/brigadecore/brigade \
        --version v2.3.1 \
        --create-namespace \
        --namespace brigade \
        --wait \
        --timeout 300s
    ```

    > ⚠️ इंस्टालेशन और आरंभिक स्टार्टअप को पूरा होने में कुछ मिनट लग सकते हैं।

    अगर डिप्लॉयमेंट विफल होता है, [समस्या निवारण](#troubleshooting) अनुभाग पर जाए|

### पोर्ट फॉरवार्डिंग

चूंकि आप Brigade(ब्रिगेड) को स्थानीय रूप से चला रहे हैं, इसलिए Brigade(ब्रिगेड) API(एपीआई) उपलब्ध बनाने के लिए पोर्ट फ़ॉरवर्डिंग का उपयोग करें स्थानीय नेटवर्क इंटरफेस के माध्यम से: 

**POSIX**
```shell
$ kubectl --namespace brigade port-forward service/brigade-apiserver 8443:443 &>/dev/null &
```

**PowerShell**
```powershell
> kubectl --namespace brigade port-forward service/brigade-apiserver 8443:443 *> $null  
```

## परीक्षा लेना

### Brigade(ब्रिगेड) में लॉगिन

Brigade(ब्रिगेड) को रूट यूजर के तौर पर प्रमाणित करने के लिए आपको auto-generated(स्व - उत्पन्न) रूट यूजर पासवर्ड हैसिल करनी होगी:

**POSIX**
```shell
$ export APISERVER_ROOT_PASSWORD=$(kubectl get secret --namespace brigade brigade-apiserver --output jsonpath='{.data.root-user-password}' | base64 --decode)
```

**PowerShell**
```powershell
> $env:APISERVER_ROOT_PASSWORD=$(kubectl get secret --namespace brigade brigade-apiserver --output jsonpath='{.data.root-user-password}' | base64 --decode)
```

फिर:

**POSIX**
```shell
$ brig login --insecure --server https://localhost:8443 --root --password "${APISERVER_ROOT_PASSWORD}"
```

**PowerShell**
```powershell
> brig login --insecure --server https://localhost:8443 --root --password "$env:APISERVER_ROOT_PASSWORD"
```

`--insecure` ध्वज `brig login` को स्व-हस्ताक्षरित प्रमाण पत्र को अनदेखा करने का निर्देश देता है जो
 हमारे Brigade(ब्रिगेड) की स्थानीय स्थापना द्वारा उपयोग किया गया है।

यदि `brig login` कमांड हैंग हो जाता है या विफल हो जाता है, तो पोर्ट-फ़ॉरवर्डिंग को दोबारा जांचें
`brigade-apiserver` सेवा के लिए पिछले में सफलतापूर्वक पूरा किया गया अनुभाग।

### प्रोजेक्ट बनाना

एक Brigade(ब्रिगेड) [प्रोजेक्ट] (/topics/project-developers/projects) वर्कर (ईवेंट हैंडलर) कॉन्फ़िगरेशन के साथ ईवेंट सब्स्क्रिप्शंस कॉन्फ़िगरेशन भी रखता है|

1. प्रारंभ से एक प्रोजेक्ट परिभाषा बनाने के बजाय, हम  प्रक्रिया को `brig init` कमांड से आगे लेके जाएंगे:

    ```shell
    $ mkdir first-project
    $ cd first-project
    $ brig init --id first-project
    ```

    यह एक प्रोजेक्ट परीभाषा बना देगा जो `.brigade/project.yaml` से काफ़ी मिलता है|
    यह `exec` इवेंट्स को सबस्क्राइब करता है `brigade.sh/cli` नामक एक सोर्स से एमिट कार्ति है| (इस प्रकार की इवेंट आसानी से बनाया जाता है CLI(सीएलआई) का उपयोग करके, इसलिए यह डेमो उद्देश्यों के लिए बहुत अच्छा है।) जब ऐसा कोई इवेंट हो, तब एम्बेडेड स्क्रिप्ट एक्जीक्यूट होता है। स्क्रिप्ट खुद ही खंडित होता है इवेंट के प्रकार के हिसाब से| `exec` इवेंट के लिए `brigade.sh/cli` नाम सोर्स से, ये स्क्रिप्ट एक सिंपल "Hello
    World!" एक्जीक्यूट कर देगा। किसी अन्य प्रकार के इवेंट के लिए, यह स्क्रिप्ट कुछ नहीं करेगी।

    ```yaml
    apiVersion: brigade.sh/v2
    kind: Project
    metadata:
      id: first-project
    description: My new Brigade project
    spec:
      eventSubscriptions:
        - source: brigade.sh/cli
          types:
            - exec
    workerTemplate:
      logLevel: DEBUG
      defaultConfigFiles:
      brigade.ts: |
        import { events, Job } from "@brigadecore/brigadier"
        
        // Use events.on() to define how your script responds to different events. 
        // The example below depicts handling of "exec" events originating from
        // the Brigade CLI.
        
        events.on("brigade.sh/cli", "exec", async event => {
            let job = new Job("hello", "debian:latest", event)
            job.primaryContainer.command = ["echo"]
            job.primaryContainer.arguments = ["Hello, World!"]
            await job.run()
        })

        events.process()
    ```

1. पिछली कमांड ने केवल एक टेम्पलेट से एक प्रोजेक्ट परिभाषा उत्पन्न की। हमे
    परियोजना निर्माण को पूरा करने के लिए अभी भी Brigade(ब्रिगेड) को इस परिभाषा को अपलोड करने की आवश्यकता है:

    ```shell
    $ brig project create --file .brigade/project.yaml
    ```

1. यह देखने के लिए कि Brigade(ब्रिगेड) को अब इस प्रोजेक्ट के बारे में पता है, `brig project list` का उपयोग करें:


    ```shell
    $ brig project list

    ID           	DESCRIPTION                         	AGE
    first-project	My new Brigade project               	1m
    ```

### इवेंट बनाने के लिए

हमारी परियोजना परिभाषित होने के साथ, अब हम मैन्युअल रूप से एक ईवेंट बनाने और देखने के लिए तैयार हैं
Brigade(ब्रिगेड) इसे संभालती है:

```shell
$ brig event create --project first-project --follow
```

नीचे उदाहरण आउटपुट है:

```plain
Created event "2cb85062-f964-454d-ac5c-526cdbdd2679".

Waiting for event's worker to be RUNNING...
2021-08-10T16:52:01.699Z INFO: brigade-worker version: v2.3.1
2021-08-10T16:52:01.701Z DEBUG: writing default brigade.ts to /var/vcs/.brigade/brigade.ts
2021-08-10T16:52:01.702Z DEBUG: using npm as the package manager
2021-08-10T16:52:01.702Z DEBUG: path /var/vcs/.brigade/node_modules/@brigadecore does not exist; creating it
2021-08-10T16:52:01.702Z DEBUG: polyfilling @brigadecore/brigadier with /var/brigade-worker/brigadier-polyfill
2021-08-10T16:52:01.703Z DEBUG: compiling brigade.ts with flags --target ES6 --module commonjs --esModuleInterop
2021-08-10T16:52:04.210Z DEBUG: running node brigade.js
2021-08-10T16:52:04.360Z [job: hello] INFO: Creating job hello
2021-08-10T16:52:06.921Z [job: hello] DEBUG: Current job phase is SUCCEEDED
```


> ⚠️ डिफ़ॉल्ट रूप से, ब्रिगेड के scheduler(अनुसूचक) नए प्रोजेक्ट के लाई स्कैन करता है प्रति तीस सेकंड में।
> अगर Brigade(ब्रिगेड) आपके पहले इवेंट को हैंडल करने में धीमी है, तो यह इसकी वजह हो सकती है।

## सफ़ाई

अगर आपको आपकी Brigade(ब्रिगेड) की स्थापना रखना है, तो यह कमांड रन करे तकी जो उदाहरण प्रोजेक्ट अभी बनाया गया है, वह हट जाए:

```shell
$ brig project delete --id first-project
```

नहीं तो, आप _all_ resources (सभी संसाधन) जो अभी बनायी गई है, वो हटा सकता है:

```shell
$ helm delete brigade -n brigade
```

## अगले कदम

अभी आपको पता है की Brigade(ब्रिगेड) को लोकल, डेवलपमेंट-ग्रेड cluster(क्लस्टर) में कैसे इंस्टॉल करते हैं, कैसे प्रोजेक्ट डिफाइन करते है और कैसे एक मैन्युअल इवेंट बनाते हैं।
जारी रहे
[आगे पढ़ें](/intro/readnext) दस्तावेज़ जहां हम अधिक उन्नत विषयों की खोज करने की सुझाव देते हैं। 

## समस्या निवारण

* [इंस्टालेशन सफलतापूर्वक पूरा नहीं हुआ](#installation-does-not-complete-successfully)
* [लॉगिन कमांड हैंग हो जाता है](#login-command-hangs)

### इंस्टालेशन सफलतापूर्वक पूरा नहीं हुआ

cluster(क्लस्टर) node(क्लस्टर नोड) पे डिस्क स्पेस कम होना एक सामान्य वजह है Brigade(ब्रिगेड) की डिप्लॉयमेंट के असफल होने का। MacOS(मैक ओएस) या Windows(विंडोज़) पर स्थानीय, विकास-ग्रेड cluster(क्लस्टर) में, यह हो सकता है
क्योंकि अपर्याप्त डिस्क स्थान Docker Desktop(डॉकर डेस्कटॉप) को आवंटित किया गया है या
आवंटन स्थान लगभग पूरा हो चुका है।अगर ये मामला है, तो यह Brigade(ब्रिगेड) के MongoDB(मोंगोडीबी) या ActiveMQ Artemis pods(एक्टिव एमक्यू आर्टेमिस पॉड्स) के logs(लॉग्स) को जांचकर स्पश्त हो जाएगा।
अगर logs(लॉग्स) में ऐसा कुछ संदेश शमील है- "No space left on device(डिवाइस पर कोई स्थान नहीं बचा है)" या "Disk Full!(डिस्क पूर्ण!)",तो आपको डिस्क स्पेस को फ्री करके फिर से इंस्टालेशन कोशिश करना होगा|
 `docker system prune` को चलाना एक रास्ता है डिस्क स्पेस को रिकवर करने का। 

डिस्क स्पेस फ्री करने के बाद, खराब इंस्टालेशन को रिमूव करे और निमंलिखित कमांड्स डालने के बाद कोशिश करे:

```shell
$ helm uninstall brigade -n brigade
$ helm install brigade \
    oci://ghcr.io/brigadecore/brigade \
    --version v2.3.1 \
    --namespace brigade \
    --wait \
    --timeout 300s
```

### लॉगिन कमांड हैंग हो जाता है

अगर `brig login` कमांड हैंग हो जाए, तो आप चेक करे की आपने `--insecure` (या `-k`) फ्लैग शामिल किया है। 
यदि `brig login` कमांड हैंग हो जाता है, तो जांच लें कि आपने `--insecure` शामिल किया है (या
`-k`)फ़्लैग्। यह फ्लैग ज़रुरी है क्यों की डिफ़ॉल्ट कॉन्फ़िगरेशन जो ईस क्विकस्टार्ट/त्वरित प्रारंभ में उपयोग किया गया समलैंगिक है, वो स्वयं हस्ताक्षरित प्रमाणपत्र उपयोग करता है।