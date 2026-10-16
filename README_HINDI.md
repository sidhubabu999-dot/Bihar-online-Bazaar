# Bihar Online Bazaar — मोबाइल से सेटअप (₹0 शुरुआत)

यह एक शुरुआती मल्टी-वेंडर मार्केटप्लेस प्रोजेक्ट है। इसमें ग्राहक signup/login, प्रोफाइल/पता, सामान सूची, कार्ट, ऑर्डर अनुरोध, दुकानदार पैनल, फोटो अपलोड, admin पैनल और WhatsApp पर नए ऑर्डर की सूचना का आधार शामिल है। **यह फाइल अभी अपने-आप लाइव नहीं है**; पहले Supabase सेटअप और उसके बाद hosting जरूरी है।

## फाइलें
- `index.html` — वेबसाइट का पेज
- `style.css` — मोबाइल फ्रेंडली डिजाइन
- `app.js` — लॉगिन, सामान, कार्ट और ऑर्डर लॉजिक
- `supabase_setup.sql` — डेटाबेस, RLS और फोटो स्टोरेज का शुरुआती SQL

## भाग 1: Supabase backend बनाएं
1. Chrome में `https://supabase.com/` खोलें और अकाउंट बनाएं/लॉगिन करें।
2. नया project बनाएं। मजबूत Database Password रखें और सुरक्षित जगह लिख लें। Free plan चुनें यदि उपलब्ध हो।
3. Project बनने के बाद `SQL Editor` खोलें → `New query` दबाएं।
4. इस ZIP में `supabase_setup.sql` खोलें, पूरा टेक्स्ट कॉपी करें, SQL Editor में पेस्ट करें और `Run` दबाएं। अगर कोई error आए तो साइट live करने से पहले error ठीक कराएं।
5. Project Settings → API (या Connect) में Project URL और anon/publishable key ढूंढें। **service_role/secret key कभी वेबसाइट में न डालें।**
6. `app.js` खोलें और ऊपर की दो लाइनें बदलें:
   - `PASTE_YOUR_SUPABASE_PROJECT_URL_HERE` की जगह अपना Project URL
   - `PASTE_YOUR_SUPABASE_ANON_KEY_HERE` की जगह anon/publishable key
7. Supabase Authentication → URL Configuration में बाद में अपनी live website URL को Site URL / Redirect URLs में जोड़ें। शुरुआत में email confirmation सेटिंग के अनुसार user को ईमेल confirm करना पड़ सकता है।

## भाग 2: Admin बनाएं
1. वेबसाइट पर ग्राहक अकाउंट बनाएं और उसी ईमेल से Supabase Dashboard → Authentication → Users में confirm करें।
2. SQL Editor में यह कमांड चलाएं; `YOUR_EMAIL` की जगह अपना असली ईमेल लिखें:

```sql
update public.profiles
set role='admin', is_approved=true
where id=(select id from auth.users where email='YOUR_EMAIL');
```

3. लॉगआउट करके दोबारा लॉगिन करें। Admin dashboard दिखना चाहिए। किसी दूसरे व्यक्ति को admin role न दें।
4. दुकानदार अपने लिए Vendor अकाउंट बनाएगा। Admin dashboard → दुकानदार देखें → मंजूर करें। उसके बाद ही दुकानदार सामान जोड़ पाएगा।

## भाग 3: मुफ्त hosting
1. अपने मोबाइल में project की सभी फाइलें एक ही folder में रखें।
2. Netlify या किसी static hosting provider पर अकाउंट बनाकर इस folder को deploy करें। `index.html`, `style.css`, `app.js` एक ही folder में रहने चाहिए।
3. अगर hosting provider ZIP upload मांगता है तो ZIP के अंदर सीधे ये फाइलें हों; ZIP के अंदर अतिरिक्त outer folder होने पर deploy तरीका अलग हो सकता है।
4. मिली हुई website URL को Supabase Authentication → URL Configuration में जोड़ें।

## परीक्षण जरूरी
- पहले खुद का test customer और test vendor बनाएं।
- Admin से vendor approve करें, test product upload करें, customer login से cart/order जांचें।
- WhatsApp order notification आपके नंबर `7321828644` पर खुलती है। WhatsApp संदेश अपने-आप भेजा नहीं जाता; ग्राहक/एडमिन को WhatsApp में Send दबाना होगा।
- वास्तविक उपयोग से पहले RLS policies और SQL की समीक्षा/परीक्षा कराएं। यह starter prototype है, production-grade audited marketplace नहीं।

## सुरक्षा और सीमाएं
- किसी भी ग्राहक का password वेबसाइट/admin को दिखाई नहीं देता; authentication Supabase संभालता है।
- फोन, पता और ऑर्डर व्यक्तिगत डेटा हैं; केवल जरूरत भर का डेटा रखें और सार्वजनिक रूप से न दिखाएं।
- यह starter browser से order total बनाता है। ऑनलाइन भुगतान/पैसा लेने से पहले server-side price/stock validation, atomic stock updates, delivery fee rules, refunds, rate limiting, logging और privacy/terms pages जोड़ना जरूरी है। अभी online payment integration शामिल नहीं है।
- ग्राहक/दुकानदार के signup पर ईमेल/फोन confirmation और vendor approval सेट करें।
- Supabase free limits, hosting limits और provider के नियम बदल सकते हैं। Free tier की उपलब्धता/सीमाएं अपने account में जांचें।
- ₹0 का मतलब कोई upfront software subscription न चुनना है; इंटरनेट, डिलीवरी, पैकिंग और व्यवसाय के दूसरे खर्च अलग हो सकते हैं।
