# 🧠 OffSec Command Center

## 🔍 Recently Worked On
```dataview
table file.mtime as "Last Modified"
from "CTFs/Drafts"
sort file.mtime desc
limit 5
```

## 📚 Open CTF Drafts
```dataview
table file.name as "Draft", Status, Tags
from "CTFs/Drafts"
where Status != "Completed"
sort file.mtime desc
```


## 🔁 Technique Repetition Tracker
```dataview
table file.name, UsedIn[0]
from "Techniques"
sort file.name
```




![](Images/Pasted%20image%2020250720175830.png)