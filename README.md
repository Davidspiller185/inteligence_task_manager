# תיאור המערכת
```text
תפקיד המערכת הוא לנהל סוכנים והמשימות שלהם המטרה היא  
 שמנהל את  SQL  לבנות מערכת עם שרת שמתחבר למסד נתונים של  
 של הסוכנים ואת מערכת המשימות  SQL מערכת הסוכנים דרך טבלת  של המשימות השרת יוכל לקבל מידע על כל סוכן  SQL  דרך טבלת 
 דרך מסד הנתונים של טבלת הסוכן למשל כמה משימות בסך הכל יש
 לסןכן הספציפי הזה כמה מהם הוא לא השלים אותם וכמה מהם הוא כן השלים אותם ולהחזיר את הנתונים ללקוח , וכן השרת 
 יוכל ליצור סוכן חדש וגם לעדכן את מצב הסוכן עם משימותיו
 באותה מידה השרת יוכל לקבל מידע על המשימות של כלל הסוכנים  דרך מסד הנתונים של טבלת המשימות למשל לספור כמה משימות יש בסך הכל לכלל הסוכנים ולהחזיר אותם ללקוח ,וכן השרת יוכל ליצור משימות חדשות וגם לעדכן שינוי סטטוס של    משימה אם היא הושלמה או לא ובכך הערכת תנהל דרך מסד 
.הנתונים את הסוכנים ומשימותיהם
```

# מבנה התיקיות
```text
intelligence-task-manager/ 
├── database/ 
│   ├── db_connection.py 
│   ├── agent_db.py 
│   └── mission_db.py 
├── README.md 
├── requirements.txt 
└── .gitignore 
```

# מבנה הטבלאות
``` python
**טבלת agents - שדות**
id - int
name - str
speciality - str
is_active - bool
completed_missions - int
failed_missions - int
agent_rank - str

**טבלת missions - שדות**
id -int
title -str
description - str
location -str
difficulty - int
importance - int
status - str
risk_level -str
assigned_agent_id -int
```
# הסבר על המחלקות והמתודות שלהם
``` text
** הסבר על מחלקת db_connection והמתודות **
 שאחראית על החיבור למסד הנתונים db_connection מחלקת     
 ויצירת הטבלאות MySQL 
** מתודות **
get_connection() - MySQL מחזירה חיבור פעיל ל 
create_database() - אם לא קייםinteligence_dbיוצרת את 
create_tables() - יוצרת את שתי הטבלאות אם לא קיימות

**