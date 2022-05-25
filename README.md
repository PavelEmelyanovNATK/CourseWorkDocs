#### Императивный подход
Xml:
```xml
  <TextView
      android:id="@+id/tv_name"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"/>
```

Kotlin:
```kotlin
  class MainActivity : AppCompatActivity() {
    lateinit var textView: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        textView = findViewById(R.id.tv_name)

        textView.text = "My Text"
    }
  }
```

#### Декларативный подход

Kotlin:
```kotlin
  class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Text("My Text")
        }
    }
  }
```
