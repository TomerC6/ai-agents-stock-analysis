# 🤖 AI Agents for Stock Research and Analysis
### מערכת סוכנים חכמים לניתוח שוק ההון

![n8n](https://img.shields.io/badge/Platform-n8n-orange?style=flat-square&logo=n8n)
![Gemini](https://img.shields.io/badge/LLM-Gemini%201.5%20Flash-blue?style=flat-square&logo=google)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-Academic-lightgrey?style=flat-square)

> פרויקט גמר | קורס: Machine Learning and Model Development | מרצה: עופר זוין

---

## 📋 תוכן עניינים

- [סקירה כללית](#-סקירה-כללית)
- [ארכיטקטורה](#-ארכיטקטורה)
- [דרישות מקדימות](#-דרישות-מקדימות)
- [התקנה והפעלה](#-התקנה-והפעלה)
- [דוגמת שימוש](#-דוגמת-שימוש)
- [תוצאות ובדיקות](#-תוצאות-ובדיקות)
- [מגבלות](#-מגבלות)
- [כיווני פיתוח](#-כיווני-פיתוח)

---

## 🔍 סקירה כללית

מערכת **Multi-Agent** המספקת ניתוח השקעות בזמן אמת עבור מניות ישראליות ואמריקאיות. המערכת משלבת שני סוכני AI אוטונומיים הפועלים בסדר סדרתי:

| סוכן | תפקיד | כלים |
|------|--------|------|
| 🔬 **Researcher** | איסוף נתוני שוק בזמן אמת | Alpha Vantage API, Yahoo Finance API |
| 📊 **Analyst** | ניתוח מעמיק + המלצת השקעה | Google Gemini 1.5 Flash, Simple Memory |

### ✨ יכולות עיקריות
- ✅ תמיכה במניות **ת"א 35** (סיומת `.TA`) ומניות **אמריקאיות** (NYSE, NASDAQ)
- ✅ המלצת השקעה: **BUY / HOLD / SELL**
- ✅ יעד מחיר ל-30 יום
- ✅ ניתוח הזדמנויות וסיכונים
- ✅ זיכרון שיחה למעקב ושאלות המשך
- ✅ זמן תגובה: **15–20 שניות**

---

## 🏗️ ארכיטקטורה

```
┌─────────────────────────────────────────────┐
│              קלט משתמש (Chat)               │
│        "תנתח לי את מניית טבע"              │
└──────────────────┬──────────────────────────┘
                   │
         ┌─────────▼──────────┐
         │    n8n Workflow     │
         │  ┌───────────────┐  │
         │  │  Researcher   │  │
         │  │  Alpha Vantage│  │
         │  │  Yahoo Finance│  │
         │  └──────┬────────┘  │
         │         │           │
         │  ┌──────▼────────┐  │
         │  │   Analyst     │  │
         │  │ Gemini Flash  │  │
         │  │    Memory     │  │
         │  └──────┬────────┘  │
         └─────────┼───────────┘
                   │
┌──────────────────▼──────────────────────────┐
│           דוח תובנות למשתמש                │
│    ניתוח + המלצה + יעד מחיר + סיכונים     │
└─────────────────────────────────────────────┘
```

---

## ⚙️ דרישות מקדימות

- [n8n](https://n8n.io/) — Self-hosted (local)
- חשבון [Google AI Studio](https://aistudio.google.com/) לקבלת Gemini API Key
- חשבון [Alpha Vantage](https://www.alphavantage.co/) לקבלת API Key (חינמי)

---

## 🚀 התקנה והפעלה

### 1. שכפול הפרויקט
```bash
git clone https://github.com/YOUR_USERNAME/ai-agents-stock-analysis.git
cd ai-agents-stock-analysis
```

### 2. הגדרת משתני סביבה
```bash
cp .env.example .env
```
ערוך את קובץ `.env` והוסף את ה-API Keys שלך:
```env
ALPHA_VANTAGE_API_KEY=your_key_here
GEMINI_API_KEY=your_key_here
```

### 3. ייבוא ה-Workflow ל-n8n
1. פתח את n8n (`http://localhost:5678`)
2. לחץ על **"Import from File"**
3. בחר את הקובץ `workflow/stock_analysis_workflow.json`
4. עדכן את ה-Credentials עם ה-API Keys שלך
5. הפעל את ה-Workflow

---

## 💬 דוגמת שימוש

**קלט:**
```
תנתח לי את מניית בנק הפועלים
```

**פלט:**
```
📊 בנק הפועלים (POLI.TA) – ניתוח השקעה

💰 מחיר נוכחי: ₪69.53 | שינוי יומי: +0.19%
📈 מגמה: המניה נסחרת בטווח יציב שבין 68 ל-70 ש"ח,
   תוך הפגנת חוסן מרשים מול חוסר הוודאות הגיאופוליטי.

🎯 המלצה: BUY (קנייה)
📅 מחיר יעד ל-30 יום: ₪73.00

🟢 הזדמנויות:
• רווחיות גבוהה המושפעת לטובה מסביבת הריבית
• מכפיל הון נמוך — תמחור חסר (Under-priced)

🔴 סיכונים:
• חשיפה לתנודתיות המאקרו-כלכלית בישראל
• המצב הביטחוני עלול להגביל עליות בטווח הקצר

⚡ סיכום מנהלים: נכס "עוגן" יציב עם פוטנציאל
   אפסייד משמעותי עם התבהרות המצב הביטחוני.
```

---

## 📊 תוצאות ובדיקות

| מניה | בורסה | זמן תגובה | סטטוס |
|------|--------|-----------|--------|
| Apple (AAPL) | NASDAQ | ~19 שניות | ✅ הצלחה |
| בנק הפועלים (POLI.TA) | ת"א 35 | ~15 שניות | ✅ הצלחה |
| מיקרוסופט (MSFT) | NASDAQ | ~17 שניות | ✅ הצלחה |
| נייס (NICE.TA) | ת"א 35 | ~16 שניות | ✅ הצלחה |
| טבע (TEVA.TA) | ת"א 35 | ~15 שניות | ✅ הצלחה |

---

## ⚠️ מגבלות

- **Alpha Vantage חינמי**: מוגבל ל-25 בקשות ביום
- **ניתוח מחיר בלבד**: אין נתונים פונדמנטליים (P/E, דוחות רבעוניים)
- **לא יועץ השקעות**: המערכת אינה מחליפה ייעוץ מקצועי מורשה
- **תלות ב-APIs**: זמינות המערכת תלויה בספקים חיצוניים

---

## 🔮 כיווני פיתוח

- [ ] הוספת RSS Feed לניתוח סנטימנט מחדשות
- [ ] שילוב אינדיקטורים טכניים (RSI, MACD, Bollinger Bands)
- [ ] פריסה על שרת ענן (Render / Railway) לזמינות 24/7
- [ ] Telegram Bot להתראות אוטומטיות
- [ ] היסטוריית ניתוחים ומעקב דיוק לאורך זמן
- [ ] API לנתונים פונדמנטליים (Financial Modeling Prep)

---

## 📚 מקורות

- [n8n Documentation](https://docs.n8n.io)
- [Alpha Vantage API](https://www.alphavantage.co/documentation)
- [Google Gemini API](https://ai.google.dev/docs)
- [Yahoo Finance API](https://query1.finance.yahoo.com)

---

> ⚠️ **Disclaimer**: פרויקט זה נועד למטרות אקדמיות בלבד. אין לראות בתכניו המלצת השקעה.
