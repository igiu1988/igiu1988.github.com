---
layout: post
title: "iOS Core Data 入门"
date: 2013-06-04 15:29
comments: true
categories: 
---

做了一年多iOS开发，真没怎么用过Core Data，来新公司一直没有活，就看Core Data了。网上一搜一大把关于Core Data的教程，我以我自己的方式重新再说一次，如看不懂就再自己搜索吧。
##注意
Core Data不是数据库，只是对SQLite的封装，集成了很多内容，并且高效。
##开始
我新建了一个类，SQLManager : NSObject

添加了一个类方法，目的是全局使用一个SQLManager对象

<!--more-->

```objective-c
+ (SQLManager *)sharedSQLManager
{
    static dispatch_once_t onceToken;
    static id shareInstance;
    dispatch_once(&onceToken, ^{
        shareInstance = [[self alloc] init];
    });
    
    return shareInstance;
}
```

Core Data涉及三个对象是肯定每次都要用的的

```objective-c
@property (nonatomic, strong) NSManagedObjectModel *managedObjectModel;
@property (nonatomic, strong) NSManagedObjectContext *managedObjectContext;
@property (nonatomic, strong) NSPersistentStoreCoordinator *persistentStoreCoordinator;
```

使用一个initCoreData来初始化这三个对象（SQLManager的属性）
```objective-c
- (void)initCoreData
{
    // 既然涉及数据库，肯定要有一个文件来存储数据
    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    NSString *basePath = [paths objectAtIndex:0];
    NSURL *storeUrl = [NSURL fileURLWithPath:[basePath stringByAppendingPathComponent:@"TestDB.sqlite"]];
    
    /* 初始化 managedObjectModel 
     * managedObjectModel 的初始化是依据工程中的xcdatamodeld文件，
     */
    _managedObjectModel = [NSManagedObjectModel mergedModelFromBundles:nil];
    
    /* 初始化 persistentStoreCoordinator
     * persistentStoreCoordinator 的初始化需要刚才的 managedObjectModel，这里我添加了一个option，该option会在数据库的版本控制及轻量迁移中用到。
     * 数据库的版本控制及轻量迁移见另一篇文章: iOS Core Data Version
     */
    NSError *error;
    // option use for lightweight migration
    NSDictionary * option = [NSDictionary dictionaryWithObjectsAndKeys:
                             [NSNumber numberWithBool:YES], NSMigratePersistentStoresAutomaticallyOption,
                             [NSNumber numberWithBool:YES], NSInferMappingModelAutomaticallyOption, nil];
    
    _persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:_managedObjectModel];
    if (![_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeUrl options:option error:&error]) {
        NSLog(@"%@: %@", [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleDisplayName"], error.localizedDescription);
    }
    
    /* 初始化 managedObjectContext
     * managedObjectContext 的初始化需要上面的 persistentStoreCoordinator
     */
    _managedObjectContext = [[NSManagedObjectContext alloc] init];
    _managedObjectContext.persistentStoreCoordinator = _persistentStoreCoordinator;
}
```




参考文档：

* Core Data Basics
* Core Data Programming Gudie
* google及baidu的各种搜索