# EF Query

## CRUD

Esempi trovi qua:  
http://www.entityframeworktutorial.net/Querying-with-EDM.aspx

### Create: Inserire un record nel db
```
using (var context = new SchoolDBEntities())
{
    var std = new Student()
    {
        FirstName = "Bill",
        LastName = "Gates"
    };
    context.Students.Add(std);

    context.SaveChanges();
}
```

### Read: Una semplice select
#### Con lambda:
```
using (var ctx = new SchoolDBEntities())
{    
    var student = ctx.Students
                    .Where(s => s.StudentName == "Bill")
                    .FirstOrDefault<Student>();
}
```

#### Con LINQ
```
using (var ctx = new SchoolDBEntities())
{    
    var student = (from s in ctx.Students
                where s.StudentName == "Bill"
                select s).FirstOrDefault<Student>();
}
```

### Update: La Update
```
using (var context = new SchoolDBEntities())
{
    var std = context.Students.First<Student>(); 
    std.FirstName = "Steve";
    context.SaveChanges();
}
```

### Delete: La delete dal DB
```
using (var context = new SchoolDBEntities())
{
    var std = context.Students.First<Student>();
    context.Students.Remove(std);

    context.SaveChanges();
}
```


