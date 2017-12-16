# Events

事件：
* BeforeCreateEvent 創建之前
* AfterCreateEvent  創建之後
* BeforeSaveEvent  保存之前
* AfterSaveEvent   保存之後
* BeforeLinkSaveEvent  鏈接之前 
* AfterLinkSaveEvent  鏈接之後
* BeforeDeleteEvent 刪除之前
* AfterDeleteEvent 刪除之後

## 應用程序級的Listener

```java
public class BeforeSaveEventListener extends AbstractRepositoryEventListener {

  @Override
  public void onBeforeSave(Object entity) {
    ... logic to handle inspecting the entity before the Repository saves it
  }

  @Override
  public void onAfterDelete(Object entity) {
    ... send a message that this entity has been deleted
  }
}
```

## 註解處理器

定義針對Person實體的監控：
```java
@RepositoryEventHandler 
public class PersonEventHandler {

  @HandleBeforeSave
  public void handlePersonSave(Person p) {
    // … you can now deal with Person in a type-safe way
  }

  @HandleBeforeSave
  public void handleProfileSave(Profile p) {
    // … you can now deal with Profile in a type-safe way
  }
}
```

注入到容器：
```java
@Configuration
public class RepositoryConfiguration {

  @Bean
  PersonEventHandler personEventHandler() {
    return new PersonEventHandler();
  }
}
```