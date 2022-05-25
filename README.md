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

```kotlin
@HiltViewModel
class FirstBootSectionDetailsViewModel
@Inject
constructor(
    private val getSectionInfo: GetSectionInfoUseCase,
    private val popBack: PopDetailsBackUseCase,
    private val navigateToAuth: NavigateToAuthUseCase
) : ViewModel() {
    private val _sectionsListViewState: MutableState<SectionDetailsViewState> = mutableStateOf(
        SectionDetailsViewState.Loading)
    val sectionsListViewState: State<SectionDetailsViewState>
        get() = _sectionsListViewState

    fun fetchSectionInfo(id: Int){
        viewModelScope.launch {
            _sectionsListViewState.value = SectionDetailsViewState.Loading

            try{
                getSectionInfo(id).let {
                    _sectionsListViewState.value = SectionDetailsViewState.PresentInfo.Unauthorized(
                        title = it.sectionName,
                        description = it.description,
                        price = it.price,
                        onAuthClick = navigateToAuth::invoke
                    )
                }
            } catch (ex: Exception) {
                _sectionsListViewState.value = SectionDetailsViewState.Error(
                    message = ex.message ?: "Неописанная ошибка: ${ex::class.java.simpleName}"
                )
            }
        }
    }

    fun backClicked() = popBack()
}
```

```kotlin
class GetSectionInfoUseCase
@Inject
constructor(
    private val sectionsRepository: ISectionsRepository
) {
    suspend operator fun invoke(id: Int): SectionDetailsResponse {
        return sectionsRepository.getSectionInfo(id)
    }
}
```

```kotlin
class PopDetailsBackUseCase
@Inject
constructor(
    private val firstBootNavProvider: FirstBootNavProvider
) {
    operator fun invoke() {
        firstBootNavProvider.navigateTo(FirstBootDestinations.PopBack)
    }
}
```

```kotlin
class NavigateToAuthUseCase
@Inject
constructor(
    private val coreNavProvider: CoreNavProvider
) {
    operator fun invoke() {
        coreNavProvider.navigateTo(CoreDestinations.Authorization)
    }
}
```
