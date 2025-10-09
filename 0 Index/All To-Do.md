``` dataview
TASK
WHERE (contains(file.folder, "Personal") OR contains(file.folder, "05 Daily Notes") OR contains(file.folder, "01 Inbox")) AND !completed
GROUP BY file.link
SORT file.name ASC
```


