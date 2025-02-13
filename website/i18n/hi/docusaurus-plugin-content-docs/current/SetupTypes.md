---
id: setuptypes
title: सेटअप प्रकार
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

WebdriverIO का उपयोग विभिन्न उद्देश्यों के लिए किया जा सकता है। यह वेबड्राइवर प्रोटोकॉल एपीआई को लागू करता है और स्वचालित तरीके से ब्राउज़र चला सकता है। ढांचे को किसी भी मनमाने वातावरण में और किसी भी प्रकार के कार्य के लिए डिज़ाइन किया गया है। यह किसी भी तीसरे पक्ष के ढांचे से स्वतंत्र है और इसे चलाने के लिए केवल Node.js की आवश्यकता होती है।

## प्रोटोकॉल बाइंडिंग

WebDriver और अन्य ऑटोमेशन प्रोटोकॉल के साथ बुनियादी इंटरैक्शन के लिए WebdriverIO [`webdriver`](https://www.npmjs.com/package/webdriver) NPM पैकेज के आधार पर अपने स्वयं के प्रोटोकॉल बाइंडिंग का उपयोग करता है:

<Tabs
  defaultValue="webdriver"
  values={[
    {label: 'WebDriver', value: 'webdriver'},
 {label: 'Chrome DevTools', value: 'devtools'},
 ]
}>
<TabItem value="webdriver">

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/setup/webdriver.js#L5-L20
```

</TabItem>
<TabItem value="devtools">

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/setup/devtools.js#L2-L17
```

</TabItem>
</Tabs>

सभी [प्रोटोकॉल कमांड](./api/_webdriver.md) ऑटोमेशन ड्राइवर से अपरिष्कृत प्रतिक्रिया लौटाते हैं। पैकेज बहुत हल्का है और प्रोटोकॉल उपयोग के साथ बातचीत को आसान बनाने के लिए ऑटो-वेट जैसे __नंबर__ स्मार्ट लॉजिक है।

उदाहरण के लिए लागू प्रोटोकॉल आदेश ड्राइवर के प्रारंभिक सत्र की प्रतिक्रिया पर निर्भर करते हैं। उदाहरण के लिए यदि प्रतिक्रिया इंगित करती है कि एक मोबाइल सत्र शुरू किया गया था, तो पैकेज इंस्टेंस प्रोटोटाइप के लिए सभी एपियम और मोबाइल JSON वायर प्रोटोकॉल कमांड लागू करता है।

[`devtools`](https://www.npmjs.com/package/devtools) NPM पैकेज आयात करते समय आप Chrome DevTools प्रोटोकॉल का उपयोग करके कमांड का एक ही सेट (मोबाइल वाले को छोड़कर) चला सकते हैं। इसमें `webdriver` पैकेज के समान इंटरफ़ेस है लेकिन [Puppeteer](https://pptr.dev/)पर आधारित अपना स्वचालन चलाता है।

इन पैकेज इंटरफेस पर अधिक जानकारी के लिए, [मॉड्यूल एपीआई](/docs/api/modules)देखें।

## स्टैंडअलोन मोड

वेबड्राइवर प्रोटोकॉल के साथ बातचीत को सरल बनाने के लिए `webdriverio` पैकेज प्रोटोकॉल के शीर्ष पर विभिन्न प्रकार के कमांड लागू करता है (उदाहरण के लिए [`dragAndDrop`](./api/element/_dragAndDrop.md) कमांड) और कोर अवधारणाएं जैसे [स्मार्ट चयनकर्ता](./Selectors.md) या [ऑटो-वेट](./AutoWait.md)। ऊपर से उदाहरण इस तरह सरल किया जा सकता है:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/setup/standalone.js#L2-L19
```

स्टैंडअलोन मोड में WebdriverIO का उपयोग करना अभी भी आपको सभी प्रोटोकॉल कमांड तक पहुंच प्रदान करता है लेकिन अतिरिक्त कमांड का एक सुपर सेट प्रदान करता है जो ब्राउज़र के साथ उच्च स्तर की सहभागिता प्रदान करता है। यह आपको एक नई ऑटोमेशन लाइब्रेरी बनाने के लिए इस ऑटोमेशन टूल को अपने (परीक्षण) प्रोजेक्ट में एकीकृत करने की अनुमति देता है। लोकप्रिय उदाहरणों में शामिल हैं [Spectron](https://www.electronjs.org/spectron) या [CodeceptJS](http://codecept.io)। आप सामग्री के लिए वेब को परिमार्जन करने के लिए सादा नोड स्क्रिप्ट भी लिख सकते हैं (या कुछ और जिसके लिए एक चल रहे ब्राउज़र की आवश्यकता होती है)।

यदि कोई विशिष्ट विकल्प सेट नहीं किया गया है तो WebdriverIO `http://localhost:4444/` पर एक ब्राउज़र ड्राइवर खोजने का प्रयास करेगा और स्वचालित रूप से Chrome DevTools प्रोटोकॉल और Puppeteer को ऑटोमेशन इंजन के रूप में स्विच करता है यदि ऐसा ड्राइवर नहीं मिल पाता है। यदि आप वेबड्राइवर के आधार पर चलाना पसंद करते हैं तो आपको या तो उस ड्राइवर को मैन्युअल रूप से या स्क्रिप्ट या [एनपीएम पैकेज](https://www.npmjs.com/package/chromedriver)के माध्यम से शुरू करना होगा।

`webdriverio` पैकेज इंटरफेस पर अधिक जानकारी के लिए, [मॉड्यूल API](/docs/api/modules)देखें।

## WDIO टेस्टरनर

हालाँकि, WebdriverIO का मुख्य उद्देश्य बड़े पैमाने पर एंड-टू-एंड परीक्षण है। इसलिए हमने एक परीक्षण धावक लागू किया है जो आपको एक विश्वसनीय परीक्षण सूट बनाने में मदद करता है जो पढ़ने और बनाए रखने में आसान है।

टेस्ट रनर कई समस्याओं का ध्यान रखता है जो प्लेन ऑटोमेशन लाइब्रेरी के साथ काम करते समय आम हैं। एक के लिए, यह आपके टेस्ट रन को व्यवस्थित करता है और टेस्ट स्पेक्स को विभाजित करता है ताकि आपके टेस्ट को अधिकतम समवर्ती के साथ निष्पादित किया जा सके। यह सत्र प्रबंधन को भी संभालता है और समस्याओं को डीबग करने और अपने परीक्षणों में त्रुटियां खोजने में आपकी मदद करने के लिए बहुत सारी सुविधाएँ प्रदान करता है।

यहाँ ऊपर से एक ही उदाहरण दिया गया है, जिसे परीक्षण युक्ति के रूप में लिखा गया है और WDIO द्वारा निष्पादित किया गया है:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/setup/testrunner.js
```

टेस्ट रनर मोचा, जैस्मीन या खीरा जैसे लोकप्रिय टेस्ट फ्रेमवर्क का एक सार है। WDIO परीक्षण रनर का उपयोग करके अपने परीक्षण चलाने के लिए, अधिक जानकारी के लिए [प्रारंभ करना](GettingStarted.md) अनुभाग देखें।

`@wdio/cli` टेस्टरनर पैकेज इंटरफ़ेस पर अधिक जानकारी के लिए, [मॉड्यूल API](/docs/api/modules)देखें।
