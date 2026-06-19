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

**מחלקת AgentDB והמתודות**
** מחלקה **

agents מול טבלת SQL  אחראית על כל פעולות 

** מתודות **
create_agent(data) - יוצרת סוכן חדש ומחזירה את המילון של אותו סוכן
get_all_agents()  - מחזירה רשימת כל הסוכנים
get_agent_by_id(id) - None או  id מחזירה סוכן לפי 
update_agent(id, data) -id ואי אפשר לשנות  UPDATE 
deactivate_agent(id) -מגדירה מצב סוכן ללא פעיל
increment_completed(id) - מעדכן את כמות המשימות שהושלמו
increment_failed(id) - מעדכן את כמות המשימות שנכשלו
get_agent_performance(id) - total,מחזירה מילון עם המפתחות 
                            failed,completed,success_rate 
count_active_agents() - מחזירה את מספר הסוכנים הפעילים

**  והמתודותMissionDB מחלקת **
** מחלקה **
mission מול טבלת   SQL היא אחראית על כל פעולות   
** מתודות **
create_mission(data) - יצירת משימה חדשה ומחזירה את כל    האובייקט      
get_all_missions() - מחזירה את כל המשימות
get_mission_by_id(id) -None או  id מחזירה משימה אחת לפי 
assign_mission(m_id, a_id) - משייכת משימה לסוכן
update_mission_status(id, status) -משמשת לכל שינוי סטטוס
get_open_missions_by_agent(id) -ASSIGNED מחזירה משימות 
                                /IN_PROGRESS של סוכן
count_all_missions() - סה''כ משימות
count_by_status(status) - סופרת לפי סטטוס מסויים
count_open_missions() - סופרת לפי משימות פתוחות
count_critical_missions() -CRITICAL סופרת משימות 
get_top_agent() - הגבוה ביותרcompleted_missionsהסוכן עם 
```

# חוקי המערכת
``` text
1 - כל ערך אחר מחזירjunior/senior/commander חייב להיות rank 
.שגיאה
2 - חייבים להיות בין 1-10אחרת שגיאה importanceו difficulty 
3 -מחושב אוטומטית בעת יצירת משימה המשתמש לא שולח  risk_level
אותו
4 - לא יכול לקבל משימות is_active = False סוכן עם 
5 - סוכן לא יכול להחזיק יותר מ 3 משימות פתוחות 
    (ASSIGNED/IN_PROGRESS)
6 -    Commander רק סוכן בדרגת    risk_level=CRITICAL אם 
יכול לקבל את המשימה
7 - status=ASSIGNED:לאחר שיווךNEW ניתן לשייך רק משימה בסטטוס
8 - ניתן להתחיל רק משימה בסטטוס לאחר מכן ASSIGNED
status = IN_PROGRESS
9 - failed ולשנות סטטוס  IN_PROGRESS ניתן לסיים רק משימה 
או completed    
10 -   אחרת שגיאהASSIGNED או  NEW ניתן לבטל רק משימה בסטטוס 
```

# הוראות הרצה
``` bash
docker run -d --name intelligence-mysql -e MYSQL_ROOT_PASSWORD=1234 \ 
  -e MYSQL_DATABASE=Intelligence_db -p 3306:3306 mysql:8.0 
pip install mysql-connector-python
python -m venv .venv
source .venv/Scripts/activate
pip install 'fastapi[standard]'
pip frezze > requirements.txt
uvicorn main:app
```


