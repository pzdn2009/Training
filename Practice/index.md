# Index

# 創建索引

```csharp
[Index("PostRatingIndex")] 
public int Rating { get; set; }

[Index]
public int Rating { get; set; } 
```

或者
```csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.MyProperty)
    .HasColumnAnnotation(
        IndexAnnotation.AnnotationName, 
        new IndexAnnotation(new IndexAttribute()));

```