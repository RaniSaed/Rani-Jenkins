
---

## 🎤 **הצגת Jenkinsfile – CI/CD + DR Automation (2–4 דקות)**

שלום לכולם 👋

בפרויקט שלי פיתחתי Jenkins Pipeline חכם שמבצע תהליך **CI/CD מלא ל־Backend** של מערכת Smart Retail.
בנוסף, שילבתי בו גם **בדיקות Failover ו־Disaster Recovery אוטומטיות** — וזה אחד החלקים הכי חזקים במימוש.

---

### 1️⃣ **Clone & Change Detection**

* השלב הראשון משכפל את שני הריפוזיטוריים שלי: קוד (`smart-retail-dev`) והגדרות (`smart-retail-config`)
* אם אין שינויים בתיקיית ה־backend – הפייפליין נעצר כדי לחסוך זמן ו־resources

---

### 2️⃣ **Build & Push Docker Image**

* נבנה Docker Image חדש עם tag לפי מספר Build
* ואז נשלח ל־Docker Hub עם גם tag `latest`
* ככה אני תמיד שומר גם גרסה מתוייגת וגם גרסה כללית

---

### 3️⃣ **Kubernetes Deployment Automation**

* אני מעדכן אוטומטית את `deployment.yaml` עם ה־Image החדש
* ואם היה שינוי – אני מבצע commit ו־push לריפו של הקונפיג
* זה מאפשר ל־ArgoCD למשוך ולהחיל את השינויים על הקלאסטר

---

### 4️⃣ **Health Check**

* לפני שאני ממשיך – אני בודק את בריאות המערכת ע"י קריאות ל־/health עד שהתשובה תקינה
* אם זה נכשל – הפייפליין נופל ומחזיר שגיאה

---

### 5️⃣ **DR Failover & Sync (Disaster Recovery)**

* כאן מגיע החלק הייחודי: אני בודק האם ה־containers של ה־Primary פועלים
* אם הם נופלים – המערכת מפעילה אוטומטית את DR Containers (frontend וה־backend החלופי)
* אני גם מריץ את `seed.py` כדי לסנכרן את הנתונים בין המערכות

---

### 6️⃣ **DR Readiness Report + Slack Notifications**

* בסוף, אני מייצר דו"ח DR מלא עם תוצאה, סטטוס בריאות, ותהליך סנכרון
* אני גם שולח את כל התוצאות לערוץ Slack — אם הצליח או נכשל
* התוצאה נשמרת כקובץ artifacts שאפשר להוריד מ־Jenkins

---

### 🔚 **לסיכום**

ה־Pipeline הזה:

* אוטומטי לחלוטין
* יודע לזהות שינויים, לבנות ולפרוס
* כולל בדיקת בריאות, DR אקטיבי, וסנכרון מידע
* ומייצר דו"חות אוטומטיים והתראות ב־Slack

המערכת יודעת לתפקד גם במקרה של כשל מלא ב־backend או frontend — וזה קריטי במערכות Production אמיתיות.

תודה רבה! 🙏

---

