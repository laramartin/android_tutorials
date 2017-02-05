# Create a local database with SQLiteOpenHelper

## Requirements
- Install SQLite https://sqlite.org/download.html

## Create a Contract class

Contract class: 
- Defines the database schema
- Declares the database name and version, tables and columns names
- Defines a inner clause for each table, and includes the columns for each of those tables
- Since all the content is static, it makes sense to make the constructor private
- In this example, the contract class is called `StockContract`
- The `StockEntry` implements the interface BaseColumns, that includes a constant representing the primary key called `_ID`
```
public class StockContract {

    private StockContract() {
    }

    public static final class StockEntry implements BaseColumns {

        public static final String TABLE_NAME = "stock";

        public static final String _ID = BaseColumns._ID;
        public static final String COLUMN_NAME = "name";
        public static final String COLUMN_PRICE = "price";
        ...

        public static final String CREATE_TABLE_STOCK = "CREATE TABLE " +
                StockContract.StockEntry.TABLE_NAME + "(" +
                StockContract.StockEntry._ID + " INTEGER PRIMARY KEY AUTOINCREMENT," +
                StockContract.StockEntry.COLUMN_NAME + " TEXT NOT NULL," +
                StockContract.StockEntry.COLUMN_PRICE + " TEXT NOT NULL," +
                ...
                ");";
    }
}
```

## Create the database with the SQL Helper
- DB Helper generates CRUD statements
- CRUD: create, read, update and delete actions
- Database versions start from 1

### Create
- Create a DbHelper class that extends SQLiteOpenHelper. In this example it is called `InventoryDbHelper`
- Declare the database name and the version number
- Create a constructor
- Override the two mandatory methods that the DbHelper needs: `onCreate` and `onUpgrade`

```
public class InventoryDbHelper extends SQLiteOpenHelper {

    public final static String DB_NAME = "inventory.db";
    public final static int DB_VERSION = 1;
    public final static String LOG_TAG = InventoryDbHelper.class.getCanonicalName();

    public InventoryDbHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(StockContract.StockEntry.CREATE_TABLE_STOCK);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
      //TODO
    }
}
```

- Add a method to insert items to database, e.g. `insertItem()`
- In this example, `insertItem()` method accepts one parameter, which is a custom class called `StockItem`

The `StockItem` class looks like:
```
public class StockItem {

    private final String productName;
    private final String price;
    private final int quantity;
    private final String supplierName;
    private final String supplierPhone;
    private final String supplierEmail;
    private final String image;

    // constructor

    // getters and setters
}
```

- In `InventoryDbHelper` add the `insertItem` method.  
- Get writable database
- Create ContentValues where to add key-value pairs
- insert always returns the inserted row ID or -1 if error

```
public class InventoryDbHelper extends SQLiteOpenHelper {

    // DbHelper previous code

    public void insertItem(StockItem item) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put(StockContract.StockEntry.COLUMN_NAME, item.getProductName());
        values.put(StockContract.StockEntry.COLUMN_PRICE, item.getPrice());
        long id = db.insert(StockContract.StockEntry.TABLE_NAME, null, values);
    }
}
```

### Read
- In `InventoryDbHelper` add the read method
- It returns a cursor

First define a method that read the whole table and returns a cursor that contains all the items:

```
public class InventoryDbHelper extends SQLiteOpenHelper {

    // DbHelper previous code

    public Cursor readStock() {
        SQLiteDatabase db = getReadableDatabase();
        String[] projection = {
                StockContract.StockEntry._ID,
                StockContract.StockEntry.COLUMN_NAME,
                StockContract.StockEntry.COLUMN_PRICE
        };
        Cursor cursor = db.query(
                StockContract.StockEntry.TABLE_NAME,
                projection,
                null,
                null,
                null,
                null,
                null
        );
        return cursor;
    }
}
```

Second define a read method that reads only one item based on the ID and returns it:

```
public class InventoryDbHelper extends SQLiteOpenHelper {

    // DbHelper previous code

    public Cursor readItem(long itemId) {
        SQLiteDatabase db = getReadableDatabase();
        String[] projection = {
                StockContract.StockEntry._ID,
                StockContract.StockEntry.COLUMN_NAME,
                StockContract.StockEntry.COLUMN_PRICE
        };
        String selection = StockContract.StockEntry._ID + "=?";
        String[] selectionArgs = new String[] { String.valueOf(itemId) };

        Cursor cursor = db.query(
                StockContract.StockEntry.TABLE_NAME,
                projection,
                selection,
                selectionArgs,
                null,
                null,
                null
        );
        return cursor;
    }
}
```
