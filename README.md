نکات مربوط به SpringData & Hibernate : 

اگه از Hibernate بعنوان JPA Provider استفاده می‌کنید و نیاز دارید Persistence Context رو مانیتور کنید می تونید از SharedSessionContractImplementor کمک بگیرد.

کافیه بوسیله ی EntityManager اول unwrap اش کنید
```java
SharedSessionContractImplementor sharedSession = entityManager.unwrap(SharedSessionContractImplementor.class );
```
و بعدش با getPersistenceContext رفرنس pc رو بگیرید:
```java
PersistenceContext pc = sharedSession.getPersistenceContext();
```
متدهای زیر هم از طریقش قابل دسترس هست که بدردتون میخوره اگه خواستید لاگ بزنید یا تریس کنید :
```java
pc.getNumberOfManagedEntities();
```
که تعداد entity هایی که تو حالت managed هستند رو برمیگردونه.
```java
pc.getCollectionEntriesSize();
```
تعداد association هایی که تو حالت managed هستند رو برمیگردونه.
```java
pc.getEntitiesByKey();
```

یک مپ <Map<EntityKey, Object برمیگردونه که EntityKey میشه ترکیب کلاس اون Entity و مقدار کلیدی که تو دیتابیس هست ،‌ Object هم مقادیری هست که لود شده از دیتابیس :
```java
EntityEntry ee = pc.getEntry(obj);
```
اگه دنبال entityName , status , hydrated State entity هستید از طریق رفرنس مربوط به EntityEntry میتونید به متد هایی که این مقادیر و برمیگردونن برسید:
```java
ee.getEntityName
ee.getStatus
ee.getLoadedState
```
