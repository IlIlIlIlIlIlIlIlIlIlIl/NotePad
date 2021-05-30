# 项目描述NotePad

## 1.项目结构

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/9.png?raw=true)

### 主要类

1. NotesList类 —— 应用程序的入口，笔记本的首页面会显示笔记的列表
2. NoteEditor类 —— 编辑笔记内容的Activity
3. TitleEditor类 —— 编辑笔记标题的Activity
4. NotePadProvider类 —— 这是笔记本应用的ContentProvider，也是整个应用的关键所在
5. NotesLiveFolder ContentProvider的LiveFolder（实时文件夹），这个功能在Android API 14后被废弃，不再支持。因此代码中所有涉及LiveFolder的内容将不再阐述

## 2.添加时间戳

### 为时间戳设置布局

在`notelist_item.xml`布局文件中修改并添加`TextView`

```
<RelativeLayout android:layout_height="match_parent"
    android:layout_width="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@android:id/text1"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/listPreferredItemHeight"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:gravity="center_vertical"
        android:paddingLeft="5dip"
        android:singleLine="true"
        />


    <TextView
        android:id="@+id/time"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAppearance="?android:attr/textAppearanceSmall"
        android:paddingLeft="5dip" />
```

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/1.png?raw=true)

### 在NoteList.java的PROJECTION中加入我们要显示的时间

```
private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1z
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, // time
    };
```

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/2.png?raw=true)

### 在NoteList.java中添加要装配的数据

```
// The names of the cursor columns to display in the view, initialized to the title column
        String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,
                NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE } ;

        // The view IDs that will display the cursor columns, initialized to the TextView in
        // noteslist_item.xml
        int[] viewIDs = { android.R.id.text1,R.id.time };
```

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/3.png?raw=true)

### 导入所需要的包

```
import java.text.SimpleDateFormat;
import java.util.Date;
```

### 在NotePadProvider.java中，我们修改这一部分

```
// Gets the current system time in milliseconds
        Long now = Long.valueOf(System.currentTimeMillis());
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String time = sdf.format(new Date(Long.parseLong(String.valueOf(now))));
```

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/6.png?raw=true)

再将put的参数由now改成我们定义的time 在NoteEditor.java中，我们在update方法中也修改时间的类型

```
// Sets up a map to contain values to be updated in the provider.
        ContentValues values = new ContentValues();
        Long now = Long.valueOf(System.currentTimeMillis());
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String time = sdf.format(new Date(Long.parseLong(String.valueOf(now))));
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, time);
```

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/7.png?raw=true)

### 添加时间戳后运行效果

![image](https://github.com/IlIlIlIlIlIlIlIlIlIlIl/Notepad/blob/main/screenshot/8.png?raw=true)
