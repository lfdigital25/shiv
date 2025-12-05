<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Shiv Milk Dairy & Traders</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&family=Noto+Sans+Devanagari:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- React & ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    
    <!-- Babel for JSX compilation in browser -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
      body {
        font-family: 'Poppins', 'Noto Sans Devanagari', sans-serif;
      }
      /* Custom scrollbar for modals */
      .custom-scrollbar::-webkit-scrollbar {
        width: 8px;
      }
      .custom-scrollbar::-webkit-scrollbar-track {
        background: #f1f1f1;
        border-radius: 4px;
      }
      .custom-scrollbar::-webkit-scrollbar-thumb {
        background: #888;
        border-radius: 4px;
      }
      .custom-scrollbar::-webkit-scrollbar-thumb:hover {
        background: #555;
      }
      @keyframes fadeIn {
        from { opacity: 0; transform: scale(0.95); }
        to { opacity: 1; transform: scale(1); }
      }
      .animate-fadeIn {
        animation: fadeIn 0.2s ease-out forwards;
      }
    </style>
  <script type="importmap">
{
  "imports": {
    "react/": "https://aistudiocdn.com/react@^19.2.1/",
    "react": "https://aistudiocdn.com/react@^19.2.1",
    "react-dom/": "https://aistudiocdn.com/react-dom@^19.2.1/",
    "lucide-react": "https://aistudiocdn.com/lucide-react@^0.556.0"
  }
}
</script>
</head>
  <body class="bg-slate-50 text-slate-900">
    <div id="root"></div>

    <script type="text/babel" data-presets="typescript,react">
      const { useState, useEffect, useMemo } = React;

      // --- ICONS (Inline SVGs to replace Lucide-React dependency) ---
      const IconBase = ({ children, size = 24, className = "" }) => (
        <svg 
          xmlns="http://www.w3.org/2000/svg" 
          width={size} 
          height={size} 
          viewBox="0 0 24 24" 
          fill="none" 
          stroke="currentColor" 
          strokeWidth="2" 
          strokeLinecap="round" 
          strokeLinejoin="round" 
          className={className}
        >
          {children}
        </svg>
      );

      const Icons = {
        Phone: (props) => <IconBase {...props}><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></IconBase>,
        MapPin: (props) => <IconBase {...props}><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></IconBase>,
        Award: (props) => <IconBase {...props}><circle cx="12" cy="8" r="7"></circle><polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"></polyline></IconBase>,
        LogIn: (props) => <IconBase {...props}><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"></path><polyline points="10 17 15 12 10 7"></polyline><line x1="15" y1="12" x2="3" y2="12"></line></IconBase>,
        Clock: (props) => <IconBase {...props}><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></IconBase>,
        Truck: (props) => <IconBase {...props}><rect x="1" y="3" width="15" height="13"></rect><polygon points="16 8 20 8 23 11 23 16 16 16 16 8"></polygon><circle cx="5.5" cy="18.5" r="2.5"></circle><circle cx="18.5" cy="18.5" r="2.5"></circle></IconBase>,
        Syringe: (props) => <IconBase {...props}><path d="m18 2 4 4"></path><path d="m17 7 3-3"></path><path d="M19 9 8.7 19.3c-1 1-2.5 1-3.4 0l-.6-.6c-1-1-1-2.5 0-3.4L15 5"></path><path d="m9 11 4 4"></path><path d="m5 19-3 3"></path><path d="m14 4 6 6"></path></IconBase>,
        Activity: (props) => <IconBase {...props}><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline></IconBase>,
        X: (props) => <IconBase {...props}><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></IconBase>,
        User: (props) => <IconBase {...props}><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></IconBase>,
        Droplet: (props) => <IconBase {...props}><path d="M12 2.69l5.66 5.66a8 8 0 1 1-11.31 0z"></path></IconBase>,
        Wallet: (props) => <IconBase {...props}><path d="M21 12V7a2 2 0 0 0-2-2H5a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2v-5Zm-6 0h6"></path></IconBase>,
        Calendar: (props) => <IconBase {...props}><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></IconBase>,
        MessageCircle: (props) => <IconBase {...props}><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"></path></IconBase>,
        Check: (props) => <IconBase {...props}><polyline points="20 6 9 17 4 12"></polyline></IconBase>
      };

      // --- CONSTANTS & DATA ---
      const PHONE_NUMBER = "+919919747247";
      const ADDRESS = "BESIYA NIKAT SHIV MANDIR RAMNaGRA";

      const TRANSLATIONS = {
        header: {
          title: { en: "Shiv Milk Dairy & Traders", hi: "शिव मिल्क डेयरी एंड ट्रेडर्स" },
          greeting: { en: "🙏 Om Namah Shivay | Jai Mata Di 🙏", hi: "🙏 ओम नमः शिवाय | जय माता दी 🙏" },
          login: { en: "Member Login", hi: "सदस्य लॉगिन" },
          logout: { en: "Logout", hi: "लॉग आउट" },
          dashboard: { en: "Dashboard", hi: "डैशबोर्ड" }
        },
        hero: {
          welcome: { en: "Welcome to Shiv Milk Dairy", hi: "शिव मिल्क डेयरी में आपका स्वागत है" },
          subtitle: { en: "Pure Milk, Quality Products, Trusted Service", hi: "शुद्ध दूध, गुणवत्तापूर्ण उत्पाद, विश्वसनीय सेवा" }
        },
        stats: {
          bestSeller: { en: "🏆 Best Seller Farmer", hi: "🏆 सर्वश्रेष्ठ विक्रेता किसान" },
          topFarmer: { en: "Top Farmer", hi: "शीर्ष किसान" },
          quantity: { en: "Total Quantity", hi: "कुल मात्रा" },
          liters: { en: "Ltr", hi: "ली." }
        },
        banners: {
          bulkTitle: { en: "📢 Bulk Orders & Pre-Booking", hi: "📢 थोक ऑर्डर और प्री-बुकिंग" },
          bulkDesc: { en: "Special rates for Marriage, Parties, and Bulk Milk/Paneer orders. Pre-book to ensure availability!", hi: "शादी, पार्टियों और थोक दूध/पनीर ऑर्डर के लिए विशेष दरें। उपलब्धता सुनिश्चित करने के लिए पहले से बुक करें!" },
          preOrderBtn: { en: "Pre-Order Milk/Paneer", hi: "दूध/पनीर प्री-ऑर्डर करें" }
        },
        sections: {
          agro: { en: "Animal Nutrition & Agriculture", hi: "पशु पोषण और कृषि" },
          dairy: { en: "Dairy & Daily Essentials", hi: "डेयरी और दैनिक आवश्यकताएं" },
          health: { en: "Animal Health & Disease Control", hi: "पशु स्वास्थ्य और रोग नियंत्रण" },
          ai: { en: "Artificial Insemination (AI)", hi: "कृत्रिम गर्भाधान (AI)" }
        },
        products: {
          title: { en: "Our Products", hi: "हमारे उत्पाद" },
          orderNow: { en: "Order Now (WhatsApp)", hi: "अभी ऑर्डर करें (व्हाट्सएप)" },
          viewDetails: { en: "View Details", hi: "विवरण देखें" },
          selectOption: { en: "Select Option", hi: "विकल्प चुनें" }
        },
        dashboard: {
          title: { en: "Member Dashboard", hi: "सदस्य डैशबोर्ड" },
          memberId: { en: "Member ID", hi: "सदस्य आईडी" },
          earnings: { en: "Total Earnings", hi: "कुल कमाई" },
          lastCollection: { en: "Last Collection", hi: "अंतिम संग्रह" },
          loading: { en: "Loading...", hi: "लोड हो रहा है..." }
        },
        info: {
          collectionTimes: { en: "Milk Collection Times", hi: "दूध संग्रह का समय" },
          morning: { en: "Morning: 05:00 AM - 06:50 AM", hi: "सुबह: 05:00 AM - 06:50 AM" },
          evening: { en: "Evening: 05:00 PM - 07:00 PM", hi: "शाम: 05:00 PM - 07:00 PM" },
          contact: { en: "Contact Us", hi: "संपर्क करें" },
          vet: { en: "Veterinary & AI Services", hi: "पशु चिकित्सा और AI सेवाएं" },
        },
        health: {
          desc: { en: "Expert consultation for Foot & Mouth Disease (FMD), Mastitis, and seasonal cattle diseases.", hi: "खुरपका-मुंहपका (FMD), थनेला और मौसमी पशु रोगों के लिए विशेषज्ञ परामर्श।" }
        },
        ai: {
          desc: { en: "High-quality Artificial Insemination services available under expert doctor supervision for breed improvement.", hi: "नस्ल सुधार के लिए विशेषज्ञ डॉक्टर की देखरेख में उच्च गुणवत्ता वाली कृत्रिम गर्भाधान सेवाएं उपलब्ध हैं।" }
        }
      };

      const PRODUCTS = [
        {
          id: 'calcium-gold', category: 'agro',
          name: { en: "Shwetdhara Gold Calcium", hi: "श्वेतधारा गोल्ड कैल्शियम" },
          price: 150,
          variants: [
            { id: '1l', name: { en: "1 Litre", hi: "1 लीटर" }, price: 150 },
            { id: '5l', name: { en: "5 Litre", hi: "5 लीटर" }, price: 800 }
          ],
          description: { 
            en: "🌟 Boost Milk, Strengthen Health 🌟\n\nExperience the Real Milk Boost!\n\nProduct Description:\nShwetdhara Gold Calcium (Glycine Chelated) is a scientifically formulated supplement that helps increase milk production in livestock and improves their overall health.\n\nKey Benefits:\n💪 Strong Bones\n🔋 High Energy\n🥛 Increased Milk Yield\n✅ Supports Bone Health\n✅ Enhances Muscle Strength\n✅ Effective in Milk Production\n✅ Reduces Fatigue and Weakness\n\nNutrient-Rich:\nEnriched with 13+ essential nutrients: Calcium (Ca), Magnesium (Mg), Zinc (Zn), Iron (Fe), Vitamin A, D3, E. Suitable for all types of livestock – Cow, Buffalo, Goat.\n\nFor Farmers and Animals:\nHappy animals — Happy farmers — Double your income!\n🌾 Trust every drop of milk with Shwetdhara Gold!", 
            hi: "🌟 दूध बढ़ाएँ, स्वास्थ्य मजबूत करें 🌟\n\nअब आएगा असली दूध का धमाका!\n\nउत्पाद विवरण:\nश्वेतधारा गोल्ड कैल्शियम (Glycine Chelated) एक वैज्ञानिक रूप से तैयार किया गया सप्लीमेंट है, जो पशुओं में दूध उत्पादन बढ़ाने और उनकी समग्र सेहत सुधारने में मदद करता है।\n\nमुख्य लाभ:\n💪 ताकतवर हड्डियाँ\n🔋 ज़बरदस्त एनर्जी\n🥛 ज़्यादा दूध\n✅ हड्डियों को मजबूत बनाए\n✅ मांसपेशियों में ताकत बढ़ाए\n✅ दूध उत्पादन में असरदार\n✅ थकान और कमजोरी से राहत\n\nपोषक तत्वों से भरपूर:\n13+ आवश्यक पोषक तत्वों से समृद्ध: कैल्शियम (Ca), मैग्नीशियम (Mg), जिंक (Zn), आयरन (Fe), विटामिन A, D3, E। सभी प्रकार के पशुओं के लिए उपयुक्त – गाय, भैंस, बकरी।\n\nकिसान और पशु दोनों के लिए:\nपशु खुश — किसान खुश — आमदनी दोगुनी!\n🌾 हर बूंद में भरोसा — श्वेतधारा गोल्ड के साथ!" 
          },
          imageUrl: "https://picsum.photos/400/400?random=10"
        },
        {
          id: 'feed-50kg', category: 'agro',
          name: { en: "Shwetdhara Animal Feed (50kg)", hi: "श्वेतधारा पशु आहार (50 किग्रा)" },
          price: 1200,
          description: { 
            en: "🌾🐄 A Golden Opportunity for Farmers! 🐄🌾\nGet healthy livestock, more milk, and higher earnings with Shwetdhara Animal Feed!\nFor price, contact us.\n\n🟢 Shwetdhara Animal Feed – 50 kg Pack\n✔ Significant increase in milk production\n✔ Rich in energy, protein, and essential nutrients\n✔ BIS Certified – A Name You Can Trust\n\n🔹 Improved Breeding\n🔹 Healthy Calves\n🔹 High-Quality Milk\n\n🚜 Adopt Shwetdhara today – because only healthy animals make prosperous farmers!", 
            hi: "🌾🐄 किसानों के लिए सुनहरा मौका! 🐄🌾\nश्वेतधारा पशु आहार के साथ पाएं स्वस्थ पशु, ज़्यादा दूध और तगड़ी कमाई!\nमूल्य के लिए संपर्क करें।\n\n🟢 श्वेतधारा पशु आहार – 50 किलो पैक\n✔ दूध उत्पादन में उल्लेखनीय बढ़ोतरी\n✔ ऊर्जा, प्रोटीन और पोषक तत्वों से भरपूर\n✔ BIS प्रमाणित – भरोसे का नाम\n\n🔹 बेहतर प्रजनन\n🔹 स्वस्थ बछड़े\n🔹 उच्च गुणवत्ता वाला दूध\n\n🚜 आज ही अपनाएं श्वेतधारा – क्योंकि जब पशु होगा तंदुरुस्त, तभी किसान बनेगा समृद्ध!" 
          },
          imageUrl: "https://picsum.photos/400/400?random=11"
        },
        {
          id: 'min-mix', category: 'agro',
          name: { en: "Shwetdhara Min Chelated Mineral Mixture", hi: "श्वेतधारा मिन चिलेटेड मिनरल मिक्सचर" },
          price: 100,
          description: { 
            en: "🌿🐄 Region Special Chelated Mineral Mixture\n\n🧪 Optimal Nutrition, Increased Milk, Better Reproduction! 🐃🐐\n\nWith Regular Use:\n🔹 ✅ Increase in Milk Production\n🔹 ✅ Improvement in Reproductive Performance\n🔹 ✅ Overall Health Enhancement in Livestock\n🔹 ✅ Strong Bones and Muscles\n\n🧂 Key Ingredients (per 100 g):\nCalcium – 20 g\nPhosphorus – 10 g\nMagnesium – 1 g\nZinc, Iodine, Copper, Cobalt, Selenium, etc.\nVitamins A, D3, E\n\n📌 Animal Feed Supplement | Not for Human Consumption\n\n💰 ₹100 per kg (sometimes provided free with Shwetdhara Animal Feed – keep in touch)\n🌾 Healthy Animals, Higher Production, Prosperous Farmers!\n✅ ISO Certified Product\n🇮🇳 Developed with the joint collaboration of the Government of India and Government of Japan", 
            hi: "🌿🐄 क्षेत्र स्पेशल चिलेटेड मिनरल मिक्सचर\n\n🧪 उत्तम पोषण, ज़्यादा दूध, बेहतर प्रजनन! 🐃🐐\n\nनियमित प्रयोग से:\n🔹 ✅ दूध उत्पादन में बढ़ोतरी\n🔹 ✅ प्रजनन क्षमता में सुधार\n🔹 ✅ पशुओं की संपूर्ण सेहत में वृद्धि\n🔹 ✅ हड्डियाँ और मांसपेशियाँ होती हैं मज़बूत\n\n🧂 प्रमुख घटक (100 ग्राम में):\nकैल्शियम – 20 ग्राम\nफॉस्फोरस – 10 ग्राम\nमैग्नीशियम – 1 ग्राम\nजिंक, आयोडीन, कॉपर, कोबाल्ट, सेलेनियम आदि\nविटामिन A, D3, E\n\n📌 पशु आहार अनुपूरक | मानव उपयोग के लिए नहीं\n\n💰 ₹100 प्रति किलो (कभी-कभी श्वेतधारा पशु आहार के साथ मुफ्त – संपर्क में रहें)\n🌾 स्वस्थ पशु, ज़्यादा उत्पादन, किसान की तरक्की!\n✅ ISO प्रमाणित उत्पाद\n🇮🇳 भारत सरकार और जापान सरकार के संयुक्त सहयोग से विकसित" 
          },
          imageUrl: "https://picsum.photos/400/400?random=12"
        },
        {
          id: 'maize-seed', category: 'agro',
          name: { en: "Maize Seeds (Makka)", hi: "मक्का बीज" },
          price: 500,
          variants: [
            { id: 'dkc-9135', name: { en: "DKC 9135", hi: "DKC 9135" }, price: 500 },
            { id: 'dkc-9081', name: { en: "DKC 9081", hi: "DKC 9081" }, price: 700 }
          ],
          description: { 
            en: "Premium quality maize seeds for high yield fodder. Available in DKC 9081 and DKC 9135 varieties.", 
            hi: "अधिक उपज वाले चारे के लिए सर्वोत्तम गुणवत्ता वाले मक्का बीज। DKC 9081 और DKC 9135 किस्मों में उपलब्ध।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=13"
        },
        {
          id: 'grass-chari', category: 'agro',
          name: { en: "Animal Grass/Chari Seeds", hi: "पशु चारा/चरी बीज" },
          price: 350,
          description: { 
            en: "High-yield green fodder seeds for all seasons.", 
            hi: "सभी मौसमों के लिए उच्च उपज वाले हरे चारे के बीज।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=14"
        },
        {
          id: 'girawn', category: 'equipment',
          name: { en: "Girawn (Animal Husbandry)", hi: "गिरान (पशुपालन उत्पाद)" },
          price: 450,
          description: { 
            en: "Essential animal husbandry tools and accessories.", 
            hi: "आवश्यक पशुपालन उपकरण और सहायक उपकरण।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=15"
        },
        {
          id: 'ghee', category: 'dairy',
          name: { en: "Pure Desi Ghee", hi: "शुद्ध देसी घी" },
          price: 650,
          description: { 
            en: "Homemade pure ghee made from fresh milk cream. Rich in aroma and taste.", 
            hi: "ताजी दूध की मलाई से बना घर का शुद्ध घी। सुगंध और स्वाद में भरपूर।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=1"
        },
        {
          id: 'paneer', category: 'dairy',
          name: { en: "Fresh Paneer", hi: "ताज़ा पनीर" },
          price: 380,
          description: { 
            en: "Soft, fresh, and hygienic paneer. Perfect for Matar Paneer and other dishes. Bulk orders available.", 
            hi: "नरम, ताजा और स्वच्छ पनीर। मटर पनीर और अन्य व्यंजनों के लिए उत्तम। थोक ऑर्डर उपलब्ध।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=20"
        },
        {
          id: 'dahi', category: 'dairy',
          name: { en: "Fresh Dahi (Curd)", hi: "ताज़ा दही" },
          price: 60,
          description: { 
            en: "Thick and creamy curd made from pure milk.", 
            hi: "शुद्ध दूध से बना गाढ़ा और मलाईदार दही।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=21"
        },
        {
          id: 'matar', category: 'grocery',
          name: { en: "Green Peas (Matar)", hi: "हरी मटर" },
          price: 80,
          description: { 
            en: "Fresh frozen green peas.", 
            hi: "ताजी फ्रोजन हरी मटर।" 
          },
          imageUrl: "https://picsum.photos/400/400?random=22"
        }
      ];

      // --- MOCK SERVICES ---
      const MOCK_DELAY = 800;
      const mockUserStats = {
        memberId: "SMD-2024-001",
        name: "Rajesh Kumar",
        totalQuantity: 450.5,
        totalEarnings: 22525,
        lastCollectionDate: new Date().toLocaleDateString('en-IN')
      };
      const mockBestSeller = {
        id: "farm-88",
        name: "Suresh Yadav",
        totalQuantity: 1240
      };

      const authService = {
        login: async () => new Promise(resolve => setTimeout(() => resolve({ uid: "user_123", email: "member@shivdairy.com" }), MOCK_DELAY)),
        logout: async () => new Promise(resolve => setTimeout(() => resolve(), MOCK_DELAY / 2))
      };

      const dbService = {
        getUserStats: async (userId) => new Promise(resolve => setTimeout(() => resolve(mockUserStats), MOCK_DELAY)),
        getBestSeller: async () => new Promise(resolve => setTimeout(() => resolve(mockBestSeller), MOCK_DELAY))
      };

      // --- COMPONENTS ---

      const Clock = () => {
        const [time, setTime] = useState(new Date());
        useEffect(() => {
          const timer = setInterval(() => setTime(new Date()), 1000);
          return () => clearInterval(timer);
        }, []);

        const formatTime = (date) => date.toLocaleTimeString('en-IN', { timeZone: 'Asia/Kolkata', hour12: true, hour: '2-digit', minute: '2-digit', second: '2-digit' });
        const formatDate = (date) => date.toLocaleDateString('en-IN', { timeZone: 'Asia/Kolkata', day: '2-digit', month: 'short', year: 'numeric' });

        return (
          <div className="flex items-center space-x-2 text-green-800 bg-green-100 px-4 py-2 rounded-full shadow-sm">
            <Icons.Clock size={18} />
            <div className="flex flex-col sm:flex-row sm:space-x-2 text-sm font-semibold">
              <span>{formatTime(time)}</span>
              <span className="hidden sm:inline">|</span>
              <span>{formatDate(time)}</span>
            </div>
          </div>
        );
      };

      const ProductModal = ({ product, language, onClose }) => {
        const t = TRANSLATIONS.products;
        const [selectedVariant, setSelectedVariant] = useState(null);

        useEffect(() => {
          if (product.variants && product.variants.length > 0) setSelectedVariant(product.variants[0]);
          else setSelectedVariant(null);
        }, [product]);

        const currentPrice = selectedVariant ? selectedVariant.price : product.price;

        const handleWhatsAppOrder = () => {
          let message = `Namaste, I want to order *${product.name.en}*.`;
          if (selectedVariant) {
            message += `\nOption: ${selectedVariant.name.en}\nPrice: ₹${selectedVariant.price}`;
          } else {
            message += `\nPrice: ₹${product.price}`;
          }
          window.open(`https://wa.me/${PHONE_NUMBER.replace('+', '')}?text=${encodeURIComponent(message)}`, '_blank');
        };

        return (
          <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
            <div className="bg-white rounded-2xl shadow-2xl w-full max-w-lg overflow-hidden animate-fadeIn max-h-[90vh] flex flex-col">
              <div className="relative h-56 flex-shrink-0">
                <img src={product.imageUrl} alt={product.name[language]} className="w-full h-full object-cover" />
                <button onClick={onClose} className="absolute top-4 right-4 bg-white/80 p-2 rounded-full hover:bg-white transition-colors shadow-lg">
                  <Icons.X size={24} className="text-gray-800" />
                </button>
              </div>
              <div className="p-6 overflow-y-auto custom-scrollbar flex-1">
                <div className="flex justify-between items-start mb-2">
                  <h3 className="text-2xl font-bold text-gray-900 leading-tight">{product.name[language]}</h3>
                  <div className="text-right">
                    <p className="text-3xl font-bold text-green-600">₹{currentPrice}</p>
                    {product.variants && <span className="text-xs text-gray-500">per unit/pack</span>}
                  </div>
                </div>
                {product.variants && (
                  <div className="mb-6">
                    <p className="text-sm font-semibold text-gray-700 mb-2">{t.selectOption[language]}</p>
                    <div className="grid grid-cols-2 gap-3">
                      {product.variants.map((variant) => (
                        <button key={variant.id} onClick={() => setSelectedVariant(variant)} className={`px-4 py-3 rounded-lg border-2 text-left transition-all ${selectedVariant?.id === variant.id ? 'border-green-500 bg-green-50 text-green-800' : 'border-gray-200 hover:border-green-200'}`}>
                          <div className="flex justify-between items-center mb-1">
                            <span className="font-bold">{variant.name[language]}</span>
                            {selectedVariant?.id === variant.id && <Icons.Check size={16} className="text-green-600" />}
                          </div>
                          <span className="text-sm text-gray-600">₹{variant.price}</span>
                        </button>
                      ))}
                    </div>
                  </div>
                )}
                <div className="bg-green-50 p-4 rounded-xl mb-6">
                  <p className="text-gray-700 leading-relaxed whitespace-pre-line text-sm md:text-base">{product.description[language]}</p>
                </div>
              </div>
              <div className="p-4 border-t border-gray-100 bg-white">
                <button onClick={handleWhatsAppOrder} className="w-full flex items-center justify-center space-x-2 bg-green-600 hover:bg-green-700 text-white py-4 rounded-xl font-bold text-lg transition-transform active:scale-95 shadow-lg shadow-green-200">
                  <Icons.MessageCircle size={24} />
                  <span>{t.orderNow[language]}</span>
                </button>
              </div>
            </div>
          </div>
        );
      };

      const Dashboard = ({ user, language, onClose, onLogout }) => {
        const [stats, setStats] = useState(null);
        const [loading, setLoading] = useState(true);
        const t = TRANSLATIONS.dashboard;

        useEffect(() => {
          dbService.getUserStats(user.uid).then(setStats).catch(console.error).finally(() => setLoading(false));
        }, [user.uid]);

        return (
          <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
            <div className="bg-white rounded-2xl shadow-2xl w-full max-w-lg overflow-hidden flex flex-col max-h-[90vh]">
              <div className="bg-green-600 p-6 flex justify-between items-center text-white">
                <div className="flex items-center space-x-3">
                  <div className="bg-white/20 p-2 rounded-full"><Icons.User size={24} /></div>
                  <div><h2 className="text-xl font-bold">{t.title[language]}</h2><p className="text-green-100 text-sm">{user.email}</p></div>
                </div>
                <button onClick={onClose} className="hover:bg-white/20 p-2 rounded-full transition-colors"><Icons.X size={24} /></button>
              </div>
              <div className="p-6 overflow-y-auto flex-1">
                {loading ? (
                  <div className="flex justify-center items-center h-40"><div className="animate-spin rounded-full h-12 w-12 border-t-2 border-b-2 border-green-600"></div></div>
                ) : stats ? (
                  <div className="grid gap-4">
                    <div className="bg-yellow-50 p-4 rounded-xl border border-yellow-100 flex items-center space-x-4">
                      <div className="bg-yellow-100 p-3 rounded-full text-yellow-600"><Icons.User size={24} /></div>
                      <div><p className="text-sm text-gray-500">{t.memberId[language]}</p><p className="text-lg font-bold text-gray-800">{stats.memberId}</p></div>
                    </div>
                    <div className="bg-blue-50 p-4 rounded-xl border border-blue-100 flex items-center space-x-4">
                      <div className="bg-blue-100 p-3 rounded-full text-blue-600"><Icons.Droplet size={24} /></div>
                      <div><p className="text-sm text-gray-500">{TRANSLATIONS.stats.quantity[language]}</p><p className="text-xl font-bold text-gray-800">{stats.totalQuantity} {TRANSLATIONS.stats.liters[language]}</p></div>
                    </div>
                    <div className="bg-green-50 p-4 rounded-xl border border-green-100 flex items-center space-x-4">
                      <div className="bg-green-100 p-3 rounded-full text-green-600"><Icons.Wallet size={24} /></div>
                      <div><p className="text-sm text-gray-500">{t.earnings[language]}</p><p className="text-xl font-bold text-gray-800">₹{stats.totalEarnings.toLocaleString('en-IN')}</p></div>
                    </div>
                    <div className="bg-purple-50 p-4 rounded-xl border border-purple-100 flex items-center space-x-4">
                      <div className="bg-purple-100 p-3 rounded-full text-purple-600"><Icons.Calendar size={24} /></div>
                      <div><p className="text-sm text-gray-500">{t.lastCollection[language]}</p><p className="text-lg font-bold text-gray-800">{stats.lastCollectionDate}</p></div>
                    </div>
                  </div>
                ) : <p className="text-center text-gray-500">No data found.</p>}
                <button onClick={onLogout} className="mt-8 w-full py-3 border-2 border-red-500 text-red-500 font-semibold rounded-xl hover:bg-red-50 transition-colors">{TRANSLATIONS.header.logout[language]}</button>
              </div>
            </div>
          </div>
        );
      };

      const ProductCard = ({ product, language, onClick }) => (
        <div onClick={onClick} className="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-all cursor-pointer group border border-slate-100 flex flex-col h-full">
          <div className="h-48 overflow-hidden relative">
            <img src={product.imageUrl} alt={product.name[language]} className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500" />
            {product.variants && <div className="absolute bottom-2 right-2 bg-black/50 backdrop-blur-sm text-white text-[10px] px-2 py-1 rounded-full">{product.variants.length} Options</div>}
          </div>
          <div className="p-4 flex flex-col flex-1">
            <div className="flex justify-between items-start mb-2">
              <h4 className="font-bold text-lg text-gray-800 line-clamp-2 leading-snug">{product.name[language]}</h4>
              <span className="bg-green-100 text-green-800 text-xs font-bold px-2 py-1 rounded whitespace-nowrap ml-2">₹{product.price}{product.variants ? '+' : ''}</span>
            </div>
            <p className="text-sm text-gray-500 line-clamp-2 mb-4 flex-1 whitespace-pre-line">{product.description[language]}</p>
            <button className="w-full bg-slate-50 text-green-700 font-semibold py-2 rounded-lg border border-green-200 group-hover:bg-green-600 group-hover:text-white transition-colors">{TRANSLATIONS.products.viewDetails[language]}</button>
          </div>
        </div>
      );

      // --- MAIN APP COMPONENT ---
      const App = () => {
        const [language, setLanguage] = useState('hi');
        const [user, setUser] = useState(null);
        const [showDashboard, setShowDashboard] = useState(false);
        const [selectedProduct, setSelectedProduct] = useState(null);
        const [bestSeller, setBestSeller] = useState(null);
        const [isLoggingIn, setIsLoggingIn] = useState(false);
        const t = TRANSLATIONS;

        const agroProducts = PRODUCTS.filter(p => p.category === 'agro' || p.category === 'equipment');
        const dairyProducts = PRODUCTS.filter(p => p.category === 'dairy' || p.category === 'grocery');

        useEffect(() => {
          dbService.getBestSeller().then(setBestSeller).catch(console.error);
        }, []);

        const handleLogin = async () => {
          setIsLoggingIn(true);
          try {
            const u = await authService.login();
            setUser(u);
            setShowDashboard(true);
          } catch (e) { alert("Login failed"); } 
          finally { setIsLoggingIn(false); }
        };

        const handleLogout = async () => {
          await authService.logout();
          setUser(null);
          setShowDashboard(false);
        };

        const handlePreOrder = () => {
          const message = `Namaste, I am interested in Bulk Order / Pre-booking.`;
          window.open(`https://wa.me/${PHONE_NUMBER.replace('+', '')}?text=${encodeURIComponent(message)}`, '_blank');
        };

        return (
          <div className="min-h-screen bg-slate-50 font-sans text-slate-800">
            <header className="sticky top-0 z-40 bg-white shadow-md border-b border-green-100">
              <div className="bg-green-700 text-white text-xs py-1 px-4 text-center font-medium tracking-wide">{t.header.greeting[language]}</div>
              <div className="container mx-auto px-4 py-3 flex items-center justify-between">
                <div className="flex items-center space-x-3">
                  <img src="https://raw.githubusercontent.com/sb7863/shiv_milk_dairy/refs/heads/main/logo.png" alt="Shiv Milk Dairy Logo" className="h-12 w-12 md:h-16 md:w-16 object-contain rounded-full border-2 border-green-100 shadow-sm bg-white" />
                  <div>
                    <h1 className="text-lg md:text-xl font-bold text-green-800 leading-tight">{t.header.title[language]}</h1>
                    <div className="text-xs text-green-600 hidden md:block">{ADDRESS}</div>
                  </div>
                </div>
                <div className="flex items-center space-x-2 md:space-x-4">
                  <button onClick={() => setLanguage(prev => prev === 'en' ? 'hi' : 'en')} className="px-3 py-1 bg-green-50 text-green-700 text-sm font-semibold rounded-full hover:bg-green-100 transition">{language === 'en' ? 'हिंदी' : 'English'}</button>
                  {user ? (
                    <button onClick={() => setShowDashboard(true)} className="flex items-center space-x-1 bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700 transition shadow-sm">
                      <div className="w-2 h-2 bg-green-300 rounded-full animate-pulse"></div>
                      <span className="hidden sm:inline">{t.header.dashboard[language]}</span>
                      <span className="sm:hidden">Profile</span>
                    </button>
                  ) : (
                    <button onClick={handleLogin} disabled={isLoggingIn} className="flex items-center space-x-2 bg-slate-800 text-white px-4 py-2 rounded-lg hover:bg-slate-700 transition">
                      {isLoggingIn ? <span className="text-xs">...</span> : <><Icons.LogIn size={16} /><span className="hidden sm:inline">{t.header.login[language]}</span></>}
                    </button>
                  )}
                </div>
              </div>
            </header>

            <div className="bg-gradient-to-br from-green-50 to-yellow-50 py-8 md:py-12 px-4 text-center border-b border-yellow-100">
              <div className="container mx-auto">
                <div className="flex justify-center mb-6"><Clock /></div>
                <h2 className="text-3xl md:text-5xl font-extrabold text-green-800 mb-4 drop-shadow-sm">{t.hero.welcome[language]}</h2>
                <p className="text-lg text-slate-600 max-w-2xl mx-auto font-medium mb-8">{t.hero.subtitle[language]}</p>
              </div>
            </div>

            <main className="container mx-auto px-4 py-8 space-y-12">
              <section className="bg-gradient-to-r from-green-600 to-green-700 rounded-2xl p-6 md:p-8 shadow-xl text-white transform hover:scale-[1.01] transition-transform">
                <div className="flex flex-col md:flex-row items-center justify-between">
                  <div className="mb-6 md:mb-0 md:mr-6">
                    <div className="flex items-center space-x-3 mb-2"><Icons.Truck className="text-yellow-300" size={32} /><h3 className="text-2xl font-bold">{t.banners.bulkTitle[language]}</h3></div>
                    <p className="text-green-50 text-lg opacity-90 max-w-xl">{t.banners.bulkDesc[language]}</p>
                  </div>
                  <button onClick={handlePreOrder} className="bg-yellow-400 text-green-900 px-8 py-3 rounded-xl font-bold hover:bg-yellow-300 transition shadow-lg whitespace-nowrap">{t.banners.preOrderBtn[language]}</button>
                </div>
              </section>

              {bestSeller && (
                <section className="bg-white rounded-2xl p-6 shadow-lg border border-yellow-200">
                  <div className="flex flex-col md:flex-row items-center justify-between">
                    <div className="flex items-center space-x-4 mb-4 md:mb-0">
                      <div className="bg-yellow-100 p-4 rounded-full"><Icons.Award className="text-yellow-600 w-10 h-10" /></div>
                      <div><h3 className="text-xl font-bold text-gray-800">{t.stats.bestSeller[language]}</h3><p className="text-green-600 font-medium">{t.stats.topFarmer[language]}</p></div>
                    </div>
                    <div className="text-center md:text-right bg-green-50 px-6 py-3 rounded-xl w-full md:w-auto">
                      <p className="text-sm text-gray-500 uppercase tracking-wide">{t.stats.quantity[language]}</p>
                      <p className="text-3xl font-black text-green-700">{bestSeller.totalQuantity} <span className="text-lg text-green-500">{t.stats.liters[language]}</span></p>
                      <p className="text-sm font-semibold text-gray-700 mt-1">{bestSeller.name}</p>
                    </div>
                  </div>
                </section>
              )}

              <section>
                <div className="flex items-center space-x-3 mb-8"><div className="h-8 w-1 bg-green-500 rounded-full"></div><h3 className="text-2xl font-bold text-gray-800">{t.sections.agro[language]}</h3></div>
                <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                  {agroProducts.map(product => <ProductCard key={product.id} product={product} language={language} onClick={() => setSelectedProduct(product)} />)}
                </div>
              </section>

              <section>
                <div className="flex items-center space-x-3 mb-8"><div className="h-8 w-1 bg-yellow-400 rounded-full"></div><h3 className="text-2xl font-bold text-gray-800">{t.sections.dairy[language]}</h3></div>
                <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                  {dairyProducts.map(product => <ProductCard key={product.id} product={product} language={language} onClick={() => setSelectedProduct(product)} />)}
                </div>
              </section>

              <section className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div className="bg-blue-50 rounded-2xl p-6 border border-blue-100 flex flex-col">
                  <div className="flex items-center space-x-3 mb-4"><div className="bg-blue-100 p-2 rounded-full"><Icons.Clock className="text-blue-600" size={24} /></div><h4 className="text-lg font-bold text-blue-900">{t.info.collectionTimes[language]}</h4></div>
                  <div className="space-y-3 flex-1">
                    <div className="bg-white p-3 rounded-xl shadow-sm border border-blue-50"><span className="block font-medium text-gray-600 text-sm mb-1">{t.info.morning[language].split(':')[0]}</span><span className="block font-bold text-blue-600 text-lg">{t.info.morning[language].split(': ').slice(1).join(': ')}</span></div>
                    <div className="bg-white p-3 rounded-xl shadow-sm border border-blue-50"><span className="block font-medium text-gray-600 text-sm mb-1">{t.info.evening[language].split(':')[0]}</span><span className="block font-bold text-blue-600 text-lg">{t.info.evening[language].split(': ').slice(1).join(': ')}</span></div>
                  </div>
                </div>

                <div className="bg-purple-50 rounded-2xl p-6 border border-purple-100 flex flex-col">
                  <div className="flex items-center space-x-3 mb-4"><div className="bg-purple-100 p-2 rounded-full"><Icons.Syringe className="text-purple-600" size={24} /></div><h4 className="text-lg font-bold text-purple-900">{t.info.vet[language]}</h4></div>
                  <div className="space-y-4 flex-1">
                     <div className="bg-white/60 p-3 rounded-lg"><h5 className="font-bold text-purple-800 text-sm mb-1">{t.sections.ai[language]}</h5><p className="text-sm text-purple-900/80">{t.ai.desc[language]}</p></div>
                     <button onClick={() => window.open(`tel:${PHONE_NUMBER}`)} className="w-full flex items-center justify-center space-x-2 bg-purple-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-purple-700 transition"><Icons.Phone size={16} /><span>{t.info.contact[language]}</span></button>
                  </div>
                </div>

                <div className="bg-rose-50 rounded-2xl p-6 border border-rose-100 flex flex-col md:col-span-2 lg:col-span-1">
                  <div className="flex items-center space-x-3 mb-4"><div className="bg-rose-100 p-2 rounded-full"><Icons.Activity className="text-rose-600" size={24} /></div><h4 className="text-lg font-bold text-rose-900">{t.sections.health[language]}</h4></div>
                  <p className="text-rose-900/80 mb-4 flex-1">{t.health.desc[language]}</p>
                  <div className="bg-white/60 p-4 rounded-xl text-center"><p className="font-bold text-rose-700">{PHONE_NUMBER}</p></div>
                </div>
              </section>
            </main>

            <footer className="bg-slate-900 text-slate-300 py-12 mt-12">
              <div className="container mx-auto px-4 text-center md:text-left grid md:grid-cols-3 gap-8">
                <div><h5 className="text-white font-bold text-lg mb-4">{t.header.title[language]}</h5><p className="text-sm leading-relaxed max-w-xs mx-auto md:mx-0">{t.hero.subtitle[language]}</p></div>
                <div>
                  <h5 className="text-white font-bold text-lg mb-4">{t.info.contact[language]}</h5>
                  <div className="flex flex-col items-center md:items-start space-y-2">
                    <div className="flex items-center space-x-2"><Icons.Phone size={16} /><span>{PHONE_NUMBER}</span></div>
                    <div className="flex items-start space-x-2 text-left"><Icons.MapPin size={16} className="mt-1 flex-shrink-0" /><span>{ADDRESS}</span></div>
                  </div>
                </div>
                <div className="text-center md:text-right pt-4 md:pt-0">
                  <p className="text-xs text-slate-500">© {new Date().getFullYear()} Shiv Milk Dairy. All rights reserved.</p>
                  <div className="mt-2 text-xs text-slate-600">Jai Mata Di | Om Namah Shivay</div>
                </div>
              </div>
            </footer>

            {showDashboard && user && <Dashboard user={user} language={language} onClose={() => setShowDashboard(false)} onLogout={handleLogout} />}
            {selectedProduct && <ProductModal product={selectedProduct} language={language} onClose={() => setSelectedProduct(null)} />}
          </div>
        );
      };

      const root = ReactDOM.createRoot(document.getElementById('root'));
      root.render(<App />);
    </script>
  </body>
</html>
