class VitalsViewModel(application: Application) : AndroidViewModel(application) {
    private val repository: VitalsRepository
    private val _vitalsList = MutableStateFlow<List<VitalsEntity>>(emptyList())
    val vitalsList: StateFlow<List<VitalsEntity>> = _vitalsList.asStateFlow()

    init {
        val dao = VitalsDatabase.getDatabase(application).vitalsDao()
        repository = VitalsRepository(dao)
        viewModelScope.launch {
            repository.allVitals.collect { _vitalsList.value = it }
        }
    }

    fun addVitals(vitals: VitalsEntity) = viewModelScope.launch {
        repository.insertVitals(vitals)
    }
}
