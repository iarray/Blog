# ContentProvider 数据源组件
	在安卓中除了放在扩展存储设备等共享目录中的数据，各应用所持有的数据库，文件，资源信息
	等内容都是私有的，其他应用没有权限直接访问。这样的设计保证了应用的独立性和安全性，避
	免数据的暴露和破坏。
	
	如果应用直接需要共享数据，比如获取手机通讯录的联系人数据，获取短信数据等，应用之间实
	现数据查询，修改等操作需要借助ContentProvider-数据源组件。

## 抽象类ContentProvider
* query(Uri, String[], String, String[], String) 查询数据
* insert(Uri, ContentValues) 新增数据
* update(Uri, ContentValues, String, String[]) 修改数据
* delete(Uri, String, String[]) 删除数据
* getType(Uri) 取数据类型

### Uri的定义
* ContentProvider（内容提供者）的scheme已经由Android所规定， scheme为：content://

* Authorities用于唯一标识这个ContentProvider，外部调用者可以根据这个标识来找到它。一般就是包名


		Public static final Uri CONTENT_URI = Uri.parse(
			"content://com.Test.db"
		);
		
		//要操作person表中id为10的记录，可以构建这样的路径:content://com.Test.db/person/10
		//要操作person表中id为10的记录的name字段,可以这样 content://com.Test.db/person/10/name
		//要操作person表中的所有记录 content://com.Test.db/person
		//要操作xxx表中的记录 content://com.Test.db/xxx
		//当然要操作的数据不一定来自数据库，也可以是文件、xml或网络等其他存储方式
		 

* 在manifest中配置

		<provider
            android:name="com.Test.db.DBProvider"
            android:authorities="com.Test.db" />

### UriMatcher类
这个类是用来定义Uri的匹配，并得到匹配的结果
	
	public class DBProvider extends ContentProvider {

		//定义ContentProvider的唯一标识
		public static final String AUTHORITY = "com.Test.db";

		//定义UriMatcher
		private static final UriMatcher URI_MATCHER;

		//定义匹配的结果值
		private static final int PERSON = 1, PERSON_ID = 2;

		static{

			//静态构造函数创建UriMatcher
			URI_MATCHER = new UriMatcher(UriMatcher.NO_MATCH);

			//定义person表的数据匹配
			URI_MATCHER.addURI(AUTHORITY, "person", PERSON);

			//定义person表的id数据匹配
			URI_MATCHER.addURI(AUTHORITY, "person/#", PERSON_ID);
		}
	}

### ContentUris类使用介绍
ContentUris类用于操作Uri路径后面的ID部分，它有两个比较实用的方法：

* `withAppendedId(uri, id)` 用于为路径加上ID部分
* `parseId(uri)` 方法用于从路径中获取ID部分

### 实现ContentProvider的方法
* ContentProvider类主要方法的作用：
	1. onCreate()：该方法在ContentProvider创建后就会被调用

	2. insert(Uri uri, ContentValues values)：供外部应用往ContentProvider添加数据。

	3. delete(Uri uri, String selection, String[] selectionArgs)：供外部应用从ContentProvider删除数据。

	4. update(Uri uri, ContentValues values, String selection, String[] selectionArgs)：供外部应用更新ContentProvider中的数据。
	
	5. query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)：供外部应用从ContentProvider中获取数据。
	
	6. String getType(Uri uri)：该方法用于返回当前Url所代表数据的MIME类型。

		> 如果操作的数据属于集合类型，那么MIME类型字符串应该以vnd.android.cursor.dir/开头
		  
		> 例如：要得到所有person记录的Uri为content://com.Test.db/person，那么返回的MIME类型字符串应该为："vnd.android.cursor.**dir**/person"。
		
		> 如果要操作的数据属于非集合类型数据，那么MIME类型字符串应该以vnd.android.cursor.item/开头，
		
		> 例如：得到id为10的person记录，Uri为content://com.Test.db/person/10，那么返回的MIME类型字符串为："vnd.android.cursor.**item**/person"。	

代码实现：

	public class DBProvider extends ContentProvider {
	
		....
		//这里省略UriMatcher的定义及创建

		public static final Uri CONTENT_URI = Uri.parse(
			"content://" + AUTHORITIES + "/person"
		);

		@Override
		public Uri insert(Uri uri, ContentValues initialValues) {
			int matched = URI_MATCHER.match(uri);
			switch (matched) {
				case PERSON:
					SQLiteDatabase database = dbHelp.getWritableDatabase();  
		            long rowId = database.insert("person", null, values);  
		           	if(rowId > 0){//判断插入是否执行成功
						Uri insertedUserUri = ContentUris.withAppendedId(
							CONTENT_URI, rowId
						);
				　　 		
						//通知监听器，数据已经改变
						getContext().getContentResolver().notifyChange(
							insertedUserUri, null
						);

				　　 		return insertedUserUri;
				　　 	}
					break;
				default:
					break;
			}
		}
	}


## 使用ContentResolver操作ContentProvider中的数据
* 添加一条记录

		ContentResolver resolver =  getContentResolver();
		Uri uri = Uri.parse("content://com.Test.db/person");
		ContentValues values = new ContentValues();
		values.put("name", "xiaoming");
		values.put("age", 25);
		resolver.insert(uri, values);  

* 获取person表中所有记录

		Cursor cursor = resolver.query(uri, null, null, null, "");
		while(cursor.moveToNext()){
		   Log.i("ContentTest", "name=" + cursor.getString(1));
		}

* 更新数据
		
		//把id为2的记录的name字段值更改新为zhangsan
		ContentValues updateValues = new ContentValues();
		updateValues.put("name", "zhangsan");
		Uri updateIdUri = ContentUris.withAppendedId(uri, 2);
		resolver.update(updateIdUri, updateValues, null, null)

* 删除记录

		Uri deleteIdUri = ContentUris.withAppendedId(uri, 2);
		resolver.delete(deleteIdUri, null, null);

如果ContentProvider的访问者需要得到数据变化通知，必须使用ContentObserver对数据（数据采用uri描述）进行监听，当监听到数据变化通知时，系统就会调用ContentObserver的onChange()方法：

	getContentResolver().registerContentObserver(
		Uri.parse(CONTENT_URI),
		true, 
		new PersonObserver(new Handler())
	);
	
	public class PersonObserver extends ContentObserver{
	   public PersonObserver(Handler handler) {
	      super(handler);
	   }
	   public void onChange(boolean selfChange) {
	      //此处可以进行相应的业务处理
	   }
	}

* 权限设置与线程同步
	1. 可以在代码中通过setReadPermission()和setWritePermission()两个方法来设置ContentProvider的操作权限，也可以在配置文件中通过android:readPermission和android:writePermission属性来控制。
	2. 因为ContentProvider可能被不同的进程和线程调用，所以里面的方法必须是线程安全的。