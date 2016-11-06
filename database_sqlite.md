# Create a local database with SQLiteOpenHelper

## Requirements
- Install SQLite https://sqlite.org/download.html

## Create a Contract class
- To define the database schema
- To declare the database name and version, tables and columns names

```
public class StockContract {

    public StockContract() {
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
- Database versions start from 1


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

    }
}
```
