نکات مربوط به SpringData & Hibernate : 

اگه از Hibernate بعنوان JPA Provider استفاده می‌کنید و نیاز دارید Persistence Context رو مانیتور کنید می تونید از SharedSessionContractImplementor کمک بگیرد.
کافیه بوسیله ی EntityManager اول unwrap اش کنید

‍‍‍‍‍‍‍‍‍‍‍‍```
SharedSessionContractImplementor sharedSession = entityManager.unwrap(SharedSessionContractImplementor.class );
```
و بعدش با getPersistenceContext رفرنس pc رو بگیرید:
```
PersistenceContext pc = sharedSession.getPersistenceContext();
```
متدهای زیر هم از طریقش قابل دسترس هست که بدردتون میخوره اگه خواستید لاگ بزنید یا تریس کنید :
```
pc.getNumberOfManagedEntities();
```
که تعداد entity هایی که تو حالت managed هستند رو برمیگردونه.
```
pc.getCollectionEntriesSize();
```
تعداد association هایی که تو حالت managed هستند رو برمیگردونه.
```
pc.getEntitiesByKey();
```

یک مپ Map<EntityKey, Object برمیگردونه که EntityKey میشه ترکیب کلاس اون Entity و مقدار کلیدی که تو دیتابیس هست ،‌ Object هم مقادیری هست که لود شده از دیتابیس :
```
EntityEntry ee = pc.getEntry(obj);
```
اگه دنبال entityName , status , hydrated State entity هستید از طریق رفرنس مربوط به EntityEntry میتونید به متد هایی که این مقادیر و برمیگردونن برسید:
```
ee.getEntityName
ee.getStatus
ee.getLoadedState
```
